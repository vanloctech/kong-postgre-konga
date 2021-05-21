# Cài đặt và chạy Kong gateway api, db Postgre và Konga UI

### Yêu cầu

Cài đặt docker: Hướng dẫn trong file docker-install.md

### Cài đặt kong, csdl postgre và konga ui

Clone repo này về hoặc tải về file docker-compose.yml ở trên hoặc tạo file docker-compose.yml với nội dung file tương tự như trên

```shell
git clone https://github.com/vanloctech/kong-postgre-konga
```

Truy cập vào thư mục "kong-postgre-konga"

```shell
cd kong-postgre-konga
```

Chạy docker-compose

```shell
docker-compose up -d
```

Hệ thống sẽ tự động pull các image docker nếu chưa có và tự động tạo và run các container tương ứng.

Trường hợp trong repo này sử dụng cdsl `postgre` cho `kong gateway api` với giá trị các biến là:

- Tên database kong sử dụng: kong
- Tài khoản: kong
- Mật khẩu: kong

Với `konga` cũng sử dụng csdl trên nhưng database có tên là: konga

Sau khi chạy hoàn tất output có thể như sau:

```shell
Creating network "kong_kong-net" with the default driver
Creating kong_db_1 ... done
Creating kong_kong-migrations_1    ... done
Creating kong_konga-prepare_1      ... done
Creating kong_kong_1               ... done
Creating kong_kong-migrations-up_1 ... done
Creating kong_konga_1              ... done
```

"http://localhost:8000" là đường dẫn kong proxy (hiện tại sẽ chưa có gì vì chưa setup services và routes)

"http://localhost:8001" là đường dẫn kong admin

Kiểm tra các container docker của các image có lỗi không sử dụng:

```shell
docker logs <tên hoặc mã container>
```

### Setup kết nối konga với kong admin

Konga chạy port 1337, truy cập vào konga với đường dẫn: "http://localhost:1337"

Nếu lần đầu konga sẽ yêu cầu tạo tài khoản, điền các thông tin username, email, password để tạo tài khoản, sau đó login vào

Tiếp theo sẽ tạo connection giữa konga và kong admin, đặt tên cho connection và `Kong Admin URL` là: http://kong:8001

Nhấn vào kết nối, sẽ kết nối thành công

### Tạo gateway api giữa các microservice bằng konga

VD: có 2 microservice là:
```shell
service123.com/users

service456.com/products
```

Tiếp theo vào tab Services -> tạo service theo hình dưới

![alt text](https://github.com/vanloctech/kong-postgre-konga/blob/master/create-service.png?raw=true)

Tên service nhập tên muốn đặt
Field `host` nhập link: `service123.com/users`

Các field khác để trống hoặc để mặc định, sau đó bấm tạo

Tạo các service khác tương tự.

###### Tiếp theo tạo route cho service:

Tạo route cho service, click vào service muốn tạo chọn route -> chọ add route

Phần host có thể để trống mà không cần điền hoặc có thể điền tên host vd: example.com, lúc call api thì thêm header là
Host: example.com

Phần paths cũng có thể để trống hoặc điền vào các đường dẫn api muốn setup vd: /users. Các phần khác để mặc định và save lại

Khi dùng thì call api vd: localhost:8000/users với header là Host: example.com

### Tạo gateway api giữa các microservice bằng Admin api của Kong

Tham khảo:
```shell
https://docs.konghq.com/gateway-oss/2.4.x/getting-started/configuring-a-service/

https://docs.konghq.com/gateway-oss/2.4.x/auth/

https://docs.konghq.com/gateway-oss/2.4.x/admin-api/
```

### Bảo vệ service và hạn chế request từ client truy cập service

Phần này sẽ sử dụng `Rating limit` plugin của Kong

Thảm khảo:

```shell
https://docs.konghq.com/getting-started-guide/2.4.x/protect-services/
```

VD để giới hạn 5 request mỗi phút từ client, được phép lưu trữ local ta sử dụng Admin api như sau:
```shell
curl -i -X POST http://localhost:8001/plugins \
  --data name=rate-limiting \
  --data config.minute=5 \
  --data config.policy=local
```

Để hủy giới hạn sử dụng Update thông tin (enable or disable) hoặc delete plugin rate-limit
```shell
https://docs.konghq.com/gateway-oss/2.4.x/admin-api/#update-plugin
https://docs.konghq.com/gateway-oss/2.4.x/admin-api/#delete-plugin
```

### Cải thiện hiệu suất với Proxy caching

Phần này sẽ sử dụng `Proxy Caching plugin` của Kong
Tham khảo:

```shell
https://docs.konghq.com/getting-started-guide/2.4.x/improve-performance/
```

### Bảo mật service

Tham khảo:
```shell
https://docs.konghq.com/getting-started-guide/2.4.x/secure-services/
```

*Cảm thấy phần này không cần thiết vì các microservice có các tài khoản người dùng và cần sử dụng auth riêng.

### Cân bằng tải

Tham khảo:
```shell
https://docs.konghq.com/getting-started-guide/2.4.x/load-balancing/
```

### Bảo mật admin api
Tham khảo link dưới phần secure ở gần cuối
```shell
https://medium.com/devopsturkiye/kong-api-gateway-installing-configuring-and-securing-dfea423ee53c
```

