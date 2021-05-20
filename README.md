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

### Tạo gateway api giữa các microservice

VD: có 2 microservice là:
```shell
service123.com/users

service456.com/products
```

Tiếp theo vào tab Services -> tạo service theo hình dưới

![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)

