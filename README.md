# Nmap scan

## SYN scan

 * **SYN scan** là loại quét mặc định và tùy chọn phổ biến nhất cho các lý do tốt. Nó có thể thực hiện nhanh chóng, quét hàng ngàn cổng trên 1 giây và không bị hạn chế bởi tường lửa. Nó cũng tương đối lén lút vì nó không hoàn thành các kết nối TCP. Nó cũng cho phép phân biệt rõ ràng giữa những trạng thái ***open***,***closed*** và ***filtered***.
 
  * Kỹ thuật này được gọi là quét nửa mở vì chúng ta không hoàn thành kết nối TCP. Chúng ta gửi 1 gói tin TCP như là yêu cầu 1 kết nối thực và chờ đợi một phản hồi lại. Một gói tin chứa SYN/ACK sẽ thể hiện là cổng đang ***open***, một gói tin có chứa RST sẽ thể hiện là cổng đang ***closed***. Sau vài lần truyền lại mà không có hồi đáp thì cổng sẽ được đánh dấu là ***filtered***. 
  
<img src="http://i.imgur.com/ifSO6YW.png"> 
*Từ cổng đích gửi lại gói tin SYN/ACK thể hiện cổng ở trạng thái ***open***.

<img src="http://imgur.com/021sAbc.png">
*Từ cổng đích gửi lại gói tin RST/ACK thể hiện cổng ở trạng thái ***closed***.
