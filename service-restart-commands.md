## ## Linux of Linux Commands which is used 

### Restart openvpn
```sh
service openvpn restart
```
### Restart prometheus
```sh
 sudo systemctl restart prometheus
 sudo systemctl status prometheus
```
### Restart grafana
```sh
sudo systemctl restart grafana-server
```
### Restart all the running and exited container
```sh
sudo docker restart $(docker ps -a -q)
```
