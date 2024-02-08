Monitoring using Prometheus and Grafana on AWS EC2

Agenda
1.	Prometheus Architecture
2.	Install Prometheus and configure Prometheus to monitor itself
3.	Install Node Exporter on other EC2 Instances
4.	Configure Prometheus for the EC2 Instance
5.	EC2 Service Discovery for Prometheus
6.	Install Grafana
Prerequisite:
1.	Prometheus EC2 instance t2.micro 
2.	Node EC2 instances to monitor
3.	Security Groups Configured properly
Security Groups Configured on EC2 Instances
•	Port 9090 — Prometheus Server
•	Port 9100 — Prometheus Node Exporter
•	Port 3000 — Grafana
Prometheus EC2 Instance
Configure the security group on EC2 Instance where Prometheus Server is installed: 9000, 3000 and 9100.
Node EC2 Instances
Configure the security group on EC2 Instance which you want to monitor and where you will install Node Exporter as shown below:
One entry is from your IP and one entry is the IP of the EC2 instance where Prometheus server is installed so that Prometheus server can read the metrics which is exposed on the Node server.
Port : 9100

Install Prometheus
Now we will install the Prometheus on one of the EC2 Instance.
You can download the latest version from here
1.	Clone my git repo
2.	Run the install-prometheus.sh script
3.	This script will install everything and configured it. You can change the version as per your project.
This script will do the below steps:
1.	Create a new user and add new directories
sudo useradd --no-create-home prometheus
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
2. Download the Prometheus, extract it and put it in /usr/local/bin folder and finally delete the software
wget https://github.com/prometheus/prometheus/releases/download/v2.23.0/prometheus-2.23.0.linux-amd64.tar.gz
tar -xvf prometheus-2.23.0.linux-amd64.tar.gz
sudo cp prometheus-2.23.0.linux-amd64/prometheus /usr/local/bin
sudo cp prometheus-2.23.0.linux-amd64/promtool /usr/local/bin
sudo cp -r prometheus-2.23.0.linux-amd64/consoles /etc/prometheus/
sudo cp -r prometheus-2.23.0.linux-amd64/console_libraries /etc/prometheus
sudo cp prometheus-2.23.0.linux-amd64/promtool /usr/local/bin/
rm -rf prometheus-2.23.0.linux-amd64.tar.gz prometheus-2.19.0.linux-amd64

3. Now we will configure Prometheus to monitor itself using yaml file. Create a prometheus.yml file at /etc/prometheus/prometheus.yml with the below content

global:
  scrape_interval: 15s
  external_labels:
    monitor: 'prometheus'
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

