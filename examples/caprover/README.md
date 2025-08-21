# LibreNMS with CapRover

This example demonstrates how to deploy LibreNMS using CapRover, a self-hosted Platform-as-a-Service (PaaS) solution.

## Overview

The CapRover version uses the `captain-overlay-network` instead of direct port mapping, making it compatible with CapRover's networking architecture. The main LibreNMS service doesn't expose port 8000 directly, but ports 514 (syslog) and 162 (SNMP traps) are still mapped as they require direct external access for their protocols.

## Files

- `compose.yml` - Docker Compose configuration for CapRover deployment
- `librenms.env` - LibreNMS environment variables
- `msmtpd.env` - SMTP relay configuration

## Deployment Steps

1. **Clone or copy the CapRover example files** to your CapRover server:

   ```bash
   # Copy the caprover example to your desired location
   cp -r examples/caprover /path/to/your/caprover/apps/
   cd /path/to/your/caprover/apps/caprover
   ```

2. **Edit the environment files** with your preferences:

   - `librenms.env` - Configure LibreNMS settings
   - `msmtpd.env` - Configure SMTP relay settings

3. **Deploy using Docker Compose**:

   ```bash
   docker compose up -d
   docker compose logs -f
   ```

4. **Create a CapRover "Nginx Reverse Proxy" app**:
   - In your CapRover dashboard, create a new app
   - Set the app type to "Nginx Reverse Proxy"
   - Configure the upstream proxy to `http://librenms` (the container name)
   - Set your desired domain/subdomain

## Key Differences from Standard Compose

- Uses `captain-overlay-network` instead of default bridge network
- Main LibreNMS service doesn't expose port 8000 (handled by CapRover proxy)
- Retains direct port mappings for syslog (514) and SNMP traps (162) as they require direct external access
- All services are configured to work within CapRover's networking environment

## Access

After deployment and reverse proxy configuration, access LibreNMS through your configured CapRover domain/subdomain.

## Services

- **librenms** - Main LibreNMS application
- **db** - MariaDB database
- **redis** - Redis cache
- **msmtpd** - SMTP relay for email notifications
- **dispatcher** - LibreNMS dispatcher service
- **syslogng** - Syslog-ng service for log processing
- **snmptrapd** - SNMP trap daemon

## Troubleshooting

- Ensure the `captain-overlay-network` exists in your CapRover environment
- Check container logs: `docker compose logs -f [service-name]`
- Verify network connectivity between services
- Ensure proper DNS resolution for your CapRover domain
