# Foundations of RISCV64 Linux Debugging, Disassembling, and Reversing
- 너구리 순진한 맛의 연습 공간

## Host
- 아래 명령을 이용해 QEMU를 설치한다.
```shell
apt install \
  qemu-system-misc \
  u-boot-qemu
```

## QEMU

### 데비안 이미지 다운로드
```shell
mkdir -p .qemu
wget -O .qemu/debian-13-nocloud-riscv64.qcow2 \
  https://cloud.debian.org/images/cloud/trixie/latest/debian-13-nocloud-riscv64.qcow2
```

### 프로그램 설치
- QEMU를 실행하고 ***root*** 계정으로 로그인 한다.
- 아래 명령을 이용해 QEMU 상에서 개발 환경을 구성한다.
```shell
apt update
apt dist-upgrade -y
apt install -y \
    build-essential \
    gdb \
    gdbserver \
    libunwind-dev \
    openssh-server \
    vim \
    wget
apt clean

mkdir -p /mnt/iHDD00

FSTAB=$(
    cat <<EOF
iHDD00 /mnt/iHDD00 9p virtio,version=9p2000.L 0 0
/mnt/iHDD00 /home/debian/iHDD00 none bind,defaults,nofail 0 0
EOF
)
echo "${FSTAB}" >>/etc/fstab

username=debian
password=debian
adduser --comment "" --disabled-password ${username}
echo "${username}:${password}" | chpasswd
adduser debian sudo

GDBINIT=$(
    cat <<EOF
set auto-load local-gdbinit on
add-auto-load-safe-path ./
EOF
)
echo "${GDBINIT}" >/home/${username}/.gdbinit

chown ${username}:${username} -R /home/${username}

history -c && shutdown -h now
```

## Enjoy
* ID: debian / PW: debian
