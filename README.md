# Nmap scan

## SYN scan

* **SYN scan** là loại quét mặc định và tùy chọn phổ biến nhất cho các lý do tốt. Nó có thể thực hiện nhanh chóng, quét hàng ngàn cổng trên 1 giây và không bị hạn chế bởi tường lửa. Nó cũng tương đối lén lút vì nó không hoàn thành các kết nối TCP. Nó cũng cho phép phân biệt rõ ràng giữa những trạng thái ***open***,***closed*** và ***filtered***.

* Kỹ thuật này được gọi là quét nửa mở vì chúng ta không hoàn thành kết nối TCP. Chúng ta gửi 1 gói tin TCP như là yêu cầu 1 kết nối thực và chờ đợi một phản hồi lại. Một gói tin chứa SYN/ACK sẽ thể hiện là cổng đang ***open***, một gói tin có chứa RST sẽ thể hiện là cổng đang ***closed***. Sau vài lần truyền lại mà không có hồi đáp thì cổng sẽ được đánh dấu là ***filtered***. 
  
<img src="http://i.imgur.com/ifSO6YW.png"> 
Từ cổng đích gửi lại gói tin TCP với cờ SYN/ACK thể hiện cổng ở trạng thái ***open***.

<img src="http://imgur.com/021sAbc.png">
Từ cổng đích gửi lại gói tin TCP với cờ RST/ACK thể hiện cổng ở trạng thái ***closed***.

## NULL scan

* Theo RFC 793,nếu một cổng ở trạng thái ***closed*** thì gói tin đến không chứa cờ RST sẽ gây ra một đáp ứng lại RST. Và các gói tin được gửi tới các cổng ***open*** mà không chứa các cờ SYN,ACK,RST thì đều không có phản hổi trở lại. Với các hệ thống quét phù hợp RFC này,bất kỳ gói nào không chứa các cờ SYN, RST, hoặc ACK sẽ dẫn đến một RST trả về nếu cổng ***closed*** và không có phản hồi nào nếu cổng ***open***. Miễn là không có một trong số ba cờ trên, bất kỳ sự kết hợp của ba khác (FIN, PSH, và URG) đều được chấp nhận.
* **NULL scan*** là việc gửi gói tin TCP mà trong đó không chứa bất kỳ cờ nào, hay trường cờ bằng 0. Việc này thỏa mãn với RFC nói trên, nếu cổng ở trạng thái ***open*** hay ***filtered*** thì đều không có phản hồi, cổng ở trạng thái ***closed*** thì sẽ có phản hồi với cờ RST. Ưu điểm chính của các loại quét này là chúng có thể lẻn qua một số bức tường lửa không phải trạng thái và bộ định tuyến lọc gói tin. Một ưu điểm khác là loại scan này có vẻ lén lút hơn nhiều so với một SYN scan tuy nhiên hầu hết các sản phẩm IDS hiện đại có thể được định cấu hình để phát hiện chúng. Nhược điểm lớn là không phải tất cả các hệ thống đều tuân theo RFC 793. Một số hệ thống gửi các phản hồi RST tới đầu dò bất kể cổng có mở hay không. Điều này làm cho tất cả các cổng được dán nhãn đóng. Một nhược điểm của scan này là không thể phân biệt các cổng ***open*** từ một số cổng ***filtered*** nhất định, để lại cho bạn với các phản ứng **open|filtered**.

<img src="http://i.imgur.com/CgsIvTa.png">
Cấu tạo gói tin TCP được gửi đi trong NULL scan.

<img src="http://imgur.com/a/xjqGE.png">
Từ cổng nguồn gửi các gói tin TCP với trường cờ bằng 0, không có phản hồi thể hiện cổng ở trạng thái ***open***.


