# OLVM Certificate Renewal Guide

This document outlines the steps to check, back up, and renew certificates in an OLVM (oVirt) environment.

---

## 🔧 Prerequisites
- SSH access to OLVM engine server (e.g., via PuTTY)  
- Root or sudo privileges  

---

## 🚀 Procedure

### 1. Login to Server
Connect to the OLVM engine using SSH.

---

### 2. Check Certificate Status

#### OLVM Engine
```bash
openssl x509 -in /etc/pki/ovirt-engine/certs/engine.cer -noout -dates
```

#### KVM Host
```bash
openssl x509 -in /etc/pki/vdsm/certs/vdsmcert.pem -noout -dates
```

---

### 3. Backup OLVM Engine
```bash
engine-backup --scope=all --mode=backup \
--file=/root/backup_renew_certificate_date.bkp \
--log=/root/backup_renew_certificate.log
```

---

### 4. Backup Existing Certificates
```bash
tar cJpf /root/pki.tar.xz /etc/pki
```

---

### 5. Renew Certificates
```bash
engine-setup --offline
```

- The setup detects certificate expiry automatically  
- Type `Yes` when prompted  

---

### 6. Restart OLVM Service
```bash
systemctl restart ovirt-engine
systemctl status ovirt-engine
```

---

### 7. Verification
- Access the OLVM web portal  
- Confirm all services are running properly  

---

## ⚠️ Important Notes
- Always take backups before making changes  
- Perform during a maintenance window  
- Do not expose sensitive information (IP, credentials)  

-----