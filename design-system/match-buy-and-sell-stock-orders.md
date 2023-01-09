---
description: >-
  Hẳn năm vừa rồi, những ai vào chứng khoán và crypto thì đều lòng đau như cắt.
  Downtrend và tôn chỉ "hold to die" đã kéo các trade thủ xa bờ hơn bao giờ hết.
  Vậy làm sao có thể khớp lệnh mua bán chúng?
---

# Match Buy and Sell stock orders

Cổ phiếu lên và xuống liên tục. Vậy bạn có biết cấu trúc dữ liệu nào được sử dụng để khớp các lệnh mua và bán.

Stocks exchanges sử dụng **order books** (sổ lệnh). Một order book là một danh sách các lệnh mua và bán được tổ chức theo các mức giá. Nó có một sổ lệnh mua và một sổ lệnh bán, trong đó mỗi mặt của order book chứa một loạt các mức giá và mỗi mức giá lại chứa một danh sách các orders lệnh (first in first out).

Sơ đồ dưới dấy là một ví dụ về các mức giá và số lượng xếp hàng của mỗi level giá.

<figure><img src="../.gitbook/assets/order book stocks exchange.jpg" alt=""><figcaption><p>Sơ đồ các lệnh orders book mua và bán</p></figcaption></figure>

Vậy điều gì sẽ xảy ra khi bạn thực hiện một lệnh đặt mua 2700 cổ phiếu trong sơ đồ bên trên?

* Lệnh mua sẽ khớp với tất cả các lệnh bán tại giá 100.10, và lệnh bán đầu tiên (giá 900) tại giá 100.11 (minh họa bằng màu đỏ nhạt).
* Bây giờ, bởi gì lênh mua lớn, nên nó đã "eats up" khớp hết các lệnh bán ở level giá đầu tiên, nên level giá bán tốt nhất sẽ được tăng lên từ 100.10 lên 100.11.
* Khi thị trường tăng giá (bullish), mọi người có xu hướng mua cổ phiếu nhiều hơn, chính vì vậy mà giá sẽ liên tục tăng.

Một cấu trúc hiệu quả cho một order book phải đáp ứng:

* Thời gian tra cứu liên tục. Các hoạt động bao gồm: lấy khối lượng ở một mức giá hoặc giữa các mức giá, truy vấn giá thầu (giá mua), giá bán tốt nhất.
* Các thao tác thêm, hủy bỏ, thực thi, update phải được thực hiện nhanh, tốt nhất độ phức tạp của chúng chỉ là O(1). Các thao tác bao gồm đặt lệnh mới, hủy một lệnh và khớp lệnh.

## Low latency stock exchange

Vậy làm thế nào để một sàn chúng khoán hiện đại có thể đạt được độ trễ là micro giây (**microsecond latency**) ? Nó sẽ phải đáp ứng những nguyên lý sau đây:

**Càng tối giản càng tốt** (_do less on the critical path_)

* Ít các task để completed một nhiệm vụ, tính năng.
* Tối ưu thời gian trên mỗi task
* Sử dụng ít các bước nhảy (network hops), chuyển trang.
* Sử dụng ít bộ nhớ hơn (less disk usage)

Đối với stock exchange, the critical path là:

* start: một lệnh order đi vào trình quản lý đơn đặt hàng (order manager)
* mandatory risks checks: kiểm tra rủi ro là bắt buộc
* lệnh được khớp và việc thực thi được gửi lại
* **end**: the execution comes out of the order manager

<figure><img src="../.gitbook/assets/low latency.jpg" alt=""><figcaption><p>Low Latency Stock Exchange Design</p></figcaption></figure>

Những non-critical tasks sẽ được loại bỏ.

Chúng ta sẽ kết hợp một thiết kế như trên sơ đồ:

* Triển khai tất cả các components trong một máy chủ khổng lồ (a single giant server - no containers). _Note, ở đây đặt ra một vấn đề về tính mở rộng của hệ thống (scalability)._&#x20;
* Sử dụng shared memory như một event bus để giao tiếp giữa các components, không sử dụng ổ đĩa để lưu trữ (no hard disk)
* Các thành phần chính (key components) như Order Manager và Matching Engine là các luồng đơn (single-threaded) on the critical path, và mỗi luồng sẽ được ghim (pinded) vào một CPU để không có chuyển đổi ngữ cảnh (no context switch) và không locks.
* The single-threaded sẽ thực hiện lặp để thực thi từng task theo một trình tự.
* Các thành phần khác lắng nghe các sự kiện trên event bus và xử lý tương ứng.

Vậy dựa vào kinh nghiệm và các phân tích kỹ thuật, các mô hình mà các trader có thể giao dịch và kiếm được lợi nhuận. Nhưng bài toán đặt ra ở đây là, liệu các sàn có can thiệp vào các lệnh, để thao túng giá bất chính không nhỉ. Thị trường tài chính quả thật là không dễ.

Nội dung bài viết trên được dịch lại từ [https://blog.bytebytego.com/p/match-buy-and-sell-stock-orders](https://blog.bytebytego.com/p/match-buy-and-sell-stock-orders)

Trong bài tiếp theo, chúng ta sẽ nghiên cứu tiếp về Log4j from attack to prevention.
