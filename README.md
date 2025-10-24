# Payrillium Store
# Payrillium Store

This is the official Payrillium applications store for CasaOS/PayOS. It provides a curated collection of applications designed specifically for the Payrillium ecosystem and PayOS platform.

## 📦 Applications Included

### HelloBox

- **Description**: Simple Nginx test application for Payrillium ecosystem
- **Category**: Utilities
- **Port**: 8080
- **Purpose**: Testing and demonstration of Payrillium app integration

### PayrilliumPanel

- **Description**: Frontend placeholder for PayOS testing and development
- **Category**: Payrillium
- **Port**: 8338
- **Purpose**: Development environment for PayOS frontend components

## 🚀 Installation

### Method 1: Add to CasaOS App Store Sources

1. Open CasaOS/PayOS web interface
2. Go to **App Store** → **Settings** → **Add Source**
3. Enter the following URL:
   ```
   https://github.com/Payrillium/payrillium-store/archive/refs/heads/main.zip
   ```
4. Click **Add** and wait for the store to be indexed
5. Navigate to **App Store** and select **Payrillium** from the author filter

### Method 2: Manual Installation

1. Download the latest release from [GitHub](https://github.com/Payrillium/payrillium-store)
2. Extract the ZIP file
3. Follow CasaOS documentation for manual store integration

## 🛠️ Development

This store is actively maintained by the Payrillium team. For development, testing, or contributing:

- **Repository**: [GitHub - Payrillium Store](https://github.com/Payrillium/payrillium-store)
- **Issues**: Report bugs or request features via GitHub Issues
- **Documentation**: Check our [Wiki](https://github.com/Payrillium/payrillium-store/wiki)

## 📋 Requirements

- CasaOS 0.4.0 or higher
- Docker and Docker Compose
- Internet connection for initial app downloads

## 🔧 Configuration

Each application includes:

- Docker Compose configuration
- Proper labels for CasaOS integration
- Volume mappings for data persistence
- Environment variables for customization

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details on how to:

- Add new applications
- Improve existing configurations
- Report issues
- Submit pull requests

## 📞 Support

- **Email**: support@payrillium.com
- **Discord**: [Payrillium Community](https://discord.gg/payrillium)
- **GitHub**: [Issues](https://github.com/Payrillium/payrillium-store/issues)

---

# 🏪 How to Create Your Own App Store for PayOS

This guide will walk you through creating your own application store that will appear in PayOS/CasaOS, even if you have no technical experience.

## 📋 Prerequisites

- A GitHub account (free)
- Basic understanding of file organization
- Docker knowledge (optional, but helpful)

## 🎯 Step-by-Step Guide

### Step 1: Create a GitHub Repository

1. Go to [GitHub.com](https://github.com) and sign in
2. Click the **"+"** button → **"New repository"**
3. Name your repository: `yourname-appstore` (e.g., `john-appstore`)
4. Make it **Public** (required for CasaOS to access it)
5. Check **"Add a README file"**
6. Click **"Create repository"**

### Step 2: Create the Store Structure

Your repository should have this structure:

```
yourname-appstore/
├── metadata.json              # Store information
├── category-list.json         # Available categories
├── README.md                  # Documentation
└── Apps/
    ├── MyApp1/
    │   ├── docker-compose.yml
    │   ├── metadata.json
    │   └── icon.png
    └── MyApp2/
        ├── docker-compose.yml
        ├── metadata.json
        └── icon.png
```

### Step 3: Create Store Metadata

Create `metadata.json` in your repository root:

```json
{
  "name": "Your Name Store",
  "description": "Your custom application store for PayOS",
  "version": "1.0.0",
  "author": "Your Name",
  "developer": "Your Name Team",
  "homepage": "https://github.com/yourusername/yourname-appstore",
  "repository": "https://github.com/yourusername/yourname-appstore",
  "license": "MIT",
  "maintainer": "Your Name <your.email@example.com>",
  "categories": ["Utilities", "Media", "Development"],
  "tags": ["custom", "personal", "utilities"],
  "screenshots": [],
  "icon": "https://raw.githubusercontent.com/yourusername/yourname-appstore/main/icon.png",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z"
}
```

### Step 4: Create Category List

Create `category-list.json`:

```json
{
  "categories": [
    { "name": "All", "title": "All" },
    { "name": "Utilities", "title": "Utilities" },
    { "name": "Media", "title": "Media" },
    { "name": "Development", "title": "Development" },
    { "name": "YourName", "title": "Your Name" }
  ]
}
```

### Step 5: Add Your First Application

#### 5.1 Create App Directory

Create a folder called `Apps/MyFirstApp/` in your repository.

#### 5.2 Create Docker Compose File

Create `Apps/MyFirstApp/docker-compose.yml`:

```yaml
services:
  myfirstapp:
    image: nginx:alpine
    container_name: myfirstapp
    ports:
      - "8080:80"
    restart: unless-stopped
    labels:
      name: "My First App"
      description: "A simple test application"
      icon: "./icon.png"
      category: "Utilities"
      tagline: "Test application for my store"
      thumbnail: "./icon.png"
      author: "YourName"
      developer: "Your Name Team"
      architectures: "amd64,arm64"
      main_app: "myfirstapp"
      port_map: "8080"
      scheme: "http"
      index: "/"
      hostname: "myfirstapp"
      volumes:
        - "/DATA/AppData/MyFirstApp:/usr/share/nginx/html"
```

#### 5.3 Create App Metadata

Create `Apps/MyFirstApp/metadata.json`:

```json
{
  "name": "My First App",
  "author": "YourName",
  "description": {
    "en_us": "A simple test application for my custom store",
    "es_es": "Una aplicación de prueba simple para mi tienda personalizada"
  },
  "category": "Utilities",
  "architecture": ["amd64", "arm64"],
  "ports": [
    {
      "container": 80,
      "host": 8080,
      "description": "Web interface port"
    }
  ],
  "volumes": [
    {
      "container": "/usr/share/nginx/html",
      "host": "/DATA/AppData/MyFirstApp",
      "description": "Web content directory"
    }
  ],
  "restart": "unless-stopped"
}
```

#### 5.4 Add App Icon

1. Find a 512x512 PNG icon for your app
2. Save it as `Apps/MyFirstApp/icon.png`
3. Upload it to your repository

### Step 6: Commit and Push

1. In GitHub, click **"Add file"** → **"Upload files"**
2. Upload all your files
3. Write a commit message: `"Initial store setup"`
4. Click **"Commit changes"**

### Step 7: Test Your Store

1. Open your PayOS/CasaOS interface
2. Go to **App Store** → **Settings** → **Add Source**
3. Enter your store URL:
   ```
   https://github.com/yourusername/yourname-appstore/archive/refs/heads/main.zip
   ```
4. Click **Add**
5. Wait for indexing (may take a few minutes)
6. Go to **App Store** and look for **"YourName"** in the author filter

## 🎨 Customization Tips

### Adding More Apps

- Create new folders in `Apps/` for each application
- Follow the same structure: `docker-compose.yml`, `metadata.json`, `icon.png`
- Make sure each app has a unique port number

### Finding App Icons

- Use [Dashboard Icons](https://github.com/walkxcode/dashboard-icons) for popular apps
- Create custom icons using tools like GIMP or Canva
- Keep icons at 512x512 pixels for best quality

### Popular Docker Images

- **Web Apps**: `nginx:alpine`, `apache:alpine`
- **Databases**: `mysql:latest`, `postgres:alpine`
- **Media**: `plex:latest`, `jellyfin/jellyfin:latest`
- **Development**: `node:alpine`, `python:alpine`

## 🔧 Troubleshooting

### Store Not Appearing

- Check that your repository is **public**
- Verify the ZIP URL is correct
- Wait 5-10 minutes for indexing
- Check CasaOS logs for errors

### Apps Not Installing

- Verify Docker Compose syntax
- Check that ports are not already in use
- Ensure Docker images exist and are accessible

### Common Errors

- **"Store already exists"**: Delete the old store first
- **"Invalid format"**: Check JSON syntax in metadata files
- **"App not found"**: Verify file structure matches requirements

## 📚 Advanced Features

### Multi-Language Support

Add translations to your `metadata.json`:

```json
{
  "description": {
    "en_us": "English description",
    "es_es": "Descripción en español",
    "fr_fr": "Description en français"
  }
}
```

### Custom Categories

Add your own categories to `category-list.json`:

```json
{
  "categories": [
    { "name": "All", "title": "All" },
    { "name": "MyCategory", "title": "My Custom Category" }
  ]
}
```

### Environment Variables

Add environment variables to your Docker Compose:

```yaml
environment:
  - MY_VAR=value
  - ANOTHER_VAR=another_value
```

## 🎉 Congratulations!

You've successfully created your own application store for PayOS! Your store will now appear in the author filter, and users can install your applications just like any other store.

## 📞 Need Help?

- **GitHub Issues**: Create an issue in this repository
- **Payrillium Discord**: Join our community for support
- **Documentation**: Check CasaOS official documentation

---

**Made with ❤️ by the Payrillium Team**
