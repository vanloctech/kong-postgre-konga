# Install docker

Gỡ cài đặt các version cũ của docker (nếu có)

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Cài đặt docker
```shell
sudo apt install docker.io
```

Enable docker
```shell
sudo systemctl enable --now docker
```

Cấp quyền cho người dùng
```shell
sudo usermod -aG docker <username>
```

Kiểm tra docker version

```shell
docker --version
```

Hướng dẫn cài đặt docker compose:

```shell
https://docs.docker.com/compose/install/
```
