# Setting Up Node Exporter

## Download Node Exporter

Begin by downloading Node Exporter using the wget command:

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
```

Note: Ensure you are using the latest version of Node Exporter and the correct architecture build for your server. The provided link is for amd64.

## Extract the Contents

After downloading, extract the contents with the following command:

```
tar xvf node_exporter-1.7.0.linux-amd64.tar.gz
```

## Move the Node Exporter Binary

Change to the directory and move the node_exporter binary to /usr/local/bin:

```
cd node_exporter-1.7.0.linux-amd64

sudo cp node_exporter /usr/local/bin
```

Then, clean up by removing the downloaded tar file and its directory:

```
rm -rf ./node_exporter-1.7.0.linux-amd64
```

## Create a Node Exporter User

Create a dedicated user for running Node Exporter:

```
sudo useradd --no-create-home --shell /bin/false node_exporter
```

Assign ownership permissions of the node_exporter binary to this user:

```
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

## Configure the Service

To ensure Node Exporter automatically starts on server reboot, configure the systemd service:

```
sudo nano /etc/systemd/system/node_exporter.service
```

Then, paste the following configuration:

```
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

Save and exit the editor.

## Enable and Start the Service

Reload the systemd daemon:

```
sudo systemctl daemon-reload
```

Enable the Node Exporter service:

```
sudo systemctl enable node_exporter
```

Start the service:

```
sudo systemctl start node_exporter
```

To confirm the service is running properly, check its status:

```
sudo systemctl status node_exporter.service
```
