---
description: >-
  Một cây tứ giác (quadtree) hoạt động như thế nào? Trong bài viết này, chúng ta
  sẽ tìm hiểu một cấu trúc dữ liệu khác, dùng để tim các nhà hàng lân cận trên
  Yelp hoặc Google Maps.
---

# How quadtree works

Một cây tư giác (quadtree) là một cấu trúc dữ liệu thường được sử dụng để phân chia vùng không gian hai chiều bằng cách chia nhỏ nó theo cách đệ quy thành bốn góc phần tư (lưới - grids) cho đến khi nội dung của lưới đáp ứng các tiêu chi nhất định (xem sơ đồ dưới đây)

<figure><img src="../.gitbook/assets/quadtree diagram.jpeg" alt=""><figcaption><p>Quadtree diagram</p></figcaption></figure>

Quadtree là một cấu trúc dữ liệu được lưu trong bộ nhớ **(in memory data structure),** nó không phải là một giải pháp database **(database solution)**. Nó chạy trên mỗi máy chủ LBS (Location-Based Service - chúng ta đã nói ở bài trước), và cấu trúc dữ liệu này được build trên server tại thời điểm khởi động máy chủ.

Sơ đồ thứ hai giải thích chi tiến hơn về quá trình xây dựng quadtree. Root node đại diện cho toàn bộ bản đồ của thế giới. Root note được chia đệ quy thành 4 góc phần tư cho đến khi không còn node nào có hơn 100 doanh nghiệp.

**Vậy làm sao để có được danh sách các doanh nghiệp lân cận.**

* Build quadtree trong vào bộ nhớ memory.
* Sau khi xây dựng quatree, hãy bắt đầu tìm kiếm từ gốc và duyệt qua cây, tới khi chúng ta tìm ra node lá, nơi xuất phát tìm kiếm.
* Nếu node lá đó có 100 doanh nghiệp, trả về node đó. Nếu không, hãy thêm các doanh nghiệp gần đó cho đến khi đủ doanh nghiệp được trả về.

**Cập nhật LBS server và rebuild quadtree.**

* Nó có thể mất vài phút để build một quadtree vào memory với 200 triệu doanh nghiệp tại thời điểm server start-up.
* Trong khi quadtree được build, server không thể phục vụ lưu lượng truy cập.
* Do đó, tại một thời điểm chúng ta nên triển khai dần dần một bản phát hành mới của máy chủ cho một nhóm nhỏ máy chủ. Điều này tránh việc mất một phần lớn cụm máy chủ ngoại tuyến (server cluster offline) và gây ra tình trạng mất kết nối dịch vụ.



Nội dung trên được dịch lại từ [https://blog.bytebytego.com/p/how-quadtree-works](https://blog.bytebytego.com/p/how-quadtree-works)
