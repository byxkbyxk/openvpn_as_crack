版本：Ubuntu22.04、OAS2.13.1
1、安装完ubuntu，开启转发
# nano /etc/sysctl.conf
新增一行：net.ipv4.ip_forward = 1
生效：
# sysctl -p

2、安装oas
# apt update && apt -y install ca-certificates wget net-tools gnupg
# wget https://as-repository.openvpn.net/as-repo-public.asc -qO /etc/apt/trusted.gpg.d/as-repository.asc
# echo "deb [arch=amd64 signed-by=/etc/apt/trusted.gpg.d/as-repository.asc] http://as-repository.openvpn.net/as/debian jammy main">/etc/apt/sources.list.d/openvpn-as-repo.list
# apt update && apt -y install openvpn-as
（安装DCO：apt install -y openvpn-dco-dkms）（重启后要输入密码验证才会启用）

3、crack
# sudo apt install python3-pip unzip zip
# systemctl stop openvpnas
# mkdir /home/temp && cd /home/temp
# cp /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.10.egg{,.bak}
# cp /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.10.egg .
# unzip -q pyovpn-2.0-py3.10.egg 
# cd ./pyovpn/lic/
# mv uprop.pyc uprop2.pyc
# nano uprop.py
填入内容：
from pyovpn.lic import uprop2
old_figure = None
 
def new_figure(self, licdict):
      ret = old_figure(self, licdict)
      ret['concurrent_connections'] = 8888
      return ret
 
for x in dir(uprop2):
      if x[:2] == '__':
         continue
      if x == 'UsageProperties':
         exec('old_figure = uprop2.UsageProperties.figure')
         exec('uprop2.UsageProperties.figure = new_figure')
      exec('%s = uprop2.%s' % (x, x))
# python3 -O -m compileall uprop.py && mv __pycache__/uprop.*.pyc uprop.pyc
# cd ../../
# zip -rq pyovpn-2.0-py3.10.egg ./pyovpn ./EGG-INFO ./common
# mv ./pyovpn-2.0-py3.10.egg /usr/local/openvpn_as/lib/python/pyovpn-2.0-py3.10.egg
# systemctl start openvpnas
