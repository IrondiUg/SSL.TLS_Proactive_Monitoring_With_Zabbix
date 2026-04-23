## Overview 
This project delivers automated SSL/TLS monitoring using `Zabbix` to eliminate one of the **most common and preventable** causes of outages and security failures: `Expired or misconfigured certificates.`
If your organization relies on `web services`, `APIs`, or `secure communication Protocols`, then SSL/TLS is not optional, and neither is monitoring it.

## Why Your Organization Needs This
An expired SSL certificate doesn’t just throw a warning, it can:
- Instantly take down customer-facing services
- Trigger security alerts and browser blocks
- Damage user trust and brand credibility
- Lead to compliance and audit failures

And the reality? These failures are often due to lack of `visibility`, not lack of tools.


## What This Project Solves
Most organizations `don’t lose services because they lack security tools`, they lose them because `no one saw the problem coming.`

**This solution eliminates that blind spot.**

Without `active SSL/TLS monitoring`, you are one missed-certificate-renewal away from `service outage`, one weak protocol away from a `vulnerability`, and one misconfiguration away from `failing compliance checks.`

This project ensures you are never operating in the dark by continuously exposing:
- `Certificates that are silently approaching expiration`
- `Weak or outdated SSL/TLS configurations attackers look for first`
- `Misconfigured or non-compliant endpoints hiding in your infrastructure`

With automated checks and real-time alerts, your team shifts from:
`“Why is the site down?”` to `“We fixed it before users even noticed.”`

## Business Impact
This is not just monitoring, it’s `risk elimination at the root.`
Implementing this project means:
- No last-minute panic over expired certificates
- No avoidable downtime affecting users or revenue
- No guessing about your SSL/TLS security posture
- No manual tracking that fails when it matters most

Instead, you get:
- `Predictability` – You know issues before they become incidents
- `Control` – Full visibility across all certificates and endpoints
- `Confidence` – Your services remain secure, trusted, and compliant

# Zabbix
<img width="1350" height="930" alt="ssltls-monitoring-flow-zabbix-based-proactive-ei8fdi" src="https://github.com/user-attachments/assets/6ff8f23b-73ef-4319-ac07-7496621a6433" />

This guide walks you through installing and running Zabbix using Docker and Docker Compose. It deploys a complete monitoring stack including:
- Zabbix Server
- Zabbix Web Interface
- MySQL Database

**Install Docker (If Not Installed)**
```
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable"
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```
**Start and enable Docker**
```
sudo systemctl start docker
sudo systemctl enable docker
```

**Install Docker Compose (Plugin)**
```
sudo apt install docker-compose-plugin
```
---
**Create docker-compose.yml and paste exact content**
```
version: '3.5'

services:
  mysql-server:
    image: mysql:8.0
    container_name: zabbix-mysql
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: zabbix
      MYSQL_USER: zabbix
      MYSQL_PASSWORD: (put a convenient password here)
      MYSQL_ROOT_PASSWORD: (put a convenient password here)
    command:
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_bin
    volumes:
      - mysql_data:/var/lib/mysql

  zabbix-server:
    image: zabbix/zabbix-server-mysql:latest
    container_name: zabbix-server
    restart: unless-stopped
    depends_on:
      - mysql-server
    environment:
      DB_SERVER_HOST: mysql-server
      MYSQL_DATABASE: zabbix
      MYSQL_USER: zabbix
      MYSQL_PASSWORD: (put a convenient password here/must be same as above)
    ports:
      - "10051:10051"

  zabbix-web:
    image: zabbix/zabbix-web-nginx-mysql:latest
    container_name: zabbix-web
    restart: unless-stopped
    depends_on:
      - mysql-server
      - zabbix-server
    environment:
      DB_SERVER_HOST: mysql-server
      MYSQL_DATABASE: zabbix
      MYSQL_USER: zabbix
      MYSQL_PASSWORD: (put a convenient password here/must be same as above)
      ZBX_SERVER_HOST: zabbix-server
      PHP_TZ: Africa/Lagos
    ports:
      - "8080:8080"
```
**Start Zabbix Stack**
```
docker compose up -d
```

**Access Zabbix Web Interface**
```
http://localhost:8080
```
**Default Credentials**
<img width="369" height="388" alt="Clipped_image_20260423_174518" src="https://github.com/user-attachments/assets/d4645675-2be2-4a7b-871a-8df99cb3ecb5" />

- Username: `Admin`
- Password: `zabbix`



