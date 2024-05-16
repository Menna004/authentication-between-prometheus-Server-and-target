### ****The Node Exporter****

- Create a directory node_exporter

`mkdir /etc/node_exporter`

- Make the current directory node_exporter

`cd /etc/node_exporter `

 - Using open SSL to generate certificates
   
`sudo openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 -keyout node_exporter.key -out node_exporter.crt -subj "/C=US/ST=California/L=Oakland/O=MyOrg/CN=localhost" -addext "subjectAltName = DNS:localhost"
`
- Create a config.yaml file which contains:

`tls_server_config:
  cert_file: node_exporter.crt
  key_file: node_exporter.key`
  
- Update the service of node_exporter with TLS config 

`[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter --web.config="/etc/node_exporter/config.yaml"
[Install]
WantedBy=multi-user.target`

- Reload the daemon and start the node_exporter

`sudo systemctl daemon-reload
sudo systemctl restart node_exporter`

- we see Node Metrics publishing on HTTPS

![hhh](https://github.com/Menna004/authentication-between-prometheus-Server-and-target/assets/88343123/6b6b5038-4b62-41fc-be6b-afc11dc1ffca)

### ****Prometheus Server****

- Copy the node_exporter.crt file from the node exporter to the Prometheus server at /etc/prometheus

- Update Prometheus configuration file as below:

![{55FE0FD3-4B41-4A92-868B-3713BF71FAF9} png](https://github.com/Menna004/authentication-between-prometheus-Server-and-target/assets/88343123/7c4e7f22-9a8a-40e7-8f88-e58c4bfb82f6)
- Now you can restart the Prometheus 

- we see that the Node_Exporter is UP 

![yyy](https://github.com/Menna004/authentication-between-prometheus-Server-and-target/assets/88343123/c7d3d7ca-afcd-4ffb-b647-37b6e92ce54d)

