---
title: Hướng dẫn nâng cấp phiên bản Ruby
date: 2024-06-19
tags:
  - technical
---
Mỗi năm Ruby cho ra một phiên bản với những tính năng mới được cập nhật, bên cạnh đó cũng dừng hỗ trợ hay thêm các bản vá cho các [version cũ](https://endoflife.date/ruby). Việc cập nhật đôi khi phức tạp và khó khăn nhưng những lợi ích nó mang lại sẽ giúp bạn tránh được các rủi ro sau này. Dưới đây, chúng ta cùng thảo luận kỹ hơn về cách cập nhật phiên bản cho Ruby nhé.

## Bước 1: Chuẩn bị

**1. Đánh giá phiên bản Ruby hiện tại và lựa chọn phiên bản mục tiêu:**

- Kiểm tra phiên bản Ruby đang sử dụng.
- Lựa chọn phiên bản mục tiêu:
  - Bạn có thể tìm danh sách phiên bản được Ruby phát hành ở [đây](https://www.ruby-lang.org/en/downloads/releases/) để quyết định xem phiên bản mục tiêu mình sẽ nâng là gì.
  - Lời khuyên là sẽ nâng dần dần, từng bước một và chỉ nâng lên phiên bản kế tiếp ổn định. Điều này giúp tránh những thay đổi lớn có thể gây ra những khó khăn không mong muốn.
- VD:
  - Version Ruby hiện tại: 2.7.6
  - Version Ruby mục tiêu: 3.3.x
  - Các bước nâng dần dần: 2.7.6 -> 2.7.x, 2.7.x -> 3.0.x, 3.0.x -> 3.1.x, 3.1.x -> 3.2.x, 3.2.x -> 3.3.x (x là version ổn định tùy từng thời điểm cập nhật)

**2. Hiểu các thay đổi trong phiên bản mục tiêu**
- Xem lại các [release changelog](https://www.ruby-lang.org/en/news/2023/12/25/ruby-3-3-0-released/) để hiểu các thay đổi được giới thiệu trong phiên bản mục tiêu.
- Tương ứng với việc nâng cấp dần dần, chúng ta cũng hiểu từng thay đổi trong từng phiên bản kế tiếp ổn định. Ví dụ, nâng từ 2.7 lên 3.0, bạn phải hiểu những thay đổi tại [version 3.0](https://rubyreferences.github.io/rubychanges/3.0.html) có những thay đổi nào và ảnh hưởng tới dự án ra sao.

**3. Chuẩn bị kế hoạch Unit test và Testing**
- Kiểm tra độ bao phủ code hiện tại của dự án, điều này cực kỳ hữu ích để đảm bảo các chức năng quan trọng nhất của ứng dụng hoạt động như mong đợi với phiên bản mới. Target (coverage của Unit test đạt trên 80%)
- Lựa chọn các phương pháp kiểm thử phù hợp, lên kế hoạch thực hiện manual test được thực hiện bởi QA.

**4. Chuẩn bị kế hoạch nâng cấp**
- Xác định quy trình làm việc phù hợp nhất để thực hiện nâng cấp
- Lên kế hoạch triển khai bao gồm các bước tìm hiểu changelog, implement, kiểm thử và đảm bảo chất lượng với các milestone tương ứng.

## Bước 2: Thực hiện resolve tất cả các deprecated warning của version trước đó.

- Điều này thường được kiểm tra khi chạy Unit Test, Ruby sẽ show các deprecated warning, việc cần làm là kiểm tra xem warning nó ảnh hưởng thế nào và xử lý các warning này.
- Lưu ý: Từ version Ruby 2.7 trở đi [Ruby không hiển thị mặc định warning](https://bugs.ruby-lang.org/issues/17000) khi chạy Unit test nữa, nên phải thêm option `RUBYOPT='-W:deprecated'` như sau:
```
RUBYOPT='-W:deprecated' bundle exec rspec
```

## Bước 3: Thực hiện nâng cấp
#### 3.1. Tài liệu nâng cấp Ruby
  **3.1.1. List changelog:** List thay đổi của Ruby version trước và version mong muốn, xem có ảnh hưởng đến source code của dự án không.

  **3.1.2. Verify gem related:**
   - Kiểm tra từng gem đang sử dụng xem version hiện tại của gem có tương thích với Ruby version mới không.
     - Kéo gem version hiện tại về máy local.
     - Chạy Unit test của gem
   - Đánh giá ảnh hưởng, tìm version tương thích
     - List changelog của gem xem có ảnh hưởng đến source code của dự án không.
     
#### 3.2. Implement

1. Upgrade gem ảnh hưởng
2. Implement changelog ruby ảnh hưởng
3. Run RSpec, list warning, failure case và resolve những warning và failure case đó.

#### 3.3. List các phần ảnh hưởng
- List những phần đã thay đổi cho QA, điều này giúp giảm thiểu effort khi test, giúp QA chú trọng test kỹ hơn phần thay đổi. 

## Bước 4: Thực hiện test
- Chuẩn bị 2 môi trường tương ứng với 2 version cũ và mới.
- Thực hiện test đảm bảo function trên môi trường mới hoạt động giống môi trường cũ.

Việc chạy phiên bản Ruby ổn định mới nhất sẽ mang lại cho ứng dụng những lợi ích từ các tính năng mới và cải thiện hiệu suất. Tuy nhiên, việc xác định các bước của quá trình này không phải lúc nào cũng dễ dàng. Áp dụng một quy trình có hệ thống và khoa học có thể giải quyết vấn đề này và đảm bảo quá trình nâng cấp diễn ra suôn sẻ, thành công, đồng thời giải quyết tất cả các khía cạnh cần thiết trong quá trình nâng cấp.