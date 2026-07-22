---
title: "Blog 2"
date: 2026-07-15
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# Xây dựng quy trình xử lý dữ liệu linh hoạt với Specification-Driven Composition trên AWS
Trong nhiều doanh nghiệp hiện nay, dữ liệu không chỉ đến từ một hệ thống duy nhất mà còn được thu thập từ nhiều nguồn như cơ sở dữ liệu, API, hệ thống ERP, CRM hay các dịch vụ SaaS. Khi nhu cầu tích hợp dữ liệu ngày càng đa dạng, việc xây dựng các workflow cố định bằng mã nguồn (hard-coded workflow) trở thành một điểm nghẽn: mỗi khi có thêm nguồn dữ liệu hoặc thay đổi quy trình, đội ngũ phát triển phải sửa code, kiểm thử và triển khai lại hệ thống.
Để giải quyết vấn đề này, AWS Architecture Blog giới thiệu mô hình Specification-Driven Composition, một phương pháp cho phép xây dựng các quy trình xử lý dữ liệu dựa trên đặc tả (specification) thay vì viết logic cố định trong mã nguồn. Kiến trúc này giúp workflow trở nên linh hoạt, dễ mở rộng và giảm đáng kể chi phí bảo trì.
## Bài toán
Trong các hệ thống dữ liệu doanh nghiệp, một yêu cầu nghiệp vụ thường phải:
1. Truy xuất dữ liệu từ nhiều nguồn khác nhau;
2. Áp dụng nhiều bước chuyển đổi;
3. Gọi nhiều API hoặc dịch vụ;
4. Tổng hợp kết quả trước khi trả về.
Nếu workflow được viết trực tiếp trong mã nguồn, mỗi thay đổi về nghiệp vụ đều yêu cầu cập nhật code, kiểm thử và triển khai lại. Điều này làm giảm khả năng thích ứng của hệ thống khi số lượng workflow và nguồn dữ liệu ngày càng tăng.
## Ý tưởng chính
Kiến trúc đề xuất sử dụng một specification như nguồn mô tả duy nhất của workflow (single source of truth).
Specification định nghĩa:
1. Các tác vụ cần thực hiện;
2. Dữ liệu đầu vào và đầu ra;
3. Quan hệ phụ thuộc giữa các tác vụ;
4. Các điều kiện để tiếp tục thực hiện;
5. Các bước có thể chạy song song.
Workflow engine chỉ có nhiệm vụ đọc specification, xây dựng đồ thị thực thi và điều phối các tác vụ tương ứng. Điều này giúp logic nghiệp vụ không còn bị gắn chặt với mã nguồn của bộ điều phối.
## Thành phần của kiến trúc
Kiến trúc bao gồm ba nhóm thành phần chính:
Specification Repository lưu trữ các định nghĩa workflow.
Workflow Engine phân tích specification và xây dựng kế hoạch thực thi.
Task Executors thực hiện từng bước xử lý như truy vấn dữ liệu, gọi API hoặc xử lý dữ liệu.
Mỗi task là một đơn vị độc lập và có thể được tái sử dụng trong nhiều workflow khác nhau.
Cơ chế thực thi
Khi có một yêu cầu mới:
**Hệ thống xác định specification phù hợp.**
**Workflow engine đọc specification.**
**Engine phân tích quan hệ phụ thuộc giữa các task.**
**Các task độc lập được chạy song song nếu có thể.**
**Kết quả của các task được truyền cho các bước tiếp theo.**
**Sau khi toàn bộ workflow hoàn thành, hệ thống tổng hợp kết quả và trả về cho ứng dụng.**

Nhờ vậy, workflow được quyết định tại thời điểm chạy (runtime) thay vì được cố định trong mã nguồn.
## Khả năng thực thi song song
Một ưu điểm quan trọng là workflow engine có thể nhận biết các tác vụ không phụ thuộc lẫn nhau và thực hiện chúng đồng thời.
Ví dụ, nếu một báo cáo cần lấy thông tin khách hàng, lịch sử giao dịch và điểm thưởng từ ba hệ thống khác nhau, engine có thể gửi cả ba yêu cầu cùng lúc thay vì thực hiện tuần tự. Điều này giúp giảm đáng kể thời gian xử lý của toàn bộ workflow.
## Lợi ích
Bài viết nhấn mạnh một số lợi ích của cách tiếp cận này:
Linh hoạt: Có thể thay đổi workflow bằng cách cập nhật specification thay vì sửa mã nguồn.
Khả năng mở rộng: Dễ bổ sung các task hoặc nguồn dữ liệu mới.
Tái sử dụng: Một task có thể được dùng trong nhiều workflow khác nhau.
Hiệu năng: Hỗ trợ thực thi song song các tác vụ độc lập.
Bảo trì đơn giản: Logic nghiệp vụ được mô tả rõ ràng trong specification, giúp dễ theo dõi và cập nhật.
## Trường hợp sử dụng
Theo bài viết, mẫu kiến trúc này đặc biệt phù hợp với:
1. Nền tảng dữ liệu doanh nghiệp;
2. Hệ thống ETL và ELT;
3. Các ứng dụng tích hợp nhiều API;
4. Nền tảng phân tích dữ liệu;
5. Các workflow AI/ML;
Những hệ thống mà quy trình xử lý thường xuyên thay đổi theo yêu cầu kinh doanh.
Kết luận
Thay vì coi workflow là mã nguồn, hãy coi workflow là dữ liệu. Bằng cách mô tả quy trình thông qua specification và để một workflow engine chịu trách nhiệm thực thi, doanh nghiệp có thể xây dựng các hệ thống xử lý dữ liệu linh hoạt hơn, dễ mở rộng hơn và ít phụ thuộc vào việc sửa đổi mã nguồn mỗi khi yêu cầu nghiệp vụ thay đổi.
Bài viết gốc : https://aws.amazon.com/blogs/architecture/specification-driven-composition-for-flexible-data-workflows/


[Link](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2211158986315728)

![endpoint](/images/3.Blog2.1.png)
![endpoint](/images/3.Blog2.2.png)



