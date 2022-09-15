# Promtail agent Installation

The Grafana and Loki setup helps to feed logs into Loki which has to be visualized by Grafana.

If Grafana has to visualize the logs the Promtail agent has to be installed in the same system. The logs can be system logs, application logs, etc.,&#x20;

The below Installation script has to be edited and pointed towards the LOKI deployment that was done earlier. Depending on the setup the domain name / IP address of the Loki deployment has to be pointed out.

Below are the lines that must be edited in the installation script and following are the placeholders:

* `<url-of-your-loki-deployment>` - IP or Domain name of your Loki deployment
* `<name-of-your-log-files>` - This is used to separate your logs for queries in Grafana UI
* `<the-path-to-your-logs>` - This is the path of the logs you wish to be parsed by Loki/Grafana

The following script is used in the Promtail installation,\
[`https://gitlab.com/PBSA/tools-libs/infrastructure/devops-misc-issues/-/blob/main/scripts/loki/promtail-install.sh`](https://gitlab.com/PBSA/tools-libs/infrastructure/devops-misc-issues/-/blob/main/scripts/loki/promtail-install.sh)``

<details>

<summary>Installation Script</summary>

```
#!/bin/bash
echo "######################"
echo "# Full system update #"
echo "######################"
sudo apt update && sudo apt dist-upgrade -y

echo "#####################################"
echo "# Grabbing and installing promtail! #"
echo "#####################################"
echo "Is there a newer release? Please check at https://github.com/grafana/loki/releases"
sleep 2

curl -O -L https://github.com/grafana/loki/releases/download/v2.6.1/promtail-linux-amd64.zip
unzip promtail-linux-amd64.zip

sudo mv promtail-linux-amd64 /usr/local/bin/promtail
echo "Version checking: "
promtail --version

echo "Configuring promtail config & data dir"
sudo mkdir /etc/promtail
sudo mkdir -p /data/promtail

echo "Adding default config for promtail - Edit THIS!"
sleep 2

sudo tee /etc/promtail/config.yaml<<EOF
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /data/promtail/positions.yaml

clients:
  - url: <url-of-your-loki-deployment>

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: <name-of-your-logs>
      __path__: <path-of-your-log-files>

#Use if multiple paths for logs are needed from one system
#- job_name: nginx
#  static_configs:
#  - targets:
#      - localhost
#    labels:
#      job: <name-of-your-logs>
#      __path__: <path-of-your-log-files>

EOF


echo "Configuring systemd service"
sudo tee /etc/systemd/system/promtail.service<<EOF
[Unit]
Description=Promtail service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/promtail -config.file /etc/promtail/config.yaml

[Install]
WantedBy=multi-user.target
EOF

echo "Ensure you edit /etc/promtail/config.yaml before using the following commands to start the promtail agent!"

echo "To start promtail run: sudo systemctl start promtail.service"
echo "To check system service status: systemctl status promtail.service"
```

</details>

### Steps to Enable Promtail on startup&#x20;

After the script has completed successfully, to start the actual Promtail service the following commands has to be executed,

* `sudo systemctl daemon-reload`
* `sudo systemctl enable promtail.service`
* `sudo systemctl start promtail.service`

{% hint style="info" %}
If there are no errors from **`sudo systemctl status promtail.service`**&#x20;

Cli execution, it denotes that the logs are being parsed properly.
{% endhint %}

