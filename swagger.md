# Hướng dẫn cài đặt và viết document với swagger, openapi 3

Hướng dẫn dưới đây sẽ cài đặt swagger bằng docker nên yêu cầu cài docker

Giới thiệu swagger cần thiết sẽ có 2 công cụ là Swagger UI và Swagger Editor
- Swagger Editor dùng để viết document cho api, thường file export ra sẽ là file json hoặc yaml, hay open api 3
- Swagger UI dùng để xem file các file json, yaml,... mà Swagger Editor export ra. 
  Giúp FE xem được cách sử dụng api mà BE viết. 

### Swagger Editor

Cài đặt
```shell
docker pull swaggerapi/swagger-editor
docker run -d -p 80:8080 swaggerapi/swagger-editor #nen thay doi port khac vi port 80 thuong duoc cac dich vu khac su dung
```

Sau đó truy cập vào http://localhost để viết document

Ngoài ra còn một số tùy chọn cài đặt khác: https://github.com/swagger-api/swagger-editor

### Swagger UI

Cài đặt
```shell
docker pull swaggerapi/swagger-ui
docker run -p 80:8080 swaggerapi/swagger-ui #nen thay doi port khac vi port 80 thuong duoc cac dich vu khac su dung
```
hoặc clone repo này về https://github.com/swagger-api/swagger-ui

sau đó vào thư mục <repo_name>/dist mở file index.html với editor tìm dòng tương tự như `url: "https://petstore.swagger.io/v2/swagger.json",`
và sửa thành đường dẫn đến file json, yaml sau đó mở file index.html này bằng trình duyệt.