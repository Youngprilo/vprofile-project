# Vprofile Project

A multi-tier Java web application deployed across 5 virtual machines using Vagrant and VMware Fusion. This project demonstrates real-world production architecture patterns used in enterprise environments.

## Architecture Overview

<img width="1440" height="1228" alt="image" src="https://github.com/user-attachments/assets/ed8cac2e-cec8-4fda-b5c6-702551c2ddee" />





## Stack

| VM | Hostname | IP | Role | Technology |
|---|---|---|---|---|
| Web Server | web01 | 192.168.56.11 | Load Balancer / Reverse Proxy | Nginx |
| App Server | app01 | 192.168.56.12 | Java Application Server | Tomcat 10 |
| Database | db01 | 192.168.56.15 | Relational Database | MySQL 8.0 |
| Cache | mc01 | 192.168.56.14 | Caching Layer | Memcache |
| Message Broker | rmq01 | 192.168.56.16 | Message Queue | RabbitMQ |

## Prerequisites

- [Vagrant](https://www.vagrantup.com/) 2.4+
- [VMware Fusion](https://www.vmware.com/products/desktop-hypervisors/fusion) (Mac) or VMware Workstation (Windows/Linux)
- [vagrant-vmware-desktop plugin](https://developer.hashicorp.com/vagrant/docs/providers/vmware)
- Minimum 8GB RAM available
- 20GB free disk space

## Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/Youngprilo/vprofile-project.git
cd vprofile-project
```

### 2. Boot all VMs
```bash
vagrant up
```

### 3. Access the application
Open your browser and navigate to the web01 VM IP address on port 80.

Default credentials:
- **Username:** `admin_vp`
- **Password:** `admin_vp`

## VM Setup Details

### web01 — Nginx
- Configured as a reverse proxy forwarding traffic to Tomcat on app01:8080
- SELinux configured to allow outbound network connections
- Firewall opened on port 80

### app01 — Tomcat
- Java 17 (OpenJDK)
- Tomcat 10.1.20
- Vprofile WAR deployed as ROOT application
- Connects to MySQL, Memcache and RabbitMQ via hostname resolution
- Firewall opened on port 8080

### db01 — MySQL
- MySQL 8.0
- Database: `accounts`
- User: `admin` with full privileges on accounts database
- Schema imported from `src/main/resources/db_backup.sql`
- Firewall opened on port 3306

### mc01 — Memcache
- Configured to listen on all interfaces (0.0.0.0)
- Port: 11211
- Firewall opened on port 11211

### rmq01 — RabbitMQ
- Installed via packagecloud.io repository
- User: `test` with administrator role
- Port: 5672
- Firewall opened on port 5672

## Network Configuration

All VMs are connected via a private network (192.168.56.0/24). Each VM has /etc/hosts configured for hostname resolution:

```
192.168.56.11 web01
192.168.56.12 app01
192.168.56.14 mc01
192.168.56.15 db01
192.168.56.16 rmq01
```

## Application Configuration

The application connects to backend services via `src/main/resources/application.properties`:

- **Database:** db01:3306
- **Memcache:** mc01:11211
- **RabbitMQ:** rmq01:5672

## Vagrant Commands

```bash
# Start all VMs
vagrant up

# Check status of all VMs
vagrant status

# SSH into a specific VM
vagrant ssh web01
vagrant ssh app01
vagrant ssh db01
vagrant ssh mc01
vagrant ssh rmq01

# Stop all VMs
vagrant halt

# Destroy all VMs
vagrant destroy -f
```

## Troubleshooting

**502 Bad Gateway:**
- Check SELinux on web01: `setsebool -P httpd_can_network_connect 1`
- Verify Tomcat is running on app01: `systemctl status tomcat`
- Check firewall on app01: `firewall-cmd --list-ports`

**Login not working:**
- Verify MySQL is running: `systemctl status mysqld`
- Check database schema is correct: `mysql -u root -padmin123 accounts -e "describe user;"`
- Check firewall on db01: `firewall-cmd --list-ports`

**RabbitMQ connection errors:**
- These appear in logs but do not affect core login functionality
- Verify RabbitMQ is running: `systemctl status rabbitmq-server`
- Check firewall on rmq01: `firewall-cmd --list-ports`

## Key Learnings

- Multi-tier application architecture and how services communicate
- Vagrant multi-VM provisioning with VMware provider
- Linux service management with systemctl
- Firewall configuration with firewalld
- SELinux policy management
- Java application deployment with Tomcat
- Reverse proxy configuration with Nginx
- MySQL database administration
- Message broker setup with RabbitMQ

## Author

Micah Okemeje — [GitHub](https://github.com/Youngprilo)

