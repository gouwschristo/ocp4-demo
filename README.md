```
Create file: install-config.yaml

apiVersion: v1
baseDomain: demo.local 
compute:
- hyperthreading: Enabled   
  name: worker
  replicas: 0 
controlPlane:
  hyperthreading: Enabled   
  name: master 
  replicas: 3 
metadata:
  name: test 
networking:
  clusterNetwork:
  - cidr: 10.128.0.0/14 
    hostPrefix: 23 
  networkType: OpenShiftSDN
  serviceNetwork: 
  - 172.30.0.0/16
platform:
  none: {} 
pullSecret: '{"auths":{"fake":{"auth": "bar"}}}' 
sshKey: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFus6ewxa/gjUNYoW2Ow75WFE0PffvzjrDw4zHHsqpnuUOZjvHSdLz79qzuNeAeHCHA7QybwrXU1ddnlxkAHh8XecBgkoDOnmsfM3jfDEUjRr4UTzeaVa9iK+os9CjWxRiUcWCke2BbmMzHJ0sSyv/JOS3eapG85AE9pu7u853D8mv1nyJymXoTUJXiP9rKXSYVV01UPafD33WWXnwUPjAJSxSeN0raH5NrhO293+Cnd/lPMP6D6Vgn2ctExoz+bFiWDaglg2cP3nGn+siGUK0lsR+6qHUsNAbk5P79sqEUtCh3tawTAFGhZQRycpKi5LOPOddSkV8LKX/klexyQ/z root@boostrap.local' 


wget https://github.com/openshift/okd/releases/download/4.4.0-0.okd-2020-03-28-092308/openshift-client-linux-4.4.0-0.okd-2020-03-28-092308.tar.gz
wget https://github.com/openshift/okd/releases/download/4.4.0-0.okd-2020-03-28-092308/openshift-install-linux-4.4.0-0.okd-2020-03-28-092308.tar.gz

wget https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/31.20200210.3.0/x86_64/fedora-coreos-31.20200210.3.0-metal.x86_64.raw.xz
wget https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/31.20200210.3.0/x86_64/fedora-coreos-31.20200210.3.0-metal.x86_64.raw.xz.sig
mv fedora-coreos-31.20200210.3.0-metal.x86_64.raw.xz fcos.raw.xz
mv fedora-coreos-31.20200210.3.0-metal.x86_64.raw.xz.sig fcos.raw.xz.sig

tar xzvf openshift-client-linux-4.4.0-0.okd-2020-03-28-092308.tar.gz
tar xzvf openshift-install-linux-4.4.0-0.okd-2020-03-28-092308.tar.gz

mv rhcos-4.1.0-x86_64-metal-bios.raw.gz iso.raw.gz
mv ./kubectl ./oc /usr/local/bin/


./openshift-install create manifests
./openshift-install create ignition-configs

# Clear firewall and setup port-forwards
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -t nat -F
iptables -t mangle -F
iptables -F
iptables -X
echo 1 > /proc/sys/net/ipv4/ip_forward
sudo sysctl -w net.ipv4.conf.all.route_localnet=1
sudo iptables -t nat -A POSTROUTING -j MASQUERADE
# Forward from config -> boostrap
iptables -t nat -A PREROUTING -p tcp --dport 22623 -j DNAT --to-destination 192.168.1.111:22623
# forward from config -> master
iptables -t nat -A PREROUTING -p tcp --dport 6443 -j DNAT --to-destination 192.168.1.113:6443

# Serve ignition files via HTTP
python -m SimpleHTTPServer 80


Boot VM with image obtained from here:
https://builds.coreos.fedoraproject.org/prod/streams/stable/builds/31.20200310.3.0/x86_64/fedora-coreos-31.20200310.3.0-live.x86_64.iso

Specify the following kernel parameters when booting up - press: e
coreos.inst.install_dev=sda
coreos.inst.image_url=http://demo.local/fcos.raw.xz
coreos.inst.insecure

* boostrap node
coreos.inst.ignition_url=http://demo.local/boostrap.ign

* master node
coreos.inst.ignition_url=http://demo.local/master.ign

* worker node
coreos.inst.ignition_url=http://demo.local/worker.ign

# Extra notes
ssh core@server

Port 22623 -> boostrap
Port 6443  -> master

openssl s_client -connect api-int.test.demo.local:22623 -showcerts |less
timedatectl set-timezone Africa/Johannesburg

# Extra links
openshift-install-linux-4.4.0-0.okd-2020-03-28-092308.tar.gz
https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.1/latest/rhcos-4.1.0-x86_64-installer.iso
https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.1/latest/rhcos-4.1.0-x86_64-metal-bios.raw.gz
https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.1.38/openshift-install-linux.tar.gz
https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.1/latest/rhcos-4.1.0-x86_64-installer.iso
https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.1/latest/rhcos-4.1.0-x86_64-metal-bios.raw.gz

```
	
