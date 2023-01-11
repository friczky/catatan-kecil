
<img src="https://i.ibb.co/n6gKkDj/image.png" alt="image" border="0">

Berikut adalah cara membuat Monitoring Server menggunakan Node Exporter , Prometheus dan Grafana.

### Install Node Exporter

- Link Download Node Exporter
1. buka link : https://prometheus.io/download#node_exporter
2. kemudian pilih yang berformat tar dan pilih sesuai sistem operasi , contohnya linux/amd64
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

- Buat Service Node Exporter dengan cara menjalankan perintah dibawah ini.
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

<img src="https://i.ibb.co/Svm4VWr/Screenshot-2023-01-11-13-33-11-1920x2160.png" alt="Screenshot-2023-01-11-13-33-11-1920x2160" border="0">

- Jalankan Service Node Exporter
```
sudo systemctl start node_exporter
```

- Aktifkan service node_exporter agar tetap jalan ketika di restart
```
sudo systemctl enable node_exporter
```

- Buka Service menggunakan browser
1. ketik url dengan ip address server , contohnya http://localhost:9100
2. port node_exporter : 9100

- Tampilan node_exporter didalam browser

<img src="https://i.ibb.co/pyC6hG1/Screenshot-2023-01-11-13-06-17-1920x2160.png" alt="Screenshot-2023-01-11-13-06-17-1920x2160" border="0">

### Install Prometheus ( docker )

<img src="https://i.ibb.co/BzfFpmd/image.png" alt="image" border="0">

- Pull docker image
```
sudo docker pull prom/prometheus
```

- Jalankan docker container
```
sudo docker run -d --name prometheus --restart always --net host /etc/localtime:/etc/localtime:ro prom/prometheus
```

- config prometheus.yml
```
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]
  - job_name: "node_exporter"
    static_configs:
      - targets: ["localhost:9100"]
```

- copy config diatas menggunakan perintah berikut di dalam terminal
```
sudo docker exec -ti prometheus vi /etc/prometheus/prometheus.yml
```

- hapus semua isinya dan ganti dengan config diatas

<img src="https://i.ibb.co/zWvMn9C/Screenshot-2023-01-11-13-34-11-1920x2160.png" alt="Screenshot-2023-01-11-13-34-11-1920x2160" border="0">

- Buka service menggunakan browser
1. port default promotheus : 9090
2. url : http://localhost:9090

- Buka menu targets maka dan harusnya akan muncul 2 targets yaitu node_exporter dan promotheus seperti ini.

<img src="https://i.ibb.co/KD4ZjSv/image.png" alt="image" border="0">

### Install Grafana ( docker )

<img src="https://i.ibb.co/y68WqD5/image.png" alt="image" border="0">

- Pull docker image
```
sudo docker pull grafana/grafana
```

- Jalankan docker container
```
sudo docker run -d --name grafana --net host grafana/grafana
```
- Buka service menggunakan browser
1. grafana default port : 3000
2. url : http://localhost:3000
3. user : admin
4. pass : admin

<img src="https://i.ibb.co/09zmMw6/Screenshot-2023-01-11-13-18-34-1920x2160.png" alt="Screenshot-2023-01-11-13-18-34-1920x2160" border="0">

<img src="https://i.ibb.co/yqK3XCf/Screenshot-2023-01-11-13-18-53-1920x2160.png" alt="Screenshot-2023-01-11-13-18-53-1920x2160" border="0">

- Aktifkan Data Source di Configuration Datasource dan Pilih promotheus

<img src="https://i.ibb.co/NKtrGvg/Screenshot-2023-01-11-13-10-00-1920x2160.png" alt="Screenshot-2023-01-11-13-10-00-1920x2160" border="0">

- Masukan url promotheus

<img src="https://i.ibb.co/3FTTpSg/Screenshot-2023-01-11-13-17-01-1920x2160.png" alt="Screenshot-2023-01-11-13-17-01-1920x2160" border="0">

- Pilih Service Promotheus

<img src="https://i.ibb.co/rbZLfKf/Screenshot-2023-01-11-13-17-12-1920x2160.png" alt="Screenshot-2023-01-11-13-17-12-1920x2160" border="0">

<img src="https://i.ibb.co/dDFyDj0/Screenshot-2023-01-11-13-17-23-1920x2160.png" alt="Screenshot-2023-01-11-13-17-23-1920x2160" border="0">

- Pilih menu dashboard kemudian pilih Import

<img src="https://i.ibb.co/g3JxbtJ/Screenshot-2023-01-11-13-17-47-1920x2160.png" alt="Screenshot-2023-01-11-13-17-47-1920x2160" border="0">

- Import tampilan Dashboard dengan ID 11074

<img src="https://i.ibb.co/brGTcRS/Screenshot-2023-01-11-13-18-09-1920x2160.png" alt="Screenshot-2023-01-11-13-18-09-1920x2160" border="0">

- Buka dashboard yang sudah di import tadi dan akan tampil seperti ini.

<img src="https://i.ibb.co/NgvZ6yh/Screenshot-2023-01-11-13-19-17-1920x2160.png" alt="Screenshot-2023-01-11-13-19-17-1920x2160" border="0">
<img src="https://i.ibb.co/5F1yz4N/Screenshot-2023-01-11-13-30-47-1920x2160.png" alt="Screenshot-2023-01-11-13-30-47-1920x2160" border="0">
<img src="https://i.ibb.co/MB9KS5F/Screenshot-2023-01-11-13-31-45-1920x2160.png" alt="Screenshot-2023-01-11-13-31-45-1920x2160" border="0">




















