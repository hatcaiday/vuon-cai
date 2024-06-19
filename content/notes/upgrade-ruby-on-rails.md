---
title: Cập nhật phiên bản Ruby on Rails
tags:
  - technical
---
## Bối cảnh:
Dự án hiện tại đang sử dụng Ruby 2.7 và Rails 6.0, phiên bản Ruby đã chính thức kết thúc vòng đời (end-of-life) vào ngày 31/3/2023 và Rails 6.0 đã chính thức kết thúc vòng đời vào ngày 1/6/2023. Việc tiếp tục sử dụng Ruby 2.7 và Rails 6.0 tiềm ẩn nhiều rủi ro nghiêm trọng, ảnh hưởng đến tính bảo mật, hiệu suất và khả năng duy trì dự án.
Dưới đây là một số vấn đề chính nếu không cập nhật phiên bản:

## Vấn đề:

**1. Nguy cơ bảo mật:**

Ruby 2.7 và Rails 6.0 không còn nhận được bản vá lỗi bảo mật từ cộng đồng Ruby / Rails. Điều này khiến dự án dễ bị tấn công bởi hacker và phần mềm độc hại, dẫn đến rò rỉ dữ liệu, mất thông tin nhạy cảm và thậm chí là vi phạm hệ thống.

**2. Hiệu suất kém, lỗi và sự cố**

Ruby 2.7, Rails 6.0 không được tối ưu hóa và cải tiến hiệu suất hay thực hiện giải quyết các lỗi, sự cố như các phiên bản mới hơn. Việc sử dụng Ruby 2.7, Rails 6.0 có thể khiến ứng dụng của bạn chạy chậm chạp, lãng phí tài nguyên, lỗi hệ thống và ảnh hưởng đến trải nghiệm người dùng.

**3. Khó khăn trong bảo trì:**

Việc tìm kiếm hỗ trợ cho Ruby 2.7, Rails 6.0 trở nên khó khăn hơn khi cộng đồng Ruby tập trung vào các phiên bản mới hơn. Hoặc khi muốn thực hiện tính năng mới mà ở phiên bản mới hỗ trợ thực hiện dễ dàng hơn thay vì phải tốn nhiều thời gian.

## Giải pháp:
Để giải quyết những rủi ro tiềm ẩn và đảm bảo an toàn cho dự án, việc nâng cấp Ruby / Rails lên phiên bản mới nhất là điều cần thiết. Quá trình nâng cấp có thể bao gồm:

- **Lựa chọn phiên bản Ruby / Rails phù hợp**: Xác định phiên bản Ruby / Rails ổn định và phù hợp nhất với nhu cầu của dự án.
- **Đánh giá impact**: Phân tích các thay đổi trong phiên bản Ruby / Rails mới và đánh giá ảnh hưởng đến dự án qua việc kiểm tra changelog (lưu ý kỹ với những changelog deprecation và delete)
- **Unit test**: Thực hiện viết Unit test để bao phủ code nhiều nhất có thể ( > 80% coverage logic code)
- **Thực hiện nâng cấp**: Thực hiện nâng cấp phiên phải theo impact đã được đánh giá và Unit test. Thực hiện nâng cấp theo minor version để giảm thiểu rủi ro.
- **Cập nhật gems**: Cập nhật các gems phụ thuộc tương thích với phiên bản Ruby / Rails mới.
- **Thực hiện testing**: Triển khai phiên bản Ruby / Rails mới lên môi trường test để thực hiện test, so sánh môi trường cũ và mới, đảm bảo ổn định sau đó thực hiện triển khai trên môi trường production một cách cẩn thận và theo dõi hiệu suất sau khi nâng cấp.

Nâng cấp phiên bản Ruby / Rails là một việc làm quan trọng để bảo vệ dự án khỏi các mối đe dọa bảo mật, cải thiện hiệu suất và đảm bảo khả năng duy trì lâu dài. Vậy nên việc dành thời gian và nguồn lực để lên kế hoạch và thực hiện nâng cấp Ruby / Rails một cách hiệu quả, đảm bảo sự an toàn và thành công cho dự án là cần thiết.

[Hướng dẫn nâng cấp phiên bản Ruby](/notes/upgrade-ruby-version)
[Hướng dẫn nâng cấp phiên bản Rails](/notes/upgrade-rails-version)
