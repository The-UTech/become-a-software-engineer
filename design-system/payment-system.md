---
description: >-
  Một hệ thống thanh toán sẽ nên được thiết kế như thế nào? Chúng ta cùng tìm
  hiểu nhé!
---

# Payment System

Những ngày cuối năm, cận tết, nhu cầu mua sắm là rất nhiều. Và đây là cách mà tiền di chuyển khi bạn nhấp vào nút mua trên Amazone, Shopee hay bất kỳ shopping websites nào mà bạn yêu thích.

<figure><img src="../.gitbook/assets/Design Amazone Payment.jpeg" alt=""><figcaption><p>Hệ thống xử lý thanh toán tiền</p></figcaption></figure>

Luồng xử lý sẽ như sau:

1. Khi một khách hàng clicks vào button Buy, một sự kiện thanh toán sẽ được khởi tạo và gửi tới payment service.
2. Payment service sẽ lưu trữ sự kiện khách hàng đặt mua vào database.
3. Đôi khi một sự kiện thanh toán có thể chứa nhiều lệnh thanh toán. Ví dụ, bạn có thể chọn mua nhiều sản phẩm, từ nhiều sellers khách nhau trong một tiến trình thanh toán (in a single checkout process). Hệ thống Payment Service sẽ gọi Payment Executor (thực hiện thanh toán) cho mỗi lệnh thanh toán.
4. The Payment Executor sẽ thực hiện lưu trữ thông tin thanh toán vào database.
5. The Payment Executor sẽ thực hiện gọi các nhà cung cấp dịch vụ thanh toán (external payment service providers PSP) để hoàn tất thanh toán (bằng thẻ tín dụng, hoặc các ví điện tử...)
6. Sau khi người mua đã thực hiện thanh toán (payment executor) thành công, dịch vụ thanh toán (payment service) sẽ tiến hành ghi nhận và cập nhật số tiền tăng thêm trong ví của người bán.
7. Wallet Service (wallet server) cập nhật thông tin số dư của sellers vào database.
8. Sau khi Wallet Service cập nhật thông tin số dư của sellers thành công, Payment Service sẽ gọi  ledger (sổ cái thanh toán - dùng để lưu trữ lịch sử cộng, từ tiền) để cập nhật thông tin.\
   _(_[_A payment ledger_](https://baremetrics.com/blog/what-is-a-payment-ledger)_: Sổ cái thanh toán là một công cụ ghi sổ kế toán kinh doanh để ghi lại và theo dõi các khoản thanh toán dành cho các mục đích cụ thể. Các công ty sử dụng chúng cho các sự kiện đặc biệt và các hoạt động hàng ngày ghi lại số tiền nợ công ty.)_
9. The Ledger Service sẽ thêm các thông tin sổ cái mới (ledger information) vào database.
10. Payment service providers (PSP) hoặc ngân hàng sẽ gửi các hồ sơ thanh toán (files) tới khách hàng của họ. Thông tin thanh toán sẽ chứa số dư, cùng với tất cả các thông tin giao dịch mà khách hàng đã sử dụng.

Nội dung trên được dịch lại từ [https://blog.bytebytego.com/p/payment-system](https://blog.bytebytego.com/p/payment-system)

