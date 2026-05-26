# VAULT EHR SYSTEM ARCHITECTURAL DIAGRAM 
# <img width="1376" height="768" alt="architecture-diagram" src="https://github.com/user-attachments/assets/70bde7d5-30d3-4d27-a3e2-33ea626298e3" />


**Internet Zone (top)**
External users and patients send HTTPS traffic through the public internet cloud. From there, traffic splits into two paths — DDoS protection/filtering and load distribution — before entering AWS.


**AWS Cloud Zone (middle)**
This is the cloud-hosted application tier, split into three sections:

- **Security Stack (left)** — Three layers of protection sit in front of everything: AWS Shield (DDoS mitigation), AWS WAF (web application firewall), and an Application Load Balancer. Traffic passes through two Firewall/Security Groups and an ALB before reaching the servers.

- **Public Subnet (center)** — Two Web/App Servers run in parallel behind Port 443. These handle live application requests and are intentionally separated from the database tier. They perform scheduled backups to the private subnet.

- **Private Subnet (right)** — Isolated from direct internet access. Contains an AWS Backup Vault and Amazon S3 bucket that receive scheduled backups from both web servers. This is where backup data is safely stored.



**On-Premises Hospital Zone (bottom)**
Connected to AWS via a **Secure VPN Tunnel**, this is where sensitive PHI data actually lives:

- **Data Tier** — A Primary PostgreSQL database server with a Secondary Failover server in active replication. Data is encrypted at rest. The secondary server also receives query/update traffic, likely for read scaling.

- **Security & Audit** — An Audit Logging Server collects syslogs and logs from the database tier, while a SIEM Monitoring Server receives telemetry for real-time threat detection. Both systems manage the Hospital Endpoints.

- **Hospital Endpoints** — Four classes of on-premises devices: Admin Workstations, Clinician Laptops, Nurses' Tablets, and Medical IoT Devices. All are managed through the security and audit layer.


**Key design decisions visible here:**
- PHI stays **on-premises** — the databases never leave the hospital network, which supports HIPAA compliance
- The VPN tunnel ensures all cloud-to-on-prem communication is encrypted in transit
- Redundancy is built in at every layer — two web servers, primary/failover databases, and geo-backed S3 storage
- The SIEM + audit logging combination provides the continuous monitoring required for HIPAA/HITECH compliance





