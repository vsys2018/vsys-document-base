#### Hệ thống API mới 2020

Những thay đổi chính:

- Cải thiện vấn đề đọc ghi API query. Các thao tác tạo mới, xóa, sửa API được đảm bảo.
- Thêm các user và quyền quản trị.
- Cho phép xuất dữ liệu cho một dịch vụ khác qua REST API. Ví dụ cho Bubble.
- Cho phép xem report trực tiếp dựa trên dữ liệu đầu ra của API.

Chức năng:

1. Đăng nhập trang quản trị.

- Ví dụ api domain là `api-sample.com` ta nhập địa chỉ `api-sample.com/login` hoặc `api-sample.com/manage` để đăng nhập vào hệ thống.

  ![](https://oustittl.sirv.com/data-controller-api/login.png)

2. Giao diện trang điều khiển.

   ![](https://oustittl.sirv.com/data-controller-api/main.png)

3. Tạo mới api.

- Tại giao diện điều khiển bấm "Create API"

  ![](https://oustittl.sirv.com/data-controller-api/create-api.png)

- Nhập tên API vào ô API name. Lưu ý: tên api phải là chữ hoặc số viết liền không dấu hoặc phân cách bằng dấu gạch "-".
- Nhập nội dung query vào ô Query. Các tham số cần truyền vào được đánh dấu bằng dấu {{ }}. Ví dụ:

  ```
  SELECT `name` AS `Tên`, note AS `Ghi chú`
  FROM `something`
  LIMIT {{c1}} , {{c2}};
  ```

  Các tham số này có thể là query trên URL GET như `api-sample.com/report/test-api?c1=10&c2=20` hoặc tham số POST body `{"c1":10, "c2":20}`.

- Mục Accept for web. Check vào đây để cho phép bên thứ 3 có thể lấy dữ liệu theo đường REST API. Mặc định API chỉ phục vụ cho IO.
- Mục Accept table display. Check vào đây để cho phép xuất báo cáo trực tiếp theo dạng table.

4. Chỉnh sửa, xóa api.

   ![](https://oustittl.sirv.com/data-controller-api/edit.png)

- Bấm vào icon Config (1) để sửa API.
- Bấm vào icon Delete (2) để xóa API.

5. Đổi mật khẩu.

   ![](https://oustittl.sirv.com/data-controller-api/user.png)

   Bấm vào icon avata ở góc trên cùng. Chọn Setting.

   ![](https://oustittl.sirv.com/data-controller-api/change-pass.png)

   Ở tab Change Password ta điền mật khẩu cũ và nhập mật khẩu mới. Cuối cùng bấm Change Password để thay đổi mật khẩu.

6. Tạo user

   ![](https://oustittl.sirv.com/data-controller-api/create-user.png)

- User này chủ yếu để login vào hệ thống và phục vụ cho IO.
- Điền vào các trường Username và Password.
- Phần Privilege để phân quyền cho user. Gồm 2 quyền chính: Publisher được phép xem, tạo mới, sửa, xóa API. Viewer chỉ được phép xem, không tạo mới hay xóa, sửa API được.

7. Cấu trúc REST API.

- Phục vụ cho IO:

  ```
   GET /api/v1/{api_name}
  ```

  ```
   POST /api/v1/{api_name}
  ```

- Phục vụ cho các bên thứ 3 ví dụ Bubble hoặc app. Để chạy được chức năng này ta phải bật "Accept for web" cho API tương ứng.

  ```
   GET /api/v2/{api_name}
  ```

  ```
   POST /api/v2/{api_name}
  ```

8. Báo cáo trực tiếp.

   ![](https://oustittl.sirv.com/data-controller-api/report.png)

   Để xem được báo cáo trực tiếp ta cần bật chức năng "Accept table display" cho API muốn xem. Sau đó vào địa chỉ url.

   ```
    GET /report/{api_name}?{query}
   ```

   Trong đó `api_name` là tên API muốn xem. `query` là các tham số muốn truyền vào. Ví dụ: `api-sample.com/report/test-api?c1=10&c2=20`

9. Tạo page theo LIMIT cho bubble

   > Tham khảo bubble `test-new-api`

- Bước 1: Tạo API connector. Với các tham số như hình.

  ![](https://oustittl.sirv.com/data-controller-api/api-connector.png)

- Bước 2: Truyền tham số `c1` và `c2` vào api. Để tách page ta dùng `LIMIT c1, c2` ở query. Trong đó `c1` là điểm bắt đầu, `c2` là số lượng phần tử. Công thức để tính điểm bắt đầu dựa trên số trang: `(page - 1) * rows`

  ![](https://oustittl.sirv.com/data-controller-api/data-source.png)
