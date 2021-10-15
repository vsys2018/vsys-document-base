### Các lệnh nội bộ trên IO CLOUD

D66: Lệnh lấy dữ liệu từ API về và lưu kết quả vào 1 #
```
Cú pháp:

D66,{api_url},{#_status},{#_result},{c1},{c2},...

```
Trong đó:

- {api_url} là địa chỉ # chứa địa chỉ api
- {#_status} là địa chỉ # chứa trạng thái truy vấn trả về. Trạng thái truy vấn sẽ được ghi vào # này. Giá trị 1 - thành công. Giá trị 0 - thất bại
- {#_result} là địa chỉ # chứa kết quả trả về.
- {c1},{c2},... là các cột dữ liệu gửi lên

Ví dụ:
![](https://oustittl.sirv.com/documents/img011.png)