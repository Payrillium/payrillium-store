# Payrillium Store

Official Payrillium applications store for CasaOS - A comprehensive collection of applications designed for the PayOS ecosystem.

## 📋 Table of Contents

- [Overview](#overview)
- [Installation](#installation)
- [Available Applications](#available-applications)
- [Management Commands](#management-commands)
- [Development Guide](#development-guide)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## 🌟 Overview

Payrillium Store is a third-party application store for CasaOS that provides a curated collection of applications specifically designed for the PayOS ecosystem. This store includes utilities, development tools, and custom applications that enhance the CasaOS experience.

### Features

- 🚀 **Easy Installation**: One-click app installation through CasaOS UI
- 🔧 **Development Tools**: Essential utilities for PayOS development
- 🌐 **Multi-language Support**: Applications with English and Spanish support
- 📱 **Responsive Design**: Optimized for various device architectures (amd64, arm64)
- 🔄 **Auto-updates**: Automatic synchronization with GitHub repository

## 🚀 Installation

### Prerequisites

- CasaOS installed and running
- `casaos-cli` command-line tool
- Internet connection for downloading applications

### Add Payrillium Store to CasaOS

1. **Using CasaOS CLI (Recommended)**:

   ```bash
   casaos-cli app-management register app-store "https://github.com/Payrillium/payrillium-store/archive/refs/heads/main.zip"
   ```

2. **Using CasaOS Web Interface**:
   - Open CasaOS web interface
   - Navigate to App Store settings
   - Click "Add Source"
   - Enter: `https://github.com/Payrillium/payrillium-store/archive/refs/heads/main.zip`
   - Click "Add"

### Verify Installation

```bash
# List all registered app stores
casaos-cli app-management list app-store

# You should see Payrillium Store listed
```

## 📱 Available Applications

### HelloBox

- **Category**: Payrillium
- **Description**: Simple Nginx test application for Payrillium ecosystem
- **Port**: 8080
- **Purpose**: Testing and development environment

### Payrillium Panel

- **Category**: Utilities
- **Description**: Frontend placeholder for PayOS testing and development
- **Port**: 8339
- **Purpose**: PayOS frontend testing environment
- **Features**: Multi-language support (English/Spanish)

## 🛠️ Management Commands

### View Logs

```bash
# Monitor CasaOS app management logs
sudo tail -f /var/log/casaos/app-management.log

# Filter for Payrillium Store specific logs
sudo tail -f /var/log/casaos/app-management.log | grep -i payrillium

# View recent errors
sudo tail -n 100 /var/log/casaos/app-management.log | grep -i error
```

### Restart CasaOS

```bash
# Restart CasaOS service
sudo systemctl restart casaos

# Check CasaOS status
sudo systemctl status casaos

# View CasaOS logs
sudo journalctl -u casaos -f
```

### Manage App Store

```bash
# List all app stores
casaos-cli app-management list app-store

# Remove Payrillium Store
casaos-cli app-management unregister app-store <store-id>

# Re-add Payrillium Store
casaos-cli app-management register app-store "https://github.com/Payrillium/payrillium-store/archive/refs/heads/main.zip"

# Force update store (clear cache and re-register)
sudo rm -rf /var/lib/casaos/appstore/github.com/*payrillium*
casaos-cli app-management register app-store "https://github.com/Payrillium/payrillium-store/archive/refs/heads/main.zip"
```

### Application Management

```bash
# List installed applications
casaos-cli app-management list apps

# Install an application
casaos-cli app-management install app <app-name>

# Uninstall an application
casaos-cli app-management uninstall app <app-name>

# Check application status
casaos-cli app-management status app <app-name>
```

## 🔧 Development Guide

### Creating a New Application

1. **Create Application Directory**:

   ```bash
   mkdir Apps/YourAppName
   cd Apps/YourAppName
   ```

2. **Create docker-compose.yml**:

   ```yaml
   name: your-app-name
   services:
     your-app-name:
       image: your-image:tag
       container_name: your-app-name
       ports:
         - target: 80
           published: "8080"
           protocol: tcp
       restart: unless-stopped
       volumes:
         - type: bind
           source: /DATA/AppData/$AppID
           target: /app/data
       x-casaos:
         ports:
           - container: "80"
             description:
               en_us: "Web interface port"
         volumes:
           - container: /app/data
             description:
               en_us: "Application data directory"

   x-casaos:
     architectures:
       - amd64
       - arm64
     main: your-app-name
     author: Payrillium
     category: Utilities
     description:
       en_us: "Your application description"
       es_es: "Descripción de tu aplicación"
     icon: https://raw.githubusercontent.com/Payrillium/payrillium-store/main/Apps/YourAppName/icon.png
     thumbnail: https://raw.githubusercontent.com/Payrillium/payrillium-store/main/Apps/YourAppName/icon.png
     tagline:
       en_us: "Your application tagline"
       es_es: "Eslogan de tu aplicación"
     title:
       en_us: "Your App Name"
       es_es: "Nombre de tu App"
     index: /
     scheme: http
     port_map: "8080"
   ```

3. **Add Application Icon**:

   - Create `icon.png` (recommended: 512x512px)
   - Place in `Apps/YourAppName/icon.png`

4. **Update category-list.json** (if needed):

   ```json
   [
     {
       "name": "All",
       "font": "apps",
       "description": "All Apps"
     },
     {
       "name": "Utilities",
       "font": "toolbox-outline",
       "description": "Utilities Apps"
     },
     {
       "name": "Payrillium",
       "font": "star",
       "description": "Payrillium Apps"
     },
     {
       "name": "YourCategory",
       "font": "your-icon",
       "description": "Your Category Description"
     }
   ]
   ```

5. **Create recommend-list.json** (optional):
   ```json
   [
     {
       "appid": "your-app-name"
     }
   ]
   ```

### Testing Your Application

1. **Commit and Push Changes**:

   ```bash
   git add .
   git commit -m "Add YourAppName application"
   git push origin main
   ```

2. **Force Update Store**:

   ```bash
   casaos-cli app-management unregister app-store <payrillium-store-id>
   casaos-cli app-management register app-store "https://github.com/Payrillium/payrillium-store/archive/refs/heads/main.zip"
   ```

3. **Verify Installation**:
   - Check CasaOS web interface
   - Look for your application in the App Store
   - Test installation and functionality

### Development Workflow

1. **Make Changes**: Modify your application files
2. **Test Locally**: Verify docker-compose.yml syntax
3. **Commit Changes**: Push to GitHub
4. **Update Store**: Re-register the store in CasaOS
5. **Verify**: Check that changes appear in CasaOS

## 🔍 Troubleshooting

### Common Issues

#### Store Not Loading

```bash
# Check if store is registered
casaos-cli app-management list app-store

# Check logs for errors
sudo tail -f /var/log/casaos/app-management.log | grep -i error

# Force re-registration
casaos-cli app-management unregister app-store <store-id>
casaos-cli app-management register app-store "https://github.com/Payrillium/payrillium-store/archive/refs/heads/main.zip"
```

#### Applications Not Appearing

```bash
# Check store root directory
sudo find /var/lib/casaos/appstore -name "*payrillium*" -type d

# Verify docker-compose.yml syntax
docker-compose -f Apps/YourApp/docker-compose.yml config

# Check for missing files
ls -la Apps/YourApp/
```

#### Port Conflicts

- Ensure `port_map` in docker-compose.yml matches the actual port
- Check that ports are not already in use
- Verify port mapping format: `"8080"` (with quotes)

#### Extension Errors

- Ensure `x-casaos` extension is present at both service and root levels
- Verify all required fields are present in `x-casaos` section
- Check YAML syntax and indentation

### Debug Commands

```bash
# Check CasaOS service status
sudo systemctl status casaos

# View detailed logs
sudo journalctl -u casaos -f

# Check app store directory
sudo ls -la /var/lib/casaos/appstore/

# Verify downloaded files
sudo find /var/lib/casaos/appstore -name "*.yml" -exec head -20 {} \;
```

## 🤝 Contributing

We welcome contributions to the Payrillium Store! Here's how you can help:

### How to Contribute

1. **Fork the Repository**: Create your own fork of this repository
2. **Create a Branch**: `git checkout -b feature/your-feature-name`
3. **Make Changes**: Add your application or improvements
4. **Test Thoroughly**: Ensure your changes work correctly
5. **Submit Pull Request**: Create a PR with detailed description

### Guidelines

- Follow the existing application structure
- Include proper documentation
- Test on multiple architectures (amd64, arm64)
- Provide clear commit messages
- Update this README if adding new features

### Application Requirements

- Must include valid `docker-compose.yml` with `x-casaos` extensions
- Provide appropriate icons (512x512px PNG)
- Include multi-language support when possible
- Follow CasaOS naming conventions
- Ensure applications are stable and secure

## 📞 Support

- **Issues**: Report bugs and request features via GitHub Issues
- **Discussions**: Join community discussions in GitHub Discussions
- **Documentation**: Check CasaOS documentation for advanced topics

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- CasaOS team for the excellent platform
- Community contributors for their valuable input
- Docker team for containerization technology

---

**Made with ❤️ by the Payrillium Team**
