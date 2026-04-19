# VidaaEdge Docker Setup

A Docker Compose setup for running the VidaaEdge development toolkit with integrated DNS configuration for VidaaOS-based TVs.

## Overview

VidaaEdge is a development toolkit for VidaaOS-based TVs, enabling remote function exploration, custom JavaScript execution, and PWA installation. This Docker setup packages the application with a DNS server to simplify network configuration.

## Features

- **Containerized Application**: Runs the VidaaEdge API server and Angular frontend in Docker
- **Integrated DNS Server**: Includes dnsmasq to resolve `vidaahub.com` domain
- **Persistent Data**: Mounts scan-data directory for session persistence
- **HTTPS Support**: Uses self-signed certificates for secure TV access

## Prerequisites

- Docker and Docker Compose installed
- Git (to clone this repository)
- Access to your network's DNS settings (to configure TV)

## Quick Start

1. **Clone the repository**:
   ```bash
   git clone https://github.com/your-username/vidaa-edge-docker.git
   cd vidaa-edge-docker
   ```

2. **Find your host IP address**:
   ```bash
   # On macOS/Linux
   ifconfig | grep inet
   # or
   ip addr show
   ```
   Note the IP address of your network interface (e.g., 192.168.1.100).

3. **Start the services**:
   ```bash
   HOST_IP=your_host_ip docker-compose up -d
   ```
   Replace `your_host_ip` with the IP address from step 2.

4. **Configure your TV's DNS**:
   - Go to your TV's network settings
   - Set DNS server to your host IP address
   - Save settings

5. **Access the application**:
   - Open `https://vidaahub.com/` in your TV's browser
   - Accept the self-signed certificate warning

## Services

### vidaa-edge
- **Ports**: 3000 (API), 443 (HTTPS frontend)
- **Volumes**: `./scan-data` for persistent scan sessions
- **Build**: Uses local Dockerfile based on Node.js 18

### dnsmasq
- **Ports**: 53 (DNS)
- **Function**: Resolves `vidaahub.com` to your host IP
- **Configuration**: Uses `HOST_IP` environment variable

## Configuration

### Environment Variables
- `HOST_IP`: Your host machine's IP address (required)

### Volumes
- `./scan-data`: Persistent storage for TV scan sessions and data

### Ports
- `3000`: VidaaEdge API server
- `443`: VidaaEdge HTTPS frontend
- `53`: DNS server (dnsmasq)

## Development

### Building the Image
```bash
docker-compose build
```

### Viewing Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f vidaa-edge
docker-compose logs -f dnsmasq
```

### Stopping Services
```bash
docker-compose down
```

## Troubleshooting

### DNS Issues
- Ensure `HOST_IP` is set correctly
- Verify your TV's DNS settings point to your host IP
- Check dnsmasq logs: `docker-compose logs dnsmasq`

### Certificate Warnings
- The app uses self-signed certificates
- Accept the security warning in your TV browser

### Port Conflicts
- Ensure ports 53, 3000, and 443 are not in use by other services
- Modify port mappings in `docker-compose.yml` if needed

### Network Access
- The TV must be on the same network as your host
- Firewall rules may need to allow traffic on ports 53, 3000, 443

## Security Notes

- This setup uses self-signed certificates for HTTPS
- Designed for local network development use only
- Do not expose to public internet without proper security measures

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Credits

- Original VidaaEdge project by [weinzii](https://github.com/weinzii/vidaa-edge)
- Exploit research by [BananaMafia](https://bananamafia.dev/post/hisensehax/)
- Docker setup inspired by community contributions
