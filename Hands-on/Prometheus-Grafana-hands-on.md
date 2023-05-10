<div align="center">
<img src=https://static.wixstatic.com/media/1c706c_a5df0ad56f894928bf858a74ba744b32~mv2.png/v1/fit/w_2500,h_1330,al_c/1c706c_a5df0ad56f894928bf858a74ba744b32~mv2.png width="400" height="200">
 </div>

# <div align="center"> PROMETHEUS/GRAFANA HANDS-ON </p>

# <div align="center"> DevOps Instructor-led Training </div>

# <div align="right"> $`\textcolor{brown}{\text{Contact us: }}`$  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; </div>

<div align="right"> T O A C C E L E R A T E Y O U R C A R E E R G R O W T H </div>

### <div align="right"> For questions and more details: </div>

<div align="right"> <img src=https://w7.pngwing.com/pngs/759/922/png-transparent-telephone-logo-iphone-telephone-call-smartphone-phone-electronics-text-trademark-thumbnail.png width="20" height="20"> +91 98712 72900 </div>

<div align="right"> <img src=https://pbs.twimg.com/profile_images/1450734615946219520/jmBHQRRa_400x400.jpg width="20" height="20"> https://www.thecloudtrain.com </div>

<div align="right"> <img src=https://icons.iconarchive.com/icons/martz90/circle/512/email-icon.png width="20" height="20"> support@thecloudtrain.com </div>

<div align="right"> <img src=https://png.pngtree.com/png-vector/20221018/ourmid/pngtree-whatsapp-icon-png-image_6315990.png width="20" height="20"> +91 98712 72900 </div>

## PROMETHEUS & GRAFANA

Refer [**https://github.com/vistasunil/prometheus-grafanna.git**](https://github.com/vistasunil/prometheus-grafanna.git) for all codes referred in this session

## _Prometheus Installation_

### Step 1: Install Prometheus using below commands:

```
sudo apt update

PROMETHEUS_VERSION="2.40.5"

wget https://github.com/prometheus/prometheus/releases/download/v${PROMETHEUS_VERSION}/prometheus-${PROMETHEUS_VERSION}.linux-amd64.tar.gz

tar -xzvf prometheus-${PROMETHEUS_VERSION}.linux-amd64.tar.gz

cd prometheus-${PROMETHEUS_VERSION}.linux-amd64/

# if you just want to start prometheus as root

#./prometheus --config.file=prometheus.yml

# create user

sudo useradd --no-create-home --shell /bin/false prometheus

# create directories

sudo mkdir -p /etc/prometheus

sudo mkdir -p /var/lib/prometheus

# set ownership

sudo chown prometheus:prometheus /etc/prometheus

sudo chown prometheus:prometheus /var/lib/prometheus

# copy binaries

sudo cp prometheus /usr/local/bin/

sudo cp promtool /usr/local/bin/

sudo chown prometheus:prometheus /usr/local/bin/prometheus

sudo chown prometheus:prometheus /usr/local/bin/promtool

# copy config

sudo cp -r consoles /etc/prometheus

sudo cp -r console_libraries /etc/prometheus

sudo cp prometheus.yml /etc/prometheus/prometheus.yml

sudo chown -R prometheus:prometheus /etc/prometheus/consoles

sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries

#setup systemd by adding below lines to file /etc/systemd/system/prometheus.service

sudo vim /etc/systemd/system/prometheus.service

## Add below lines to this file

[Unit]

Description=Prometheus

Wants=network-online.target

After=network-online.target

[Service]

User=prometheus

Group=prometheus

Type=simple

ExecStart=/usr/local/bin/prometheus \

--config.file /etc/prometheus/prometheus.yml \

--storage.tsdb.path /var/lib/prometheus/ \

--web.console.templates=/etc/prometheus/consoles \

--web.console.libraries=/etc/prometheus/console_libraries

[Install]

WantedBy=multi-user.target

##Save and exit the file with :wq

sudo systemctl daemon-reload

sudo systemctl enable prometheus

sudo systemctl start prometheus
```

### Step 2: Once started, verified the Prometheus service using below command:

`sudo service prometheus status`

![image](https://user-images.githubusercontent.com/37858762/236439180-c28abe6e-a92f-4900-bbe2-976517afec69.png)

### Step 3: Access Prometheus UI on url below. Make sure to open firewall port for 9090:

_http://\<PUBLIC\_IP:9090/_

![image](https://user-images.githubusercontent.com/37858762/236439146-f146209d-2ec5-4270-8daa-069558c50f4a.png)

### Step 4: Run Sample Queries to see if Prometheus working fine

`up`

`up{job="prometheus"}`

![image](https://user-images.githubusercontent.com/37858762/236439123-c794f328-1365-4188-9008-6f6d787f2675.png)

![image](https://user-images.githubusercontent.com/37858762/236439107-c52701d4-98db-4dfb-9d01-2e4b6880f18d.png)

## _Prometheus Configuration_

### Step 1: Prometheus default configuration file looks like below:

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
```

## _Monitoring Nodes Using Node Exporter_

### Step 1: Install Node exporter using below commands:

```
NODE_EXPORTER_VERSION="1.5.0"

wget https://github.com/prometheus/node_exporter/releases/download/v${NODE_EXPORTER_VERSION}/node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64.tar.gz

tar -xzvf node_exporter-${node_exporter_VERSION}.linux-amd64.tar.gz

cd node_exporter-${node_exporter_VERSION}.linux-amd64

sudo cp node_exporter /usr/local/bin

# create user

sudo useradd --no-create-home --shell /bin/false node_exporter

sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter

# Setup systemd by adding below lines to file /etc/systemd/system/node_exporter.service

sudo vim /etc/systemd/system/node_exporter.service

## Add below lines to this file

[Unit]

Description=Node Exporter

Wants=network-online.target

After=network-online.target

[Service]

User=node_exporter

Group=node_exporter

Type=simple

ExecStart=/usr/local/bin/node_exporter

[Install]

WantedBy=multi-user.target

# enable node_exporter in systemctl

sudo systemctl daemon-reload

sudo systemctl start node_exporter

sudo systemctl enable node_exporter

# Append below to /etc/prometheus/prometheus.yml under scrape_configs section:

sudo vim /etc/prometheus/prometheus.yml

- job_name: 'node_exporter'

scrape_interval: 5s

static_configs:

- targets: ['localhost:9100']
```

![image](https://user-images.githubusercontent.com/37858762/236439047-ae697483-f42d-48ee-b97d-725ac40d9d43.png)

### Step 2: Save the config file and restart Prometheus process with below command:

`ps aux | grep prometheus`

`sudo kill -HUP <PID>`

![image](https://user-images.githubusercontent.com/37858762/236439001-c486398f-a03f-46d1-99cc-87f44cd6fc4d.png)

## _Querying Metrics_

### Step 1: Use below queries to see different metrics and results
```
# Query total http requests with various options
prometheus_http_requests_total
prometheus_http_requests_total{job=~".*metheus"}
prometheus_http_requests_total{job=~".*metheus"}[5m]
prometheus_http_requests_total{job=~".*metheus", instance="localhost:9090"}[5m]
prometheus_http_requests_total{code=~"2.."}
prometheus_http_requests_total{code=~"4.."}
prometheus_http_requests_total{job=~".*metheus", method="get"}[5m]

# return per sec rate of each time series inlast 5 mins
rate(prometheus_http_requests_total[5m]) 

# sum of all time series by jobs in last 5 mins
sum(rate(prometheus_http_requests_total[5m])) by (job)

# used memory on node(s) KB/MB/GB
node_memory_MemTotal_bytes - node_memory_MemFree_bytes  (node_memory_MemTotal_bytes - node_memory_MemFree_bytes)/1024
(node_memory_MemTotal_bytes - node_memory_MemFree_bytes)/1024/1024

# top 3 cpu users group per application
topk(3,sum(rate(node_cpu_seconds_total[5m])) by (app)) 
```

## Alert Setup

## _Setup Slack_

### Step 1: Go to [**Slack Page**](https://slack.com/) and sign-up by clicking on **TRY FOR FREE**. If you already have an account then sign-in directly:

![image](https://user-images.githubusercontent.com/37858762/236438707-6bf63191-cec3-4f85-853f-47059afd171f.png)

### Step 2: You can sign-up with an email ID or use your Google or Apple account. I used my Google account here:

![image](https://user-images.githubusercontent.com/37858762/236438688-d4f37b51-08aa-4228-9d75-31c66342c7d5.png)

### Step 3:Create a workspace:

![image](https://user-images.githubusercontent.com/37858762/236438670-1bfea38f-3a10-4a7c-90cf-75ecddbf8019.png)

### Step 4: Choose a name of your choice. I use **cloudtrain** :

![image](https://user-images.githubusercontent.com/37858762/236438653-fe79c708-b123-4165-badc-f88c8c8d54c6.png)

### Step 5: Add team mates, if you want or skip. I skipped here:

![image](https://user-images.githubusercontent.com/37858762/236438623-f6284710-7e77-4cd8-b355-535bfb2bf5fd.png)

### Step 6: Enter anything in doing here. This info will create a slack channel for you, where we will send on our Prometheus alerts by setting hook. I am using ' **devops'** here:

![image](https://user-images.githubusercontent.com/37858762/236438605-5bf7c143-29da-435a-9c3b-c5eec2d1ef0a.png)

### Step 7: Your workspace is created successfully:

![image](https://user-images.githubusercontent.com/37858762/236438579-d1b069ee-4bfc-4b77-95ca-3a3e47ac224a.png)

### Step 8: Let's setup hook to this **devops** channel to send alerts. Go to **[Slack Apps](https://api.slack.com/apps?new_app=1)** and choose to create app from scratch:

![image](https://user-images.githubusercontent.com/37858762/236438549-7228ac07-d28c-4b42-a28f-a4f295cdf07c.png)

### Step 9: Give a name to your app, say ' **prometheus\_alerts'** and choose workspace – **cloudtrain** here. Create app:

![image](https://user-images.githubusercontent.com/37858762/236438515-95883f23-80d9-45bb-9505-8633677945f0.png)

### Step 10: You land up on below page. Choose **Incoming Webhooks** to create and setup one:

![image](https://user-images.githubusercontent.com/37858762/236438495-8d5ea972-49f7-45b6-8f24-b08c06fecd12.png)

### Step 11: Activate the webhook by turning the toggle flag **on** :

![image](https://user-images.githubusercontent.com/37858762/236438463-6c74a18b-5d3f-467b-b6c6-e74070ac711d.png)

### Step 12: Add a new Webhook URL by clicking as shown below:

![image](https://user-images.githubusercontent.com/37858762/236438444-e8f4f1cf-3634-4116-9fb5-7272c14688ae.png)

### Step 13: Choose your channel to add this app's webhook and click allow:

![image](https://user-images.githubusercontent.com/37858762/236438429-a8328d85-16c3-4794-ba40-ae6bb1b8e29d.png)

### Step 14: Webhook is created now. Copy it from here and keep it to place in alert manager yaml config file:

![image](https://user-images.githubusercontent.com/37858762/236438376-d432df2c-b160-46a5-9571-8ca1b6538ef7.png)

### Step 15: Your Slack webhook setup is complete now. Let's proceed with alert manager setup. You can test your webhook is working using below command:

`curl -X POST -H 'Content-type: application/json' --data '{"text":"Hello, World!"}' <YOUR WEBHOOK URL>`

![image](https://user-images.githubusercontent.com/37858762/236438335-8d07140f-a749-431f-8dee-ea80339fb940.png)

![image](https://user-images.githubusercontent.com/37858762/236438315-dab7c61a-a331-45f8-bc8e-f18ead9d0c8c.png)

## _Setup Alert Manager_

### Step 1: Now, lets setup alert manager using below commands:

```
ALERTMANAGER_VERSION="0.24.0"

wget https://github.com/prometheus/alertmanager/releases/download/v${ALERTMANAGER_VERSION}/alertmanager-${ALERTMANAGER_VERSION}.linux-amd64.tar.gz

tar xvzf alertmanager-${ALERTMANAGER_VERSION}.linux-amd64.tar.gz

cd alertmanager-${ALERTMANAGER_VERSION}.linux-amd64/

# if you just want to start prometheus as root
#./alertmanager --config.file=simple.yml

# create user
sudo useradd --no-create-home --shell /bin/false alertmanager 

# create directories
sudo mkdir /etc/alertmanager
sudo mkdir /etc/alertmanager/template
sudo mkdir -p /var/lib/alertmanager/data

# touch config file
sudo touch /etc/alertmanager/alertmanager.yml

# set ownership
sudo chown -R alertmanager:alertmanager /etc/alertmanager
sudo chown -R alertmanager:alertmanager /var/lib/alertmanager

# copy binaries
sudo cp alertmanager /usr/local/bin/
sudo cp amtool /usr/local/bin/

# set ownership
sudo chown alertmanager:alertmanager /usr/local/bin/alertmanager
sudo chown alertmanager:alertmanager /usr/local/bin/amtool

# Setup systemd by adding below lines to file /etc/systemd/system/alertmanager.service
sudo vim /etc/systemd/system/alertmanager.service

## Add below lines to this file

[Unit]
Description=Prometheus Alertmanager Service
Wants=network-online.target
After=network.target

[Service]
User=alertmanager
Group=alertmanager
Type=simple
ExecStart=/usr/local/bin/alertmanager \
    --config.file /etc/alertmanager/alertmanager.yml \
    --storage.path /var/lib/alertmanager/data
Restart=always

[Install]
WantedBy=multi-user.target

##Save and exit the file with :wq

sudo systemctl daemon-reload
sudo systemctl enable alertmanager
sudo systemctl start alertmanager

# restart prometheus
sudo systemctl restart prometheus

# Add the following lines and substitute with correct values to /etc/alertmanager/alertmanager.yml:
global:
  smtp_smarthost: 'localhost:25'
  smtp_from: 'alertmanager@prometheus.com'  ## <CHANGE TO UPDATE SENDER EMAIL>
  smtp_auth_username: ''
  smtp_auth_password: ''
  smtp_require_tls: false
templates:
- '/etc/alertmanager/template/*.tmpl'
route:
  repeat_interval: 1h
  receiver: operations-team
receivers:
- name: 'operations-team'
  email_configs:
  - to: 'operations-team+alerts@example.org'  ## <CHANGE  THIS TO YOUR OR RECEIVER EMAIL>
  slack_configs:
  - api_url: https://hooks.slack.com/services/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 ## <CHANGE THIS LINE WITH YOUR SLACK HOOK URL>
    channel: '#devops'  ## <CHANGE THIS TO YOUR SLACK CHANNEL NAME>
    send_resolved: true

# Alter the following config in /etc/prometheus/prometheus.yml:
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093

# Also, update alert rules file details under rule_files:
rule_files:
  - "/etc/prometheus/alert.rules"

# Create a new file /etc/prometheus/alert.rules with below alert rule(s). More rules of your requirements can be added here:

groups:
- name: example
  rules:
  - alert: cpuUsge
    expr: 100 - (avg by (instance) 	(irate(node_cpu_seconds_total{job="node_exporter",mode="idle",cpu="1"}[5m])) * 100) > 50
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: Machine under heavy load

# Restart both Alert manager and Prometheus services.
sudo service alertmanager restart
sudo service prometheus restart
```

### Step 2: Your files should looks like below:

**/etc/prometheus/prometheus.yml**

![image](https://user-images.githubusercontent.com/37858762/236438130-c067db8e-a5d5-4bde-a773-82bce17f96b1.png)

**/etc/alertmanager/alertmanager.yml**

![image](https://user-images.githubusercontent.com/37858762/236438105-ca0bce6b-13e5-466d-bbaa-9ea67e65ddf1.png)

**/etc/prometheus/alert.rules**

![image](https://user-images.githubusercontent.com/37858762/236438079-7abbb126-f981-4f00-b152-e44291fc9e95.png)

### Step 3: Check alertmanager service is running:

![image](https://user-images.githubusercontent.com/37858762/236438062-cb40a174-7119-4cbf-822b-c1b11df8b8a9.png)

### Step 4: Open alert manager on browser using url **http://\<Public\_IP\>:9093**. Make sure port 9093 is opened in firewall:

![image](https://user-images.githubusercontent.com/37858762/236438046-b90aa7f5-732c-4102-a634-8979265dca38.png)

### Step 5: Go to Prometheus console and check alert listed or not:

![image](https://user-images.githubusercontent.com/37858762/236438025-fc01fef0-892b-4918-9757-9615a5f32e3d.png)

### Step 6: Run below command to put 100% cpu load on the server and get alerted and run top command to see cpu usage on the server:

`yes 1 >> /dev/null &`

`top`

![image](https://user-images.githubusercontent.com/37858762/236437993-7a5da92a-de4a-450c-bf31-eb43386c08b4.png)

![image](https://user-images.githubusercontent.com/37858762/236437974-03d74369-3faa-446f-8725-9b6ccb8a4b92.png)

### Step 7: It will trigger the alert that can be seen in Prometheus alerts page as below:

![image](https://user-images.githubusercontent.com/37858762/236437911-45114399-4e12-4226-96e4-47f6e1303705.png)

### Step 8: After defined duration (1m here) it will fire the alert as show below:

![image](https://user-images.githubusercontent.com/37858762/236437894-fbf54c99-176b-4ffe-a10f-a87843322867.png)

### Step 9: Alert manager shows below entry:

![image](https://user-images.githubusercontent.com/37858762/236437871-0c7f94a7-1c3e-4b0d-9f1b-1a8bd617e241.png)

### Step 10: You will see alert sent to slack as below:

![image](https://user-images.githubusercontent.com/37858762/236437857-655063cf-5d3d-401b-878b-f5ddc4c939c2.png)

## _Monitor a Python Application with Prometheus Client Library_

### Step 1: As pre-requisites install docker and docker-compose on the server using below commands:

_Use below command to install docker and docker-compose_

```
curl -fsSL get.docker.com -o get-docker.sh

sudo sh get-docker.sh

sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

### Step 2: Verify if docker and docker-compose installed successfully:
  
![image](https://user-images.githubusercontent.com/37858762/236437585-de086ae8-0e67-42f0-9140-3ecc8989ddd6.png)

### Step 3: Run below commands to download the python app code from github and start the application using docker-compose.yaml file provided:

```
# Clone the repo from GitHub repo and cd to the directory

git clone https://github.com/vistasunil/prometheus_python_app.git

cd prometheus_python_app

# Bring the application up using docker-compose

sudo docker-compose up -d

# You can build your docker image and use in docker-compose file using the provided Dockerfile using below command
# sudo docker build . -t <image_name>

# Check if both containers are running

sudo docker ps

# Check if application application on 5000 port

curl localhost:5000
```

### Step 4: Output of application startup using docker-compose.

![image](https://user-images.githubusercontent.com/37858762/236437543-9692823c-8911-4ae7-a654-e959fb922f6e.png)

### Step 5: Output of docker command to check both app and db containers are up:

![image](https://user-images.githubusercontent.com/37858762/236437209-9f0d7437-3c35-4694-afe1-8590ba37ccc5.png)

### Step 6: Application up and running:

![image](https://user-images.githubusercontent.com/37858762/236437192-6ae4b739-9525-4971-b307-8cb819caf3b3.png)

### Step 7: Run more commands below to test different app endpoints:

```
curl localhost:8000/

curl localhost:5000/query

curl localhost:5000/sleep

curl localhost:8000/
```

![image](https://user-images.githubusercontent.com/37858762/236437154-ddff3073-741a-446c-85e5-a7f3e1fc6d2d.png)

![image](https://user-images.githubusercontent.com/37858762/236437132-dc703fcc-d9d9-4eb3-936a-f879134aad19.png)

### Step 8: Add python app to Prometheus config file **/etc/prometheus/prometheus.yml** as below:

```
  - job_name: 'flask_app'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:8000']
```

### Step 9: Reload Prometheus to take effect. Python flask app metrics should be visible in Prometheus now.

`kill -HUP <prometheus_PID>`

### Step 10: Run below command to continuously run curl commands to generate metrics.

`while true; do time curl localhost:5000/query && time curl localhost:5000/sleep; done`

![image](https://user-images.githubusercontent.com/37858762/236437024-2922ebd2-05d7-4997-8ba8-63941d917dcd.png)

### Step 11: Observer the metrics in Prometheus by running below queries in console:

```
flask_request_count
flask_request_latency_seconds_sum
flask_request_latency_seconds_count
```

![image](https://user-images.githubusercontent.com/37858762/236436957-6adf8b29-5874-409f-a2a1-8814c1cfc625.png)

![image](https://user-images.githubusercontent.com/37858762/236436935-161b6712-5b6e-4c3a-914d-b5ff5d360bde.png)

### Step 12: Terminate the command to stop generating metrics.

## _Installing and Configuring Grafana_

### Step 1: Install Grafana using below commands:

```
sudo sh -c 'echo deb https://packages.grafana.com/oss/deb stable main > /etc/apt/sources.list.d/grafana.list'

sudo curl https://packages.grafana.com/gpg.key | sudo apt-key add -

sudo apt-get update

sudo apt-get -y install grafana

sudo systemctl daemon-reload

sudo systemctl start grafana-server

sudo systemctl enable grafana-server.service

sudo systemctl status grafana-server
```

![image](https://user-images.githubusercontent.com/37858762/236436875-8831458f-0ea7-4bba-a5c8-dceb980c7c65.png)

### Step 2: Open Grafana in browser using url **http://\<PUBLIC\_IP\>:3000**. Make sure port 3000 is open in firewall:

![image](https://user-images.githubusercontent.com/37858762/236436844-13ca0de5-90e8-400e-b42f-1615371af882.png)

### Step 3: Login using both user and password **admin** and change the password or skip it.

![image](https://user-images.githubusercontent.com/37858762/236436817-90764547-639f-45c3-8ab6-e427fc6facda.png)

### Step 4: Add datasource using GUI or yaml file. We will use yaml file here.
### Step 5: Create file **/etc/grafana/provisioning/datasources/datasource-prometheus.yaml** with below data to add Prometheus datasource:

```
apiVersion: 1
datasources:
 - name: Prometheus
   type: prometheus
   orgId: 1
   url: http://localhost:9090
   access: proxy
   version: 1
   editable: false
   isDefault: true
```

### Step 6: Create file **/etc/grafana/provisioning/dashboards/dashboards.** yaml with below data to add dashboards to Grafana. Here /var/lib/grafana/dashboards path will be used to place multiple dashboards yaml files and Grafana can add all of them to show dashboards:

```
apiVersion: 1
providers:
 - name: 'default'
   orgId: 1
   folder: ''
   type: file
   options:
     path: /var/lib/grafana/dashboards
```

### Step 7: Create directory **/var/lib/grafana/dashboards/** using below command:

`sudo mkdir -p /var/lib/grafana/dashboards/`

### Step 8: Now, let's add sample dashboard by create **file /var/lib/grafana/dashboards/node-dashboard.json** with below JSON content. This adds two dashboards in Grafana, one for cpu and one for memory metrics:

```
{
  "annotations": {
    "list": [
      {
        "builtIn": 1,
        "datasource": "-- Grafana --",
        "enable": true,
        "hide": true,
        "iconColor": "rgba(0, 211, 255, 1)",
        "name": "Annotations & Alerts",
        "type": "dashboard"
      }
    ]
  },
  "editable": true,
  "gnetId": null,
  "graphTooltip": 0,
  "id": 5,
  "links": [],
  "panels": [
    {
      "aliasColors": {},
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": null,
      "fill": 1,
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 0,
        "y": 0
      },
      "id": 4,
      "legend": {
        "avg": false,
        "current": false,
        "max": false,
        "min": false,
        "show": true,
        "total": false,
        "values": false
      },
      "lines": true,
      "linewidth": 1,
      "links": [],
      "nullPointMode": "null",
      "percentage": false,
      "pointradius": 5,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "expr": "sum( node_memory_MemAvailable_bytes ) by (instance) / 1024 / 1024 / 1024\n",
          "format": "time_series",
          "hide": false,
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "thresholds": [],
      "timeFrom": null,
      "timeShift": null,
      "title": "Free Memory",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "buckets": null,
        "mode": "time",
        "name": null,
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "decimals": null,
          "format": "short",
          "label": "Mem / GB",
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        },
        {
          "format": "short",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        }
      ],
      "yaxis": {
        "align": false,
        "alignLevel": null
      }
    },
    {
      "aliasColors": {},
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": null,
      "fill": 1,
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 12,
        "y": 0
      },
      "id": 2,
      "legend": {
        "avg": false,
        "current": false,
        "max": false,
        "min": false,
        "show": true,
        "total": false,
        "values": false
      },
      "lines": true,
      "linewidth": 1,
      "links": [],
      "nullPointMode": "null",
      "percentage": false,
      "pointradius": 5,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "expr": "100 - (avg by (instance) (irate(node_cpu_seconds_total{job=\"node_exporter\",mode=\"idle\"}[5m])) * 100)\n",
          "format": "time_series",
          "hide": false,
          "intervalFactor": 1,
          "refId": "A"
        }
      ],
      "thresholds": [],
      "timeFrom": null,
      "timeShift": null,
      "title": "CPU Usage",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "buckets": null,
        "mode": "time",
        "name": null,
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "decimals": null,
          "format": "short",
          "label": "CPU %",
          "logBase": 1,
          "max": "100",
          "min": "0",
          "show": true
        },
        {
          "format": "short",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        }
      ],
      "yaxis": {
        "align": false,
        "alignLevel": null
      }
    }
  ],
  "schemaVersion": 16,
  "style": "dark",
  "tags": [],
  "templating": {
    "list": []
  },
  "time": {
    "from": "now-6h",
    "to": "now"
  },
  "timepicker": {
    "refresh_intervals": [
      "5s",
      "10s",
      "30s",
      "1m",
      "5m",
      "15m",
      "30m",
      "1h",
      "2h",
      "1d"
    ],
    "time_options": [
      "5m",
      "15m",
      "1h",
      "6h",
      "12h",
      "24h",
      "2d",
      "7d",
      "30d"
    ]
  },
  "timezone": "",
  "title": "Node Statistics",
  "uid": "KchAVsdmk",
  "version": 3
}
```

### Step 9: Restart Grafana service using below command and verify its running after restart:

`sudo service grafana-server restart`

`sudo service grafana-server status`

![image](https://user-images.githubusercontent.com/37858762/236436735-fe43628e-7b5b-4f2f-83ec-3198dedf57f3.png)

### Step 10: Navigate to dashboard and verify everything. Can see here that both the dashboards are visible here:

![image](https://user-images.githubusercontent.com/37858762/236436701-3dc32f51-38b8-413c-850a-efd5104688e3.png)
  
**We have successfully setup monitoring and alerting using Prometheus and Grafana.**
