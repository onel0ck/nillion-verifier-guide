# Nillion Verifier Installation Guide
A guide for installing Nillion Verifier on a Linux server alongside other nodes.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installation Steps](#installation-steps)
- [Configuration](#configuration)
- [Running the Verifier](#running-the-verifier)
- [Monitoring & Maintenance](#monitoring--maintenance)
- [Security](#security)
- [Troubleshooting](#troubleshooting)

## Prerequisites
- NIL tokens in Keplr wallet (get test tokens from [Nillion Faucet]([link to faucet](https://faucet.testnet.nillion.com/)))
- Docker installed
- Linux server with root access

Verify Docker installation:
```bash
docker --version
docker ps
```

## Installation Steps

### 1. Prepare Working Directory
```bash
cd /root
mkdir -p nillion-node
cd nillion-node
mkdir -p verifier
```

### 2. Install Nillion Verifier

Pull the Docker image:
```bash
docker pull nillion/verifier:v1.0.1
```

Initialize the verifier:
```bash
docker run -v $(pwd)/verifier:/var/tmp nillion/verifier:v1.0.1 initialise
```

⚠️ **Important**: After initialization, you'll receive:
- Verifier account id
- Verifier public key

Save these credentials and register your verifier at https://verifier.nillion.com/verifier

![image](https://github.com/user-attachments/assets/2976ec15-ecfb-4023-b191-cbf85de2a8e3)

### 3. Configuration

Create startup script:
```bash
nano start-verifier.sh
```

Add the following content:
```bash
#!/bin/bash
docker run -v $(pwd)/verifier:/var/tmp \
  nillion/verifier:v1.0.1 verify \
  --rpc-endpoint "https://nillion-testnet.rpc.decentrio.ventures/"
```

Make script executable:
```bash
chmod +x start-verifier.sh
```

## Running the Verifier

### Standard Launch
```bash
./start-verifier.sh
```

### Background Launch
```bash
nohup ./start-verifier.sh > verifier.log 2>&1 &
```

### Monitor Logs
```bash
tail -f verifier.log
```

## Monitoring & Maintenance

### Essential Commands
```bash
# Check all containers
docker ps

# View verifier logs
tail -f verifier.log

# Monitor resource usage
docker stats
```

### Backup Credentials
Always backup your credentials file:
```bash
cp verifier/credentials.json verifier/credentials.json.backup
```

## Security
- Keep secure backups of `credentials.json`
- Never share verifier credentials
- Monitor logs regularly for suspicious activity

## Troubleshooting

If you encounter issues, check:

```bash
# View logs
tail -f verifier.log

# Check Docker status
systemctl status docker

# Check disk space
df -h

# Check memory usage
free -m
```

## Notes
The Nillion verifier can run alongside other nodes (like Allora) as they:
- Use different ports
- Store data in separate directories
- Have independent configurations

## Contributing
Feel free to submit issues and enhancement requests!
