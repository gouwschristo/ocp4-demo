```
create install-config.yaml

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

tar xzvf openshift-client-linux-4.4.0-0.okd-2020-03-28-092308.tar.gz
tar xzvf openshift-install-linux-4.4.0-0.okd-2020-03-28-092308.tar.gz

cp ./kubectl ./oc /usr/local/bin/



https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.1/latest/rhcos-4.1.0-x86_64-installer.iso


./openshift-install create manifests
./openshift-install create ignition-configs

wget https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.1/latest/rhcos-4.1.0-x86_64-metal-bios.raw.gz
mv rhcos-4.1.0-x86_64-metal-bios.raw.gz iso.raw.gz

iptables -F
python -m SimpleHTTPServer 


coreos.inst.install_dev=sda
coreos.inst.image_url=http://demo.local/iso.raw.xz
coreos.inst.ignition_url=http://demo.local/bootstrap.ign



# Extra notes
openshift-install-linux-4.4.0-0.okd-2020-03-28-092308.tar.gz
https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.1/latest/rhcos-4.1.0-x86_64-installer.iso
https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/4.1/latest/rhcos-4.1.0-x86_64-metal-bios.raw.gz
https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.1.38/openshift-install-linux.tar.gz

```
