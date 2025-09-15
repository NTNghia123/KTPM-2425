FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Cập nhật, cài openssh-server, sudo, tools cơ bản
RUN apt-get update && apt-get install -y \
    openssh-server sudo ca-certificates curl wget nano less \
 && rm -rf /var/lib/apt/lists/*

# Tạo thư mục cho sshd
RUN mkdir /var/run/sshd

# Tạo user dev, đặt mật khẩu (thay "DevPass123" bằng mật khẩu bạn chọn)
RUN useradd -m -s /bin/bash dev \
 && echo "dev:DevPass123" | chpasswd \
 && adduser dev sudo

# Cho phép dev dùng sudo không cần password (tiện cho lab; nếu production thì KHÔNG làm)
RUN echo "dev ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Bật password auth để SSH bằng mật khẩu (mục đích lab). Bạn có thể chuyển sang key auth sau.
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config \
 && sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config || true

EXPOSE 22 5901

CMD ["/usr/sbin/sshd","-D"]
