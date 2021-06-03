# haproxy
**By: Clemente Machado**

```
# docker-compose up -d
```


## Caso queira usar em failover


```
# yum install -y keepalived 

```


**Config Server 1** 
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

