### Mẫu quy trình trình bày menu iota theo dạng cột

Bước 1: Chuẩn bị api

![](https://oustittl.sirv.com/documents/img001.png)

Ví dụ đây là table sản phẩm. Để có thể hiển thị được trên iota thì ta phải đổi tên cột theo mẫu ở dưới.

![](https://oustittl.sirv.com/documents/img002.png)

API nhớ phải bật "accept for web"

![](https://oustittl.sirv.com/documents/img003.png)

Bước 2: Query dữ liệu ở io-cloud

Phải sử dụng api dạng v2.

![](https://oustittl.sirv.com/documents/img004.png)

Dùng D62 để query như bình thường. Lúc này dữ liệu trả về đầy đủ sẽ nằm ở #892

![](https://oustittl.sirv.com/documents/img005.png)

Bước 3: Gửi lên iota

Dùng cú pháp theo mẫu

```
Cú pháp:
I101.D14,{send_type},{name_id},{title},{column},{data}

Trong đó:
{send_type} là kiểu gửi dữ liệu. Có 2 kiểu là: "user" và "session"
{name_id} là username hoặc session id muốn gửi đến
{title} là tiêu đề list
{column} là số lượng cột. Nhận giá trị từ 1-8
{data} là dữ liệu gửi lên
```

![](https://oustittl.sirv.com/documents/img006.png)

Kết quả:
![](https://oustittl.sirv.com/documents/img007.png)
