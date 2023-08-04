1、安装完ubuntu-server-22.04，开启转发
# sudo passwd root
# su
#apt install -y nano
# nano /etc/sysctl.conf
新增一行：net.ipv4.ip_forward = 1
生效：
# sysctl -p

2、安装oas
# apt update && apt -y install ca-certificates wget net-tools gnupg
# wget https://as-repository.openvpn.net/as-repo-public.asc -qO /etc/apt/trusted.gpg.d/as-repository.asc
# echo "deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/as-repository.asc] http://as-repository.openvpn.net/as/debian jammy main">/etc/apt/sources.list.d/openvpn-as-repo.list
# apt update && apt -y install openvpn-as
或离线安装
# apt update
# apt install -y bridge-utils dmidecode iptables iproute2 libc6 libffi7 libgcc-s1 liblz4-1 liblzo2-2 libmariadb3 libpcap0.8 libssl3 libstdc++6 libsasl2-2 libsqlite3-0 net-tools python3-pkg-resources python3-migrate python3-sqlalchemy python3-mysqldb python3-ldap3 sqlite3 zlib1g python3-netaddr python3-arrow python3-lxml python3-constantly python3-hyperlink python3-automat python3-service-identity python3-cffi python3-defusedxml libcap-ng0 libnl-3-200 libnl-genl-3-200
# dpkg -i openvpn-as-bundled-clients-28.deb openvpn-as_2.12.0-2e834031-Ubuntu22_amd64.deb

3、安装DCO
# apt install -y openvpn-dco-dkms
（输入密码）
# reboot
（重启后要输入密码验证才会启用 Enroll MOK）

4、crack
# systemctl stop openvpnas
# cd /usr/local/openvpn_as/lib/python
# rm /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.10.egg
# mv pyovpn-2.0-py3.10.egg /usr/local/openvpn_as/lib/python
（pyovpn-2.0-py3.10.egg上传到用户目录，再替换）
# cd /usr/local/openvpn_as/bin
# ./ovpn-init

DELETE
yes
