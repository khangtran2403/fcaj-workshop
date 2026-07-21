---
title: "Bản đề xuất"
date: 2026-07-12 
weight: 2 
chapter: false 
pre: " <b> 2. </b> "
---
# Tự động số hóa hóa đơn với Amazon Bedrock Data Automation và AWS

# Xây dựng pipeline serverless, event-driven biến PDF hóa đơn giấy thành dữ liệu có cấu trúc, sẵn sàng tích hợp vào hệ thống kế toán và ERP.

# 1.Bài toán: Hóa đơn giấy vẫn là điểm nghẽn của doanh nghiệp

Dù hóa đơn điện tử đang được thúc đẩy mạnh, thực tế ở nhiều doanh nghiệp Việt Nam,đặc biệt trong các chuỗi cung ứng, bán lẻ, và xây dựng vẫn còn một lượng lớn hóa đơn giấy, PDF scan, hoặc ảnh chụp bằng điện thoại. Đội kế toán phải nhập liệu thủ công: mã số thuế, ngày xuất, tổng tiền, danh sách hàng hóa,công việc lặp đi lặp lại, dễ sai sót và không thể scale.

Bài toán đặt ra rõ ràng: làm thế nào trích xuất thông tin có cấu trúc từ hóa đơn phi cấu trúc ở quy mô lớn, mà không cần xây mô hình ML riêng hay hard-code parser cho từng mẫu hóa đơn khác nhau?

# 2.Tổng quan giải pháp

Kiến trúc đề xuất sử dụng pipeline serverless được điều phối bởi AWS Step Functions trên AWS không có server cần quản lý, không cần polling, không cần lịch chạy định kỳ. Mỗi hóa đơn tải lên sẽ tự động kích hoạt một luồng xử lý (workflow) hoàn chỉnh.

Các dịch vụ AWS được sử dụng

Amazon Cognito (Xác thực người dùng)

CloudFront (CDN phân phối Frontend)

AWS WAF (Bảo mật Web cho CloudFront và API Gateway)

AWS IAM (Quản lý quyền truy cập)

S3 Static Website (Hosting giao diện)

API Gateway (Cổng API)

AWS Lambda (Xử lý logic cấp phát URL và Validation)

Amazon EventBridge (Bắt sự kiện từ S3)

AWS Step Functions (Điều phối Workflow)

Amazon Bedrock Data Automation (Trí tuệ nhân tạo trích xuất dữ liệu)

Amazon S3 (Lưu trữ file PDF đầu vào, file JSON đầu ra và file lỗi)

DynamoDB (Cơ sở dữ liệu lưu kết quả)

Amazon SQS (Hàng đợi cho các hóa đơn cần con người duyệt)

CloudWatch & CloudWatch Logs (Giám sát và ghi log)

Amazon SNS (Gửi thông báo)

## Kiến trúc chi tiết

Pipeline chạy qua ba giai đoạn, mỗi giai đoạn xây trên nền của giai đoạn trước.

Giai đoạn 1: Khởi tạo tài nguyên

Tài nguyên được tạo tay trên console: S3 buckets (input, output, error), các Lambda functions, Step Functions State Machine, IAM roles với least-privilege permissions, KMS keys, CloudWatch log groups, và data store đích.

Giai đoạn 2: Điều phối với Step Functions và Xử lý dữ liệu

Đây là trái tim của kiến trúc. Quá trình tải lên được bảo mật và luồng xử lý được quản lý chặt chẽ bởi Step Functions:

Bước 1.Xác thực và Lấy URL tải lên: Người dùng (hoặc hệ thống đối tác) xác thực qua Amazon Cognito. Sau đó, gọi API Gateway để yêu cầu một S3 Presigned URL. AWS Lambda sẽ tạo và trả về URL tạm thời này, giúp ứng dụng client tải trực tiếp file PDF lên S3 Input Bucket một cách an toàn mà không bị giới hạn payload của API Gateway.

Bước 2.Kích hoạt luồng xử lý: Khi file PDF được tải lên S3 thành công, S3 Event Notification gửi sự kiện tới Amazon EventBridge. EventBridge lập tức kích hoạt (trigger) AWS Step Functions State Machine.

Bước 3.Trích xuất thông tin (AI): Bước đầu tiên trong Step Functions là gọi API của Amazon Bedrock Data Automation với một blueprint định nghĩa sẵn. BDA tự hiểu cấu trúc tài liệu. Nó trích xuất các trường như: Tên công ty, mã số thuế, số hóa đơn, ngày phát hành, chi tiết hàng hóa, tổng tiền... Mỗi trường đi kèm với một confidence score (điểm tin cậy). Hoàn toàn không cần API Key, hệ thống kết nối nội bộ thông qua quyền IAM.

Bước 4.Lưu trữ JSON thô: Step Functions đợi BDA xử lý xong và đảm bảo kết quả JSON gốc được ghi vào S3 Output Bucket. Bước này cực kỳ quan trọng cho việc audit và lưu vết dữ liệu (Data lineage) sau này.

Bước 5.Validation và Chuẩn hóa: Step Functions gọi tiếp Invoice Processor Lambda. Hàm này đọc JSON từ S3, chuẩn hóa định dạng (ngày tháng, tiền tệ) và kiểm tra confidence score tổng thể.

Bước 6.Rẽ nhánh (Choice State) & Lưu trữ: Dựa trên điểm tin cậy do Lambda tính toán, Step Functions sẽ rẽ nhánh:

Confidence cao (>= 0.9): Ghi thẳng dữ liệu có cấu trúc vào DynamoDB.

Confidence thấp (< 0.9): Đẩy thông báo kèm dữ liệu vào Amazon SQS (Human Review Queue) để kế toán viên kiểm tra lại.

File lỗi/Không thể đọc: Đưa file gốc vào S3 Error/Quarantine Bucket.

Giai đoạn 3: Truy vấn và phân tích

Sau khi dữ liệu được lưu vào DynamoDB, nó ngay lập tức có thể query được thông qua API hoặc dashboard nội bộ. Các truy vấn điển hình: lọc theo nhà cung cấp, tổng hợp VAT theo tháng, kiểm tra hóa đơn chưa đối soát, phát hiện trùng lặp số hóa đơn.

# 3.Cách các service kết nối với nhau

Hiểu rõ cách các service giao tiếp là chìa khóa để tùy chỉnh và mở rộng kiến trúc này.

## Step Functions là Orchestrator

Thay vì để các Lambda gọi chéo nhau rất khó debug, Step Functions cung cấp một giao diện trực quan hiển thị toàn bộ vòng đời của một hóa đơn. Bạn có thể nhìn vào AWS Console và biết chính xác hóa đơn bị kẹt ở đâu (đang chờ AI xử lý, hay đã rơi vào hàng đợi SQS do mờ). Step Functions cũng hỗ trợ xử lý lỗi (try/catch) và retry tự động khi gọi các API bên ngoài.

## S3 Presigned URL và WAF bảo vệ đầu vào

Việc sử dụng Presigned URL là Best Practice của AWS để xử lý file lớn. Nó giảm tải hoàn toàn cho lớp Backend (API Gateway/Lambda). Đồng thời, AWS WAF được gắn trực tiếp vào CloudFront (bảo vệ Frontend) và API Gateway (chặn các request xin URL trái phép, chống DDoS, SQLi).

## Bedrock Data Automation là lớp trí tuệ

BDA là lý do kiến trúc này không cần custom ML model. Khi Step Functions kích hoạt extraction job, BDA lấy PDF từ S3 và áp dụng invoice blueprint một schema JSON định nghĩa các trường cần trích xuất. Service này hiểu cấu trúc tài liệu mà không cần template hay training data.

Ví dụ output thực tế:

{
    *vendor*: {
    *name*: *Công ty **TNHH** **ABC** Technology*,
    *tax_id*: *0123456789*,
    *confidence*: 0.97
    },
    *invoice_number*: ***INV**-**2025**-**00431***,
    *total_amount*: **5500000**,
    *line_items*: [
    {
    *description*: *Dịch vụ tư vấn phần mềm*,
    *quantity*: 10,
    *unit_price*: **500000**,
    *total*: **5000000**,
    *confidence*: 0.94
    }
    ]
}

## IAM là lớp bảo mật kết dính

Mọi giao tiếp được thực thi qua IAM roles với least-privilege permissions. Không có hardcoded credentials hay API Keys. Step Functions chỉ có quyền gọi
bedrock:InvokeDataAutomationAsync và gọi Lambda cụ thể. Lambda chỉ có quyền ghi vào DynamoDB bảng Invoices.

# 4.Bảo mật và tuân thủ

Hóa đơn chứa thông tin tài chính nhạy cảm. Kiến trúc này tích hợp các lớp bảo mật ngay từ đầu:

Encryption at rest: AWS KMS mã hóa toàn bộ dữ liệu lưu trong S3 và DynamoDB với customer managed keys.

Encryption in transit: Mọi giao tiếp service-to-service đều qua TLS 1.2+. Không có traffic nào đi qua public internet không an toàn.

Bảo vệ biên (Edge Protection): AWS WAF chặn các lưu lượng độc hại ngay từ biên mạng (CloudFront/API Gateway).

Audit trail: CloudTrail ghi lại mọi hoạt động gọi API nội bộ của các dịch vụ. S3 Output Bucket đóng vai trò là hộp đen lưu trữ nguyên bản mọi kết quả AI phân tích để đối chiếu pháp lý khi cần.

# 5.Mở rộng kiến trúc

Sau khi pipeline cơ bản vận hành ổn định, có nhiều hướng mở rộng:

Tích hợp ERP / kế toán: Lambda processor gọi API của SAP, Odoo, MISA,thay vì chỉ lưu vào DynamoDB.

Review workflow chuyên sâu: Tạo một trang web nội bộ (React/Vue) cho kế toán viên. Trang web này kéo các message từ hàng đợi SQS để duyệt các hóa đơn có confidence score thấp.

Phát hiện bất thường: Thêm một task trong Step Functions để truy vấn DynamoDB so sánh hóa đơn mới với lịch sử phát hiện số hóa đơn trùng lặp trước khi quyết định lưu.

# 6.Kết luận

Kiến trúc Serverless với Step Functions này giải quyết bài toán số hóa hóa đơn một cách toàn diện, vững chắc và chuyên nghiệp nhất:

Tối ưu luồng dữ liệu (S3 Presigned URL): Khắc phục triệt để điểm yếu thắt cổ chai khi tải file lớn của kiến trúc API truyền thống.

Điều phối mạnh mẽ (Step Functions): Biến quy trình từ một chuỗi các sự kiện rời rạc thành một cỗ máy trạng thái (state machine) có khả năng theo dõi trực quan, rẽ nhánh thông minh và quản lý lỗi tuyệt vời.

Không cần ML riêng (Bedrock): Rút ngắn thời gian triển khai từ nhiều tháng (thu thập data, train mô hình OCR) xuống còn vài ngày bằng cách tận dụng sức mạnh Generative AI có sẵn của AWS.

Điểm cốt lõi: Khi hóa đơn không còn nằm trong file PDF hay ngăn kéo, mà trở thành dữ liệu có cấu trúc trong hệ thống, toàn bộ tổ chức có thêm khả năng ra quyết định dựa trên data thực một cách tự động và ở quy mô không giới hạn.