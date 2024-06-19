---
title: Hướng dẫn nâng cấp phiên bản Rails
date: 2024-06-19
---


Tương tự Ruby, mỗi năm Rails cho ra một phiên bản với những tính năng mới được cập nhật, bên cạnh đó cũng dừng hỗ trợ hay thêm các bản vá cho các [version cũ](https://endoflife.date/rails). Việc cập nhật đôi khi phức tạp và khó khăn nhưng những lợi ích nó mang lại sẽ giúp bạn tránh được các rủi ro sau này. Dưới đây, chúng ta cùng thảo luận kỹ hơn về cách cập nhật phiên bản cho Rails nhé. Nhìn chung, các bước upgrade Rails cũng tương tự upgrade Ruby, tuy nhiên có thêm một vài lưu ý khác

## Bước 1: Chuẩn bị

**1. Đánh giá phiên bản Rails hiện tại và lựa chọn phiên bản mục tiêu:**

- Kiểm tra phiên bản Rails đang sử dụng.
- Lựa chọn phiên bản mục tiêu:
  - Bạn có thể tìm danh sách phiên bản được Rails phát hành ở [đây](https://rubygems.org/gems/rails/versions) để quyết định xem phiên bản mục tiêu mình sẽ nâng là gì.
  - Lời khuyên là sẽ nâng dần dần, từng bước một và chỉ nâng lên phiên bản kế tiếp ổn định. Điều này giúp tránh những thay đổi lớn có thể gây ra những khó khăn không mong muốn.
  - Kiểm tra Ruby version hiện tại có tương thích với phiên bản Rails mong muốn không. Nếu phiên bản Ruby không tương thích bạn có thể xem [Hướng dẫn nâng cấp Ruby](/notes/upgrade-ruby-version)	  
	  - Rails 7 requires Ruby 2.7.0 or newer.
	- Rails 6 requires Ruby 2.5.0 or newer.
	- Rails 5 requires Ruby 2.2.2 or newer.

**2. Hiểu các thay đổi trong phiên bản mục tiêu**
- Xem lại các [release changelog](https://guides.rubyonrails.org/7_0_release_notes.html) để hiểu các thay đổi được giới thiệu trong phiên bản mục tiêu.

**3. Chuẩn bị kế hoạch Unit test và Testing**
- Kiểm tra độ bao phủ code hiện tại của dự án, điều này cực kỳ hữu ích để đảm bảo các chức năng quan trọng nhất của ứng dụng hoạt động như mong đợi với phiên bản mới. Target (coverage của Unit test đạt trên 80%)
- Lựa chọn các phương pháp kiểm thử phù hợp, lên kế hoạch thực hiện manual test được thực hiện bởi QA.

**4. Chuẩn bị kế hoạch nâng cấp**
- Xác định quy trình làm việc phù hợp nhất để thực hiện nâng cấp
- Lên kế hoạch triển khai bao gồm các bước tìm hiểu changelog, implement, kiểm thử và đảm bảo chất lượng với các milestone tương ứng.

## Bước 2: Thực hiện resolve tất cả các deprecated warning của version trước đó.

- Điều này thường được kiểm tra khi chạy Unit Test, Rails sẽ show các deprecated warning, việc cần làm là kiểm tra xem warning nó ảnh hưởng thế nào và xử lý các warning này.
- Lưu ý: Từ version Ruby 2.7 trở đi [Ruby không hiển thị mặc định warning](https://bugs.ruby-lang.org/issues/17000) khi chạy Unit test nữa, nên phải thêm option `RUBYOPT='-W:deprecated'` như sau:
```
RUBYOPT='-W:deprecated' bundle exec rspec
```

## Bước 3: Thực hiện nâng cấp
#### 3.1. Tài liệu nâng cấp Rails
  **3.1.1. List changelog:** List thay đổi của Ruby version trước và version mong muốn, xem có ảnh hưởng đến source code của dự án không.

  **3.1.2. Verify gem related:**
  - Kiểm tra từng gem đang sử dụng xem version hiện tại của gem có tương thích với Rails version mới không.
	  - Bước 1: Thay đổi version của gem trong Gemfile.
	  - Bước 2: Run `bundle update rails` trong terminal.
	  - Bước 3: Liệt kê các gem ảnh hưởng.
  - Đánh giá ảnh hưởng, tìm gem version tương thích.
  - List changelog của gem xem có ảnh hưởng đến source code của dự án không.
#### 3.2. Implement

1. Implement changelog Rails ảnh hưởng
2. Upgrade gem ảnh hưởng
3. Run RSpec, list warning, failure case và resolve những warning và failure case đó.

#### 3.3. List các phần ảnh hưởng
- List những phần đã thay đổi cho QA, điều này giúp giảm thiểu effort khi test, giúp QA chú trọng test kỹ hơn phần thay đổi. 

## Bước 4: Thực hiện test
- Chuẩn bị 2 môi trường tương ứng với 2 version cũ và mới.
- Thực hiện test đảm bảo function trên môi trường mới hoạt động giống môi trường cũ.


## Lưu ý khi upgrade:

### 1. Cache
- Vấn đề:
	Một số trường hợp version cũ lưu cache với key theo định dạng của version cũ nhưng sang version mới không đọc được key theo định dạng cũ dẫn đến hiển thị bị lỗi.
- Giải pháp:
	- Sử dụng prefix để tách cache rails v1 và rails v2.
		- `rails_v1_key_name` và `rails_v2_key_name`. Khi get data thì sẽ get dựa vào version rails đang deploy ở server hiện tại.

### 2. Changelog
- Vấn đề:
	List changelog dựa **Release Notes** chính thức của Rails nhưng vẫn có một số case bị lack do note thiếu so với thay đổi trong source code.
- Giải pháp:
	- Tìm hiểu thêm về các thay đổi trong CHANGELOG.md
	- Thực hiện viết Unit Test với độ bao phủ cao.
	- Thông báo cho Rails để update vào release note.

Việc chạy phiên bản Rails ổn định mới nhất sẽ mang lại cho ứng dụng những lợi ích từ các tính năng mới và cải thiện hiệu suất. Tuy nhiên, việc xác định các bước của quá trình này không phải lúc nào cũng dễ dàng. Áp dụng một quy trình có hệ thống và khoa học có thể giải quyết vấn đề này và đảm bảo quá trình nâng cấp diễn ra suôn sẻ, thành công, đồng thời giải quyết tất cả các khía cạnh cần thiết trong quá trình nâng cấp.