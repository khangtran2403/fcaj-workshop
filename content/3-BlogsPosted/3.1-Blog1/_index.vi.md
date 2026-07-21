---
title: "Blog 1"
date: 2026-06-30
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Ngăn chặn rò rỉ dữ liệu trong môi trường Machine Learning với Amazon SageMaker AI

Trong quá trình xây dựng các giải pháp Machine Learning sử dụng dữ liệu nhạy cảm, một trong những thách thức lớn nhất là làm sao để ngăn chặn dữ liệu bị đưa ra ngoài hệ thống (data exfiltration) mà vẫn đảm bảo các nhà khoa học dữ liệu (Data Scientist) có thể làm việc hiệu quả.
Bài viết này giới thiệu cách một doanh nghiệp fintech giả định có tên iBusiness đã triển khai kiến trúc bảo mật nhiều lớp dựa trên Amazon SageMaker AI, Amazon WorkSpaces Secure Browser và các thành phần mạng của AWS để bảo vệ dữ liệu nhạy cảm trong quá trình phát triển mô hình Machine Learning.
# Thách thức
Trước đây, khi cần xử lý dữ liệu quan trọng, iBusiness sử dụng các môi trường nội bộ được cô lập hoàn toàn (air-gapped environment). Tuy nhiên, khi đội ngũ Data Scientist ngày càng mở rộng và xu hướng làm việc từ xa trở nên phổ biến, mô hình này bộc lộ nhiều hạn chế:
Chi phí vận hành cao do mỗi người dùng cần một máy ảo riêng.
Việc cập nhật thư viện Machine Learning, vá lỗi và bảo trì hệ thống tốn nhiều thời gian.
Khó mở rộng khi số lượng dự án và người dùng tăng lên.
Để giải quyết vấn đề này, doanh nghiệp chuyển sang sử dụng Amazon SageMaker Studio – môi trường phát triển Machine Learning được AWS quản lý hoàn toàn. SageMaker Studio giúp giảm đáng kể công sức quản lý hạ tầng, đồng thời tích hợp với các dịch vụ như AWS Lake Formation và Amazon Athena để truy cập dữ liệu thuận tiện hơn.
# Kiến trúc bảo mật ba lớp
Để giảm thiểu nguy cơ rò rỉ dữ liệu, iBusiness xây dựng một kiến trúc gồm ba lớp bảo vệ:

    
  Kiểm soát điểm truy cập bằng WorkSpaces Secure Browser.
  Hạn chế hoạt động của trình duyệt và ngăn truy cập sang các tài khoản AWS khác.
  Cô lập hoàn toàn môi trường Amazon SageMaker AI khỏi Internet.

Mỗi lớp đảm nhiệm một vai trò riêng, tạo thành nhiều tầng phòng vệ nếu một cơ chế bảo mật bị vượt qua.
# Lớp 1: Kiểm soát truy cập bằng Amazon WorkSpaces Secure Browser
Thay vì cấp cho mỗi người dùng một máy tính ảo đầy đủ, iBusiness sử dụng Amazon WorkSpaces Secure Browser – trình duyệt Chromium được AWS quản lý.
Secure Browser được triển khai trong một VPC riêng và chỉ cho phép truy cập tới môi trường Machine Learning thông qua địa chỉ mạng đã được xác thực. Đồng thời, doanh nghiệp áp dụng các chính sách IAM để chỉ chấp nhận các yêu cầu đến từ Secure Browser hoặc từ địa chỉ IP NAT Gateway được chỉ định.
Ngoài ra, Secure Browser được cấu hình để:
Vô hiệu hóa tải tệp xuống máy người dùng.
Chặn tải tệp từ máy cá nhân lên hệ thống.
Tắt chức năng sao chép và dán (Clipboard).
Không cho phép in tài liệu.
Nhờ đó, người dùng vẫn có thể làm việc trên SageMaker nhưng rất khó sao chép dữ liệu ra khỏi môi trường bảo mật.
# Lớp 2: Giới hạn hoạt động của trình duyệt
Sau khi kiểm soát điểm truy cập, doanh nghiệp tiếp tục giới hạn những gì người dùng có thể làm bên trong Secure Browser.
Chỉ cho phép truy cập các miền được phê duyệt
Secure Browser sử dụng danh sách URL được phép (Allow List). Người dùng chỉ có thể truy cập:

*.aws.amazon.com

Các tên miền liên quan đến Amazon SageMaker AI

Tất cả các website khác như email, Google Drive, Dropbox hoặc các dịch vụ lưu trữ trực tuyến đều bị chặn. Điều này giúp ngăn chặn việc tải dữ liệu lên các nền tảng bên ngoài.
Ngăn chặn chuyển dữ liệu sang tài khoản AWS khác
Để đảm bảo người dùng không thể đăng nhập vào một tài khoản AWS khác và sao chép dữ liệu, iBusiness triển khai:
VPC Endpoint cho AWS Management Console.
VPC Endpoint cho AWS IAM Identity Center.
Lưu lượng truy cập tới các dịch vụ này luôn đi trong mạng riêng của AWS và không đi qua Internet.
Bên cạnh đó, doanh nghiệp cấu hình Amazon Route 53 Private Hosted Zone để chuyển hướng các tên miền của AWS Console tới VPC Endpoint nội bộ thay vì endpoint công khai.
Cuối cùng, Amazon Route 53 Resolver DNS Firewall được sử dụng để chỉ cho phép phân giải DNS tới các tên miền AWS cần thiết. Mọi truy vấn tới các miền không nằm trong danh sách cho phép sẽ bị từ chối.
Doanh nghiệp cũng bổ sung các chính sách IAM nhằm:
Chỉ cho phép thao tác khi yêu cầu xuất phát từ VPC Endpoint hợp lệ.
Từ chối các hành động hướng tới tài nguyên thuộc tài khoản AWS khác, ngoại trừ một số tài khoản quản trị đặc biệt.
# Lớp 3: Bảo vệ môi trường Amazon SageMaker AI
Lớp phòng vệ cuối cùng tập trung vào chính môi trường SageMaker AI.
Mặc dù SageMaker Studio cung cấp Terminal và IDE để lập trình, nhưng các tính năng này cũng có thể bị lợi dụng để gửi dữ liệu ra ngoài nếu môi trường có kết nối Internet.
Để loại bỏ rủi ro này, iBusiness cấu hình:
Không sử dụng Internet Gateway.
Không triển khai NAT Gateway cho VPC của SageMaker AI.
Chỉ tạo VPC Endpoint tới các dịch vụ AWS cần thiết.
Nhờ đó, SageMaker vẫn có thể truy cập các dịch vụ nội bộ của AWS để phục vụ quá trình huấn luyện và triển khai mô hình, nhưng hoàn toàn không thể gửi dữ liệu trực tiếp ra Internet.
Ngoài ra, các VPC Endpoint Policy cũng được giới hạn để chỉ cho phép truy cập các tài nguyên thuộc chính tổ chức. Ví dụ, quyền ghi dữ liệu vào Amazon S3 chỉ được cấp cho các bucket do doanh nghiệp quản lý.

Kết quả đạt được
 
  Sau khi triển khai kiến trúc này, iBusiness ghi nhận nhiều cải thiện đáng kể:
  Chi phí giảm khoảng 80%, từ hơn 40 USD/người/tháng xuống còn khoảng 7 USD/người/tháng nhờ sử dụng Amazon WorkSpaces Secure Browser thay cho các môi trường VDI truyền thống.
  Thời gian cấp phát môi trường làm việc giảm từ khoảng hai ngày xuống chỉ còn vài phút.
  Không còn phải duy trì và cập nhật các máy tính ảo riêng cho từng Data Scientist.
  Người dùng vẫn có đầy đủ công cụ để phát triển mô hình Machine Learning trong khi dữ liệu nhạy cảm luôn được bảo vệ bởi nhiều lớp kiểm soát.

# Kết luận
Việc bảo vệ dữ liệu trong môi trường Machine Learning không chỉ dựa vào phân quyền người dùng mà cần kết hợp nhiều lớp kiểm soát từ trình duyệt, mạng cho đến môi trường phát triển.
Kiến trúc được AWS giới thiệu cho thấy việc kết hợp Amazon WorkSpaces Secure Browser, Amazon SageMaker AI, VPC Endpoint, Amazon Route 53 Resolver DNS Firewall và AWS IAM có thể giúp doanh nghiệp vừa đảm bảo an toàn cho dữ liệu nhạy cảm, vừa duy trì năng suất làm việc của đội ngũ Data Scientist.
Thay vì phụ thuộc vào các môi trường cô lập truyền thống có chi phí cao, mô hình này mang lại một giải pháp linh hoạt, dễ mở rộng và phù hợp với các tổ chức đang phát triển các ứng dụng Machine Learning trên nền tảng AWS.
Bài viết gốc : https://aws.amazon.com/blogs/architecture/preventing-data-exfiltration-in-machine-learning-environments-with-amazon-sagemaker-ai/

Link : https://www.facebook.com/groups/awsstudygroupfcj/permalink/2200376417393985

![endpoint](/images/3-Blog/3.Blog1.png)

