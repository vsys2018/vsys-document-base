### IOTA CONNECTOR DOCUMENT
[Tài liệu V2](https://github.com/VSYS-DevTeam/IOTA-VTConnector)

IOTA connector là một ngoại vi ảo hóa. Mục đích chính của nó là để tạo một kết nối realtime đến IOTA server. Từ đây, giữa nền tảng IOTA và IO ảo có thể trao đổi với nhau thông qua các thông điệp gửi đi.

![](https://oustittl.sirv.com/iota-connector/IOTA.png?w=800)

IOTA connector hoạt động theo kiểu "1 - nhiều". Tức là 1 IOTA connector sẽ có nhiều bộ IO ảo. Giống như đầu số tổng đài vậy, mỗi một user truy cập vào 1 địa chỉ IOTA connector xác định sẽ được phân phối đến 1 IO ảo tương ứng để làm nhiệm vụ xử lí. IOTA connector sẽ có số lượng IO giới hạn, nhiều hay ít tùy thuộc vào người lập trình thêm/bớt IO vào. Khi tất cả các IO đều đã được phân phối thì những user truy cập vào sau sẽ nhận được thông báo "IO center busy now" cho đến khi có IO rảnh trong hàng đợi.

![](https://oustittl.sirv.com/iota-connector/IOTA-BUSY.png?w=800)

### Quy trình hàng đợi:

![](https://oustittl.sirv.com/iota-connector/IOTA-ADD-QUEUE.png)

**(1)**: Khi user 1 gửi tin nhắn tới IOTA connector. Trong hàng đợi Session đã có User 1 nên tin nhắn được chuyển thẳng tới IO 1.

**(2)**: Khi user 2 gửi tin nhắn tới IOTA connector. Trong hàng đợi Session chưa có user 2.

**(3)**: Yêu cầu 1 bộ IO trong hàng đợi FREE.

**(4)**: Di chuyển bộ IO đầu tiên trong hàng đợi FREE lên hàng đợi Session. Dán nhãn cho bộ này phục vụ user 2.

### Quy trình giải phóng:

![](https://oustittl.sirv.com/iota-connector/IOTA-FREE-QUEUE.png)

**(1)**: Bộ IO 1 chủ động gửi lệnh `'io.free'` đến IOTA connector.

**(2)**: Di chuyển bộ IO 1 trong hàng chờ Session xuống hàng chờ FREE.

### Thêm IOTA connector thành input cho IO

Mỗi IOTA connector có một mã MAC tương ứng. Mã MAC này sử dụng khi add input vào IO. Mã này được tạo ngẫu nhiên, chỉ có người nào biết mã này thì mới add input cho IO được.

Sau khi add input thành công ta gửi lệnh `'info'` để kiểm tra trạng thái thiết bị.

Ví dụ ta đã add IOTA connector 01 thành I101 cho IO ảo 01 thì ở giao diện dòng lệnh của IO ảo 01 ta gửi lệnh: `I101'info'`

Kết quả trả về: `OP1A00001,LOGGED,SESSION,5f7c22fafd0b185068458b28`
Trong đó:

- `OP1A00001` là mã ID của IOTA connector
- `LOGGED` tức là IOTA connector này đã được đăng nhập tài khoản IOTA. Nếu là `UNLOGIN` tức là chưa đăng nhập.
- `SESSION` tức là IO ảo 01 đã được phân phối và đang xử lí cho một ai đó (đang bận). Nếu là `FREE` tức là IO ảo 01 đang trong hàng chờ rảnh của IOTA connector 01 (đang rảnh).

### Danh sách các lệnh đối với IOTA connector:

- `I101'info'` kiểm tra trạng thái thiết bị.
- `I101'io.del'` xóa bộ IO hiện tại ra khỏi IOTA connector. Xóa được coi là thành công khi kiểm tra lại `I101'info'` và nhận được thông báo `ERR_TARGET_NOT_FOUND`
- `I101'io.send,user,message'` yêu cầu gửi tin nhắn qua IOTA connector cho `user` với nội dung `message`.
- `I101'io.free'` giải phóng phiên làm việc cho bộ IO hiện tại. Trả bộ IO hiện tại vào hàng chờ rảnh của IOTA connector.

### Danh sách sự kiện:

- `I101-1` sự kiện khi có tin nhắn đến từ user.

Khi sự kiện IOTA connector nhận được tin nhắn thì nội dung có dạng `user|message|session` và lưu vào `#1` của `N` đang chứa sự kiện đó.

![](https://oustittl.sirv.com/iota-connector/IOTA-MSG.png)

- `I101-2` sự kiện khi đóng trình duyệt.

Tài liệu sử dụng IOTA:
[Link](https://github.com/nghuyy/iota/blob/main/VirtualIO.md)
