# Loki Installation

Grafana is only for viewing Dashboard. Loki has to be installed in the same system in order to feed the logs into grafana.

The following steps are used for Loki setup,

1. The following script is used in the installation\
   [https://gitlab.com/PBSA/tools-libs/infrastructure/devops-misc-issues/-/blob/main/scripts/loki/loki-install.sh](https://gitlab.com/PBSA/tools-libs/infrastructure/devops-misc-issues/-/blob/main/scripts/loki/loki-install.sh)

<details>

<summary>Installation script</summary>

```
#!/bin/bash
echo "Updating System & Installing Dependencies"
sudo apt update && sudo apt dist-upgrade -y
sudo apt install unzip -y

echo "Creating /etc/loki"
mkdir /etc/loki
cd /etc/loki

echo "Fetching Loki version 2.6.1"
curl -O -L "https://github.com/grafana/loki/releases/download/v2.6.1/loki-linux-amd64.zip"
# extract the binary
unzip "loki-linux-amd64.zip"
#make sure it is executable
chmod a+x "loki-linux-amd64"
sudo cp loki-linux-amd64 /usr/local/bin/loki

echo "Adding config.yaml"
sudo tee /etc/loki/config.yaml<<EOF
auth_enabled: false

server:
  http_listen_port: 3100
  grpc_listen_port: 9096

common:
  path_prefix: /data/loki
  storage:
    filesystem:
      chunks_directory: /data/loki/chunks
      rules_directory: /data/loki/rules
  replication_factor: 1
  ring:
    instance_addr: 127.0.0.1
    kvstore:
      store: inmemory

schema_config:
  configs:
    - from: 2020-10-24
      store: boltdb-shipper
      object_store: filesystem
      schema: v11
      index:
        prefix: index_
        period: 24h
EOF

echo "Creating systemd service"
sudo tee /etc/systemd/system/loki.service<<EOF
[Unit]
Description=Loki service
After=network.target

[Service]
Type=simple
User=root

ExecStart=/usr/local/bin/loki -config.file /etc/loki/config.yaml
[Install]
WantedBy=multi-user.target
EOF

echo "Enabling loki on startup, and starting the service"
sudo systemctl daemon-reload
sudo systemctl enable loki.service
sudo systemctl start loki.service
sudo systemctl status loki.service

```

</details>

2\.  Copy and paste the above script into a file name: `loki-install.sh`

3\. Run the script with below CLI,

```
sudo sh loki-install.sh
```

### To Configure NGINX proxy

If the deployment is running on a cloud/remote, the following document can be use to configure an nginx proxy.

{% embed url="https://dev.to/ruanbekker/running-loki-behind-nginx-reverse-proxy-1699" %}
Document to Install NGINX
{% endembed %}

{% hint style="info" %}
Ignore Loki configuration in above document as the Loki installation steps are completed using the script.
{% endhint %}
