# Payrillium Store

Official Payrillium applications store for CasaOS - A comprehensive collection of applications designed for the PayOS ecosystem.

## 📋 Table of Contents

- [Overview](#overview)
- [Important: Category Filtering](#important-category-filtering)
- [Installation](#installation)
- [Creating Compatible Applications](#creating-compatible-applications)
- [Available Applications](#available-applications)
- [Common Errors and Solutions](#common-errors-and-solutions)
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

## ⚠️ Important: Category Filtering

**For your apps to appear in the "Payrillium" filter:**

- ✅ Use `category: Payrillium` in the `category` field
- ✅ Use `author: Payrillium` in the `author` field
- ❌ **DO NOT use** `category: Utilities` or other generic categories

**Remember:** Although you use `author: Payrillium`, the actual filtering is done by the `category` field.

---

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

---

## 📝 Creating Compatible Applications

### Required Files Per Application

```
Apps/
└── AppName/
    ├── docker-compose.yml   # ✅ REQUIRED - Main file
    ├── icon.png             # ✅ REQUIRED - App icon (192x192px)
    ├── screenshot-1.png    # ✅ REQUIRED - At least 1 screenshot
    ├── screenshot-2.png    # ⚪ OPTIONAL - More screenshots
    └── thumbnail.png        # ⚪ OPTIONAL - For featured apps
```

### Critical Elements in `docker-compose.yml`

#### 1. **Top Level - `name` (REQUIRED)**

```yaml
name: app-name # Only lowercase letters, numbers, hyphen and underscore
```

#### 2. **`services` Section (REQUIRED)**

```yaml
services:
  service-name:
    image: image:version # ✅ USE SPECIFIC VERSION, NOT "latest"
    ports:
      - target: 8080
        published: "8080"
        protocol: tcp
    restart: unless-stopped
    volumes:
      - type: bind
        source: /DATA/AppData/$AppID/config
        target: /app/data
```

#### 3. **`x-casaos` Extension at SERVICE LEVEL (REQUIRED)**

```yaml
x-casaos:
  envs:
    - container: VARIABLE
      description:
        en_us: "Variable description"
  ports:
    - container: "8080"
      description:
        en_us: "Application web port"
  volumes:
    - container: /app/data
      description:
        en_us: "Data directory"
```

#### 4. **`x-casaos` Extension at COMPOSE LEVEL (REQUIRED)**

```yaml
x-casaos:
  architectures:
    - amd64
    - arm64
    - arm
  main: service-name # ✅ Must match the service name
  author: Payrillium
  category:
    Payrillium # ✅ REQUIRED: Use "Payrillium" to appear in Payrillium filter
    # ⚠️ IMPORTANT: Although we use "author: Payrillium", filtering is done by "category"
  description:
    en_us: "Complete application description"
  developer: Developer Name
  icon: https://raw.githubusercontent.com/Payrillium/payrillium-store/main/Apps/AppName/icon.png
  tagline:
    en_us: "Short phrase describing the app"
  title:
    en_us: "App Name"
  port_map: "8080"
  scheme: http
  index: /
```

### Complete Working Example

```yaml
name: hellobox
services:
  hellobox:
    image: nginx:alpine
    ports:
      - target: 80
        published: "8080"
        protocol: tcp
    restart: unless-stopped
    volumes:
      - type: bind
        source: /DATA/AppData/$AppID
        target: /usr/share/nginx/html
    x-casaos:
      ports:
        - container: "80"
          description:
            en_us: "Web interface port"
      volumes:
        - container: /usr/share/nginx/html
          description:
            en_us: "Web content directory"

x-casaos:
  architectures:
    - amd64
    - arm64
  main: hellobox
  author: Payrillium
  category: Payrillium # ✅ This creates the filter automatically
  title:
    en_us: HelloBox
  description:
    en_us: Test application for Payrillium ecosystem
  icon: https://raw.githubusercontent.com/Payrillium/payrillium-store/main/Apps/HelloBox/icon.png
  thumbnail: https://raw.githubusercontent.com/Payrillium/payrillium-store/main/Apps/HelloBox/icon.png
  tagline:
    en_us: Test application for Payrillium ecosystem
  index: /
  scheme: http
  port_map: "8080"
```

### Validation Rules

#### **App Name (`name`):**

- ✅ Only lowercase letters
- ✅ Numbers allowed
- ✅ Hyphen (`-`) and underscore (`_`) allowed
- ❌ No uppercase letters
- ❌ No spaces
- ❌ No special characters

**Valid examples:**

- `hello-box` ✅
- `app_123` ✅
- `test` ✅

**Invalid examples:**

- `Hello-Box` ❌ (uppercase)
- `App Name` ❌ (spaces)
- `app@123` ❌ (special characters)

#### **Image Version:**

- ✅ Use specific version: `nginx:1.25.3`
- ❌ DO NOT use `latest`: `nginx:latest`

#### **`category` Field:**

Must match a category in `category-list.json`:

- `Payrillium` ⭐ **Custom category for Payrillium apps**
  - **IMPORTANT**: Use `category: Payrillium` for your apps to appear in the Payrillium filter
  - Although you use `author: Payrillium`, filtering is done by the `category` field
- `Utilities`
- `Media`
- `Network`
- `Developer`
- etc.

---

## ⚠️ Common Errors and Solutions

### **Error 1: "failed to load compose file"**

**Cause:** Missing `x-casaos` extension  
**Solution:** Add `x-casaos` both at service level and root level

### **Error 2: "extension x-casaos not found"**

**Cause:** Incorrect `x-casaos` structure  
**Solution:** Verify that all required fields are present

### **Error 3: Apps appear in wrong category**

**Cause:** Incorrect or missing `category` field  
**Solution:** Use a category from `category-list.json`

### **Error 4: Cannot install from app-store**

**Cause:** CasaOS cannot download files  
**Solution:** Verify that:

- GitHub URL is accessible
- Files have correct permissions
- ZIP has correct structure

### **Error 5: Store not loading**

**Cause:** Network issues or invalid ZIP  
**Solution:**

```bash
# Check logs
sudo tail -f /var/log/casaos/app-management.log | grep -i error

# Force re-registration
casaos-cli app-management unregister app-store <store-id>
casaos-cli app-management register app-store "https://github.com/Payrillium/payrillium-store/archive/refs/heads/main.zip"
```

### **Error 6: Applications not appearing**

**Cause:** Missing files or incorrect structure  
**Solution:**

```bash
# Check store root directory
sudo find /var/lib/casaos/appstore -name "*payrillium*" -type d

# Verify docker-compose.yml syntax
docker-compose -f Apps/YourApp/docker-compose.yml config

# Check for missing files
ls -la Apps/YourApp/
```

### **Error 7: Port conflicts**

**Cause:** Port already in use  
**Solution:**

- Ensure `port_map` in docker-compose.yml matches the actual port
- Check that ports are not already in use
- Verify port mapping format: `"8080"` (with quotes)

### **Error 8: Extension errors**

**Cause:** Incorrect `x-casaos` structure  
**Solution:**

- Ensure `x-casaos` extension is present at both service and root levels
- Verify all required fields are present in `x-casaos` section
- Check YAML syntax and indentation

---

## 📱 Available Applications

### HelloBox

- **Category**: Payrillium
- **Description**: Simple Nginx test application for Payrillium ecosystem
- **Port**: 8080
- **Purpose**: Testing and development environment

### Payrillium Panel

- **Category**: Payrillium
- **Description**: Frontend placeholder for PayOS testing and development
- **Port**: 8339
- **Purpose**: PayOS frontend testing environment
- **Features**: Multi-language support (English/Spanish)

---

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

---

## 🔧 Development Guide

### Creating a New Application

1. **Create Application Directory**:

   ```bash
   mkdir Apps/YourAppName
   cd Apps/YourAppName
   ```

2. **Create docker-compose.yml** (see example above)

3. **Add Application Icon**:
   - Create `icon.png` (recommended: 512x512px)
   - Place in `Apps/YourAppName/icon.png`

4. **Update category-list.json** (if adding new category):

   ```json
   [
     {
       "name": "All",
       "font": "apps",
       "description": "All Apps"
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

---

## 🔍 Troubleshooting

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

# Check for x-casaos extension
sudo grep -A 5 "x-casaos:" /var/lib/casaos/appstore/github.com/[hash]/Apps/YourApp/docker-compose.yml

# Verify category field
sudo grep "category:" /var/lib/casaos/appstore/github.com/[hash]/Apps/YourApp/docker-compose.yml
```

### Common Issues

See [Common Errors and Solutions](#common-errors-and-solutions) section above.

---

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
- **Use `category: Payrillium` for Payrillium apps**

### Validation Checklist

Before uploading your app to GitHub, verify:

- [ ] `docker-compose.yml` file exists
- [ ] Has `name` at top level
- [ ] Has `services` section
- [ ] Uses specific image version (not `latest`)
- [ ] Has `x-casaos` at service level
- [ ] Has `x-casaos` at root level
- [ ] `main` field matches service name
- [ ] `category` field exists and is valid
- [ ] `author` field is defined
- [ ] `description` has at least `en_us`
- [ ] `title` has at least `en_us`
- [ ] `icon` points to valid URL
- [ ] `architectures` field is defined
- [ ] `icon.png` file exists (192x192px)
- [ ] `screenshot-1.png` file exists
- [ ] Names without uppercase or spaces
- [ ] Tested locally before uploading

---

## 📚 References

- [CasaOS AppStore Contributing Guide](https://github.com/IceWhaleTech/CasaOS-AppStore/blob/main/CONTRIBUTING.md)
- [Docker Compose Specification](https://docs.docker.com/compose/compose-file/)
- [CasaOS Documentation](https://wiki.casaos.io/)

---

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
