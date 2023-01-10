---
description: >-
  Dịch vụ ở gần - Làm thế nào chúng ta có thể tìm thấy các nhà hàng trên Google
  Maps hoặc Map trên iOS? Hôm nay chúng ta sẽ thảo luận về một số thiết kế đằng
  sau các dịch vụ đó.
---

# Proximity service

Có hai dịch vụ chính (xem sơ đồ phía dưới)

<figure><img src="../.gitbook/assets/proximity service.jpeg" alt=""><figcaption><p>Proximity Service Design</p></figcaption></figure>

**- Business Service**

* Thêm/ sửa/ xóa thông tin nhà hàng (restaurant informations)
* Khách hàng xem thông tin chi tiết nhà hàng (restaurant details)

**- Location-based Service**

* Đưa ra bán khính và vị trí, trả về danh sách các nhà hàng lân cận.&#x20;

Vậy làm thế nào vị trí của các nhà hàng được lưu trữ trong database để Local Based Service (LBS) có thể trả về danh sách các nhà hàng gần đó một cách hiệu quả?

Lưu trữ vĩ độ và kinh độ của những nhà hàng vào database? Việc truy vấn sẽ rất không hiệu quả khi bạn cần tính toán khoảng cách giữa bạn và mỗi nhà hàng.

Một cách để tăng tốc độ tìm kiếm là sử dụng thuật toán geohash.

Đầu tiên, chia trái đất thành bốn góc phần tư, cùng với kinh tuyến gốc và đường xích đạo.

* Phạm vi vĩ độ \[-90, 0] được biểu thị bằng 0
* Phạm vi vĩ độ \[0, 90] được biểu thị bằng 1
* Phạm vi kinh độ \[-180, 0] được biểu thị bằng 0
* Phạm vi kinh độ \[0, 180] được biểu thị bằng 1

Thứ hai, chia mỗi góc phần tư thành bốn lưới nhỏ hơn, mỗi lưới có thể được biểu diễn bằng cách xen kẽ giữa các bit kinh độ và bit vĩ độ.

Vì vậy khi bạn muốn tìm kiếm các nhà hàng gần đó tong lưới được đánh dấu màu đỏ, bạn có thể viết câu lệnh SQL như sau:

SELECT \* FROM geohash\_index WHERE geohash LIKE "01%"

Geohash có nhiều hạn chế. Có thể có nhiều nhà hàng trong một khu vực lưới (trung tâm thành phố), nhưng cũng có thể không có nhà hàng nào trong mạng lưới khác (ở trên biển). Vì vậy, có rất nhiều thuật toán phức tạp khác được sử dụng để tối ưu hóa quy trình này. Bạn sẽ tìm hiểu thêm các thuật toán khác chứ?

Nội dung bài viết trên được dịch lại từ [https://blog.bytebytego.com/p/proximity-service](https://blog.bytebytego.com/p/proximity-service)
