![iota connector](https://oustittl.sirv.com/iota-connector/sub-input-iocloud.png)

### TÀI LIỆU MÔ TẢ BỘ TẬP LỆNH INPUT IOTA (IOTA CONNECTOR)

Để cải tiến hệ thống thì trước hết các ngoại vi (input) của hệ thống IOTA nên đưa về một chuẩn chung. Tức là đảm bảo tính đồng bộ và nhất quán giữa các ngoại vi. Ở đây, dựa theo nguyên lí module mỗi input sẽ có các đặc điểm chung:

- Có một khai báo các lệnh con trong input đó.
- Có một khai báo các # cài đặt con trong input đó.
- Có một khai báo các sự kiện

Việc quy hoạch lại như thế này sẽ giúp cho các thao tác lệnh phức tạp được giao cho các ngoại vi input xử lí. Code sẽ tường minh, trong sáng hơn. Đảm bảo chuẩn kết nối giữa các hệ thống

Mô tả phương thức kết nối input:

- Khi add một input bất kỳ vào bộ iocloud. Input sẽ tự động khai báo với iocloud các danh sách cài đặt của mình. Bao gồm:
  - Danh sách các hành động con được quy ước từ D1, D2,... Dn
  - Danh sách các tham số quy ước
  - Danh sách các sự kiện
- Việc đặt tên sẽ được người dùng cài đặt thủ công

Bảng hành động IOTA CONNECTOR:

Các hành động của một input sẽ có dạng `I{number}.D{number}`.
Ví dụ: Ta khai input iota connector là `I101` thì nó sẽ có các hành động tương ứng `I101.D1`, `I101.D2`, `I101.D3`, ...

Để chạy các hành động này, ta dùng lại lệnh D4 để gọi và truyền tham số vào.
Ví dụ: `D4I101.D2,"user","0912345678"`

**D1 - giải phóng IO session**

```
Cú pháp:
I101.D1
```

- Ví dụ:
- Cú pháp cũ : `D4I101,'io.free'`
- Cú pháp mới: `D4I101.D1`

**D2 - xóa màn hình trang iota**

```
Cú pháp:
I101.D2,{send_type},{name_id}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
```

- Ví dụ 1:
- Cú pháp cũ : `D4I101,'io.send,0912345678,:cls'`
- Cú pháp mới: `D4I101.D2,"user","0912345678"`

- Ví dụ 2:
- Cú pháp cũ : `D4I101,'io.ssend,sX7tmXzbuA6szfsgA_mhE,:cls'`
- Cú pháp mới: `D4I101.D2,"session","sX7tmXzbuA6szfsgA_mhE"`

**D3 - gửi text lên trang iota**

```
Cú pháp:
I101.D3,{send_type},{name_id},{text}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{text} là nội dung muốn gửi
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,welcome to vsys'`
- Cú pháp mới: `D4I101.D3,"user","0912345678","welcome to vsys"`

**D4 - gửi hình ảnh lên trang iota**

```
Cú pháp:
I101.D4,{send_type},{name_id},{image_url},{title},{padding},{width},{height},{hoverlink}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{image_url} là url hình ảnh
{title} là tiêu đề nằm trên hình ảnh
{padding} là khoảng cách từ mép hình đến viền
{width} là chiều rộng hiển thị
{height} là chiều cao hiển thị
{hoverlink} là link sẽ được chuyển khi click vào hình

```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Mua hàng', 'l':'iota://mua-hang'}'`

- Cú pháp mới: `D4I101.D4,"user","0912345678","https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png","Mua hàng",0,80,80,"iota://mua-hang"`

**D5 - gửi list hình lên trang iota**

```
Cú pháp:
I101.D5,{send_type},{name_id},{title},{data}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{title} là tiêu đề list
{data} là dữ liệu gửi lên
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,{'iolist':[{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Mua hàng', 'l':'iota://mua-hang'},{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Bán hàng', 'l':'iota://ban-hang'}],'n': 'Danh sách'}'`

- Cú pháp mới: `D4I101.D5,"user","0912345678","Danh sách","[{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Mua hàng', 'l':'iota://mua-hang'},{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Bán hàng', 'l':'iota://ban-hang'}]"`

**D6 - gửi video lên trang iota**

```
Cú pháp:
I101.D6,{send_type},{name_id},{video_url},{title},{hover_link}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{video_url} là địa chỉ url video
{title} là tiêu đề video
{hover_link} là liên kết khi click vào
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,{'video':'https://next.iotabot.app/index.php/s/zMkeBpqmNFC66Ky/download','t':'Video 1','l' :'iota://url-1'}'`

- Cú pháp mới: `D4I101.D6,"user","0912345678","https://next.iotabot.app/index.php/s/zMkeBpqmNFC66Ky/download","Video 1","iota://url-1"`

**D7 - yêu cầu quét qrcode**

```
Cú pháp:
I101.D7,{send_type},{name_id}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,:qrcode'`

- Cú pháp mới: `D4I101.D7,"user","0912345678"`

**D8 - cài đặt input box**

```
Cú pháp:
I101.D8,{send_type},{name_id},{input_type}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{input_type} là kiểu input: "none", "text", "number", "date", ....
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,:input,none'`

- Cú pháp mới: `D4I101.D8,"user","0912345678","none"`

**D9 - cài đặt menu chính**

```
Cú pháp:
I101.D9,{send_type},{name_id},{menu_icon},{text}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{menu_icon} là mã icon. Tham khảo tại [https://fontawesome.com/v5.15/icons?d=gallery&p=2&s=solid&m=free]
{text} là nội dung muốn hiển thị
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,:toolbar.ex1./ico-money-check-alt 1.000.000 VND/'`

- Cú pháp mới: `D4I101.D9,"user","0912345678","money-check-alt","1.000.000 VND"`

**D10 - cài đặt menu phụ**

```
Cú pháp:
I101.D10,{send_type},{name_id},{menu_icon},{text},{event_name}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{menu_icon} là mã icon. Tham khảo tại [https://fontawesome.com/v5.15/icons?d=gallery&p=2&s=solid&m=free]
{text} là nội dung muốn hiển thị
{event_name} là mã sự kiện trả về khi click vào
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,:toolbar.ex1.chuyen-tien/ Chuyển tiền/'`

- Cú pháp mới: `D4I101.D10,"user","0912345678","","Chuyển tiền","chuyen-tien"`

Note: Menu phụ sẽ bị xóa khi update menu chính.

**D11 - bật tắt các icon theo vị trí**

```
Cú pháp:
I101.D11,{send_type},{name_id},{position},{state}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{position} là vị trí của icon từ 1 đến 7
{state} là trạng thái: "show" hoặc "hide"
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,:toolbar.icon.1.show'`

- Cú pháp mới: `D4I101.D11,"user","0912345678",1,"show"`

**D12 - đổi hình ảnh icon**

```
Cú pháp:
I101.D12,{send_type},{name_id},{position},{icon}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{position} là vị trí của icon từ 1 đến 7
{icon} là mã icon. Tham khảo tại [https://fontawesome.com/v5.15/icons?d=gallery&p=2&s=solid&m=free]
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,:toolbar.icon.1.ic-arrow-left'`

- Cú pháp mới: `D4I101.D12,"user","0912345678",1,"arrow-left"`

**D13 - cài đặt mã sự kiện khi click vào icon**

```
Cú pháp:
I101.D13,{send_type},{name_id},{position},{event_name}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{position} là vị trí của icon từ 1 đến 7
{event_name} là mã sự kiện
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,:toolbar.icon.1.iota-mnu-back'`

- Cú pháp mới: `D4I101.D13,"user","0912345678",1,"mnu-back"`

**D14 - gửi list hình lên trang iota theo cột**

```
Cú pháp:
I101.D14,{send_type},{name_id},{title},{column},{data}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{title} là tiêu đề list
{column} là số lượng cột
{data} là dữ liệu gửi lên
```

- Ví dụ :
- Cú pháp cũ : `D4I101,'io.send,0912345678,{'clist':[{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Mua hàng', 'l':'iota://mua-hang'},{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Bán hàng', 'l':'iota://ban-hang'}],'n': 'Danh sách','columns':2}'`

- Cú pháp mới: `D4I101.D14,"user","0912345678","Danh sách",2,"[{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Mua hàng', 'l':'iota://mua-hang'},{'src':'https://oustittl.sirv.com/vsys-brochure/quy-trinh-mua-hang.png','p':0,'w':80 ,'h':80 ,'t':'Bán hàng', 'l':'iota://ban-hang'}]"`