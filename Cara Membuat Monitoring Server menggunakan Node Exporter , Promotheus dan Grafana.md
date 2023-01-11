
Berikut adalah cara membuat Monitoring Server menggunakan Node Exporter , Promotheus dan Grafana.

### Install Node Exporter

- Link Download Node Exporter
buka link : https://prometheus.io/download#node_exporter
kemudian pilih yang berformat tar dan pilih sesuai sistem operasi , contohnya linux/amd64
```
wget url
```

- Extract Node Exporter
```
tar xvfz node_exporter-*.*-amd64.tar.gz
```

- Copy Node Exporter
```
cd node_exporter
cp node_exporter /usr/local/bin/

```

- Buat Service Node Exporter
```
sudo nano /etc/systemd/system/node_exporter.service
```
isi dengan code berikut :
```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

- Jalankan Service Node Exporter
```
sudo systemctl start node_exporter
```

- Buka Service menggunakan browser
ketik url dengan ip address server , contohnya http://localhost:9100
port node_exporter : 9100


### Install Promotheus ( docker )

- Pull docker image
```
docker pull prom/prometheus
```

- Jalankan docker container
```
docker run -d --name promotheus --restart always --net host -v promotheus-config:/etc/promotheus -v promotheus-data:/promotheus -v /etc/localtime:/etc/localtime:ro prom/promotheus
```

- Buka service menggunakan browser
port default promotheus : 9090
url : http://localhost:9090


### Install Grafana ( docker )

- Pull docker image
```
docker pull grafana/grafana
```

- Jalankan docker container
```
```
- Buka service menggunakan browser
grafana default port : 3000
url : http://localhost:3000
user : admin
pass : admin

























