# Grafana Installation

The following installation steps are tested and verified using `Ubuntu 20.04`. This procedure helps to,

* Install dependencies
* Add Grafana repository
* Enable Grafana system services to run on startup

### Installation steps

1. The following script helps in swift Grafana Installation,\
   [https://gitlab.com/PBSA/tools-libs/infrastructure/devops-misc-issues/-/blob/main/scripts/grafana/grafana-install.sh](https://gitlab.com/PBSA/tools-libs/infrastructure/devops-misc-issues/-/blob/main/scripts/grafana/grafana-install.sh)

<details>

<summary>Installation script</summary>

```
#!/bin/bash
echo "Install dependencies and adding GPG key for Grafana/n"
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
sudo wget -q -O /usr/share/keyrings/grafana.key https://packages.grafana.com/gpg.key

echo "Adding the stable Grafana repository/n"
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list

echo "Updating system & installing Grafana/n"
sudo apt-get update
sudo apt-get install grafana

echo "Starting Grafana & Enabling Grafana on startup/n"
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl status grafana-server
sudo systemctl enable grafana-server.service

echo "Grafana is now running on http://localhost:3000/"
echo "Login with default credentials admin/admin and you will be prompted to change the admin login"
```

</details>

2\. Copy and paste the above script into a file name: `grafana-install.sh`&#x20;

3\. Run the script with the below cli

```
sudo sh grafana-install.sh
```

