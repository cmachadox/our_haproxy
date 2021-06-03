# haproxy
**By: Clemente Machado**
![image](https://user-images.githubusercontent.com/30154971/120698673-be758780-c485-11eb-84e9-6dce872d6f5b.png)

```
# docker-compose up -d
```
## Dashboard haproxy

http://seu-ip:70

## Caso queira usar em failover com keepalived

**Instalar o pacote no server 01 e 02**
```
# yum install -y keepalived 
```

**Config Server 1** 
```
# vim /etc/keepalived/keepalived.conf
```
```
vrrp_script chk_haproxy {
  script "pidof haproxy"
  interval 2
}

vrrp_instance VI_1 {
  interface eth0 # nome da sua placa de rede
  state MASTER
  virtual_router_id 51
  priority 100
  virtual_ipaddress {
          192.168.0.100
  }
  track_script {
    chk_haproxy
  }
}

```

**Config Server 2** 
```
# vim /etc/keepalived/keepalived.conf
```
```
vrrp_script chk_haproxy {
  script "pidof haproxy"
  interval 2
}

vrrp_instance VI_1 {
  interface eth0 # nome da sua placa de rede
  state BACKUP
  virtual_router_id 51
  priority 50
  virtual_ipaddress {
          192.168.0.100
  }
  track_script {
    chk_haproxy
  }
}

```
![image](https://user-images.githubusercontent.com/30154971/120698974-2926c300-c486-11eb-8b45-28c1d17acf69.png)

![image](https://user-images.githubusercontent.com/30154971/120699033-42c80a80-c486-11eb-84a8-3bb1e9bb77be.png)
