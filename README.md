# Nmap scan

**1. SYN scan (-sS)**

**SYN scan** là loại quét mặc định và tùy chọn phổ biến nhất cho các lý do tốt. Nó có thể thực hiện nhanh chóng, quét hàng ngàn cổng trên 1 giây và không bị hạn chế bởi tường lửa. Nó cũng tương đối lén lút vì nó không hoàn thành các kết nối TCP. Nó cũng cho phép phân biệt rõ ràng giữa những trạng thái ***open***,***closed*** và ***filtered***.

Kỹ thuật này được gọi là quét nửa mở vì chúng ta không hoàn thành kết nối TCP. Chúng ta gửi 1 gói tin TCP như là yêu cầu 1 kết nối thực và chờ đợi một phản hồi lại. Một gói tin chứa SYN/ACK sẽ thể hiện là cổng đang ***open***, một gói tin có chứa RST sẽ thể hiện là cổng đang ***closed***. Sau vài lần truyền lại mà không có hồi đáp thì cổng sẽ được đánh dấu là ***filtered***. 
  
<img src="http://i.imgur.com/ifSO6YW.png">

Từ cổng đích gửi lại gói tin TCP với cờ SYN/ACK thể hiện cổng ở trạng thái ***open***.
<img src="http://imgur.com/021sAbc.png">

Từ cổng đích gửi lại gói tin TCP với cờ RST/ACK thể hiện cổng ở trạng thái ***closed***.

**2. TCP connect scan(-sT)**

**TCP connect scan**: Kỹ thuật này cho kết quả tương tự như **SYN scan**, nếu nhận được SYN/ACK thì sẽ gửi gói tin ACK để hoàn tất quá trình bắt tay 3 bước. **TCP connect scan** được dùng khi user không có quyền truy cập *raw packet* để thực hiện **SYN scan** (thường thì với quyền root trên linux mới có thể sử dụng **SYN scan** ). **TCP connect scan** sẽ sử dụng TCP stack của hệ điều hành để tạo ra 1 kết nối bình thường với mục tiêu, do thực hiện 1 kết nối đầy đủ nên kỹ thuật này dễ bị phát hiện bởi hệ thống log của mục tiêu do đó **SYN scan** thường được sử dụng nhiều hơn để tránh bị phát hiện.

<img src="http://imgur.com/KM4CkzQ.png">

Từ cổng đích gửi lại gói tin TCP với cờ SYN/ACK thể hiện cổng ở trạng thái ***open*** và cổng nguồn sẽ gửi gói tin với cờ ACK để hoàn thành quá trình bắt tay 3 bước.
<img src="http://imgur.com/sVnjT93.png">

Từ cổng đích gửi lại gói tin TCP với cờ RST/ACK thể hiện cổng ở trạng thái ***closed***.

**3. NULL scan,FIN scan,XMAS scan**

Theo RFC 793,nếu một cổng ở trạng thái ***closed*** thì gói tin đến không chứa cờ RST sẽ gây ra một đáp ứng lại RST. Và các gói tin được gửi tới các cổng ***open*** mà không chứa các cờ SYN,ACK,RST thì đều không có phản hổi trở lại. Với các hệ thống quét phù hợp RFC này,bất kỳ gói nào không chứa các cờ SYN, RST, hoặc ACK sẽ dẫn đến một RST trả về nếu cổng ***closed*** và không có phản hồi nào nếu cổng ***open***. Miễn là không có một trong số ba cờ trên, bất kỳ sự kết hợp của ba khác (FIN, PSH, và URG) đều được chấp nhận.

**3.1 NULL scan(-sN)**

**NULL scan*** là việc gửi gói tin TCP mà trong đó không chứa bất kỳ cờ nào, hay trường cờ bằng 0. Việc này thỏa mãn với RFC nói trên, nếu cổng ở trạng thái ***open*** hay ***filtered*** thì đều không có phản hồi, cổng ở trạng thái ***closed*** thì sẽ có phản hồi với cờ [RST,ACK]. Ưu điểm chính của các loại quét này là chúng có thể lẻn qua một số bức tường lửa không phải trạng thái và bộ định tuyến lọc gói tin. Một ưu điểm khác là loại scan này có vẻ lén lút hơn nhiều so với một SYN scan tuy nhiên hầu hết các sản phẩm IDS hiện đại có thể được định cấu hình để phát hiện chúng. Nhược điểm lớn là không phải tất cả các hệ thống đều tuân theo RFC 793. Một số hệ thống gửi các phản hồi RST tới đầu dò bất kể cổng có mở hay không. Điều này làm cho tất cả các cổng được dán nhãn đóng. Một nhược điểm của scan này là không thể phân biệt các cổng ***open*** từ một số cổng ***filtered*** nhất định, để lại cho bạn với trạng thái **open|filtered**.

<img src="http://i.imgur.com/CgsIvTa.png">

Cấu tạo gói tin TCP được gửi đi trong **NULL scan**.
<img src="http://imgur.com/rLG8sVE.png">

Từ cổng nguồn gửi các gói tin TCP với trường cờ bằng 0, không có phản hồi thể hiện cổng ở trạng thái ***open*** .

**3.2 FIN scan(-sF)**

Tương tự **NULL scan**, **FIN scan** cũng tuân theo hệ thống scan sử dụng RFC 793 nhưng thay vì gửi gói tin với trường cờ bằng 0 thì **FIN scan** gửi gói tin TCP với cờ FIN. Do tuân theo RFC 793 nên các trạng thái cổng được scan được trả về giống như với **NULL scan**. Cổng ở trạng thái ***open*** hay ***filtered*** thì không có phản hồi và cổng ở trạng thái ***closed*** được phản hồi với cờ [RST,ACK]. Ưu nhược điểm của **FIN scan** cũng tương tự với **NULL scan**.

<img src="http://imgur.com/caP8POe.png">

Cấu tạo gói tin TCP được gửi đi trong **FIN scan**.
<img src="http://imgur.com/9Dd6noh.png">

Từ cổng nguồn gửi các gói tin TCP với cờ [FIN,PSH.URG] có phản hồi với cờ [RST,ACK] thể hiện cổng ở trạng thái ***closed*** .

**3.3 XMAS scan(-sX)**

Tương tự **NULL scan** và **FIN scan**, **XMAS scan** cũng tuân theo hệ thống scan sử dụng RFC 793 nhưng thay vì gửi gói tin không có tờ hay gói tin với cờ FIN thì **XMAS scan** gửi gói tin TCP với cờ [FIN,PSH.URG]. Do cũng tuân theo RFC 793 nên các trạng thái cổng được scan được trả về giống như với **NULL scan** và **FIN scan**. Cổng ở trạng thái ***open*** hay ***filtered*** thì không có phản hồi và cổng ở trạng thái ***closed*** được phản hồi với cờ RST. Ưu nhược điểm của **XMAS scan** cũng tương tự với **NULL scan**,**FIN scan**.

<img src="http://imgur.com/4zzJD9H.png">

Cấu tạo gói tin TCP được gửi đi trong **XMAS scan**.
<img src="http://imgur.com/MEsMg3F.png">

Từ cổng nguồn gửi các gói tin TCP với cờ [FIN,PSH.URG] có phản hồi với cờ [RST,ACK] thể hiện cổng ở trạng thái ***closed*** .




