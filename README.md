<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" alt="WhatsApp Logo" width="80" />
</p>

<h1 align="center">Multi-Device WhatsApp Web UI</h1>

<p align="center">
  <em>A web application to interact with multiple WhatsApp accounts simultaneously.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Node.js-20+-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/Vue.js-3-4FC08D?style=for-the-badge&logo=vuedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/Express-5-000000?style=for-the-badge&logo=express&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-3-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white" />
</p>

---

## 📑 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Deployment Options](#-deployment-options)
  - [Option 1: Local Development (npm)](#-option-1-local-development-npm)
  - [Option 2: Docker on a VPS](#-option-2-docker-on-a-vps)
  - [Option 3: Heroku](#-option-3-heroku)
- [Environment Variables](#-environment-variables)
- [Default Users](#-default-users)
- [Usage Guide](#-usage-guide)
- [Troubleshooting](#-troubleshooting)
- [License](#-license)

---

## ✨ Features

- **Multi-Session Management** — Add, remove, and switch between multiple WhatsApp accounts via QR code scanning.
- **Send Text Messages** — Send individual or bulk text messages with number validation.
- **Send Media** — Send images and videos via file upload or URL with optional captions.
- **Send Voice Notes** — Upload and send audio files as WhatsApp voice notes.
- **Send Location** — Share GPS coordinates with a description.
- **Bulk Number Check** — Verify large lists of phone numbers against WhatsApp registration, with country code support.
- **Bulk Send** — Send messages to multiple recipients with configurable time intervals and stop/resume control.
- **Real-time Chat View** — Browse chats, view incoming/outgoing messages in real-time via Socket.IO.
- **Contact Info Lookup** — Retrieve profile picture, push name, and registration details for any contact.
- **Set Status (About)** — Update your WhatsApp "About" status from the web UI.
- **Session Toggles** — Enable/disable typing indicator simulation, auto "seen" receipts, and online presence per session.
- **JWT Authentication** — Login system with token-based auth protecting all API endpoints and socket connections.
- **Dark Mode** — Toggle between light and dark themes, with system preference detection.
- **Responsive Design** — Works on desktop and mobile browsers.

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | Vue 3 (Composition API), Pinia, Vue Router, Tailwind CSS 3, Socket.IO Client, QRCode.vue |
| **Backend** | Node.js 20, Express 5, Socket.IO, whatsapp-web.js, Puppeteer, JWT, bcryptjs |
| **Containerization** | Docker (multi-stage build), Docker Compose |
| **Browser Engine** | Chromium (bundled inside Docker via `apt-get`) |

---

## 🏗 Architecture

```
┌─────────────────────────────────────────────────┐
│                   Browser                       │
│  Vue 3 SPA  ←──── Socket.IO ────→  Backend     │
│  (Tailwind)       (Real-time)       (Express)   │
└───────────────────────┬─────────────────────────┘
                        │
              ┌─────────▼──────────┐
              │  whatsapp-web.js   │
              │  (Puppeteer +      │
              │   Chromium)        │
              └─────────┬──────────┘
                        │
              ┌─────────▼──────────┐
              │   WhatsApp Web     │
              │   (Multi-device)   │
              └────────────────────┘
```

In production (Docker/Heroku), the backend serves the built Vue frontend as static files from a single port. In local development, the frontend runs on its own Vite dev server.

---

## 📋 Prerequisites

| Requirement | Minimum Version | Notes |
|---|---|---|
| **Node.js** | 20.x | Required for local development |
| **npm** | 8.x | Comes with Node.js |
| **Docker** | 20.x | Required for Docker deployment |
| **Docker Compose** | 2.x | Required for Docker deployment |
| **Git** | Any | To clone the repository |

---

## 🚀 Deployment Options

---

### 💻 Option 1: Local Development (npm)

<p>
  <img src="https://img.shields.io/badge/Platform-Local_Machine-grey?style=for-the-badge&logo=windows&logoColor=white" />
  <img src="https://img.shields.io/badge/macOS-Compatible-000000?style=for-the-badge&logo=apple&logoColor=white" />
  <img src="https://img.shields.io/badge/Linux-Compatible-FCC624?style=for-the-badge&logo=linux&logoColor=black" />
</p>

Best for **testing and development**. Runs the backend and frontend separately.

#### Step 1: Clone the Repository

```bash
git clone https://github.com/azhdaha-100kg/wwebjs-webui-dockerized.git
cd wwebjs-webui-dockerized
```

#### Step 2: Configure Environment Variables

```bash
cp .env.example .env
```

Edit `.env` with your settings:

```env
NODE_ENV=development
PORT=3000
JWT_SECRET=your_local_dev_secret_key_here
ADMIN_PASSWORD=admin123
USER1_PASSWORD=user1pass
USER2_PASSWORD=user2pass
CORS_ORIGIN=http://localhost:8787
```

> **Important:** In local development, `CORS_ORIGIN` must point to the Vite dev server URL (`http://localhost:8787`), not the backend port.

#### Step 3: Install & Start the Backend

```bash
cd backend
npm install
node server.js
```

The backend will start on `http://localhost:3000`.

> **Note:** You need Google Chrome or Chromium installed on your local machine for Puppeteer/whatsapp-web.js to work. On some systems you may need to set `PUPPETEER_EXECUTABLE_PATH` to your Chrome binary path.

#### Step 4: Install & Start the Frontend (new terminal)

```bash
cd vue-frontend/vue-whatsapp-frontend
npm install
npm run dev
```

The frontend will start on `http://localhost:8787`.

#### Step 5: Open in Browser

Navigate to **`http://localhost:8787`** and log in with the credentials you set in `.env`.

#### Stopping

Press `Ctrl+C` in both terminal windows.

---

### 🐳 Option 2: Docker on a VPS

<p>
  <img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/DigitalOcean-0080FF?style=for-the-badge&logo=digitalocean&logoColor=white" />
  <img src="https://img.shields.io/badge/AWS_EC2-FF9900?style=for-the-badge&logo=amazonec2&logoColor=white" />
  <img src="https://img.shields.io/badge/Vultr-007BFC?style=for-the-badge&logo=vultr&logoColor=white" />
  <img src="https://img.shields.io/badge/Hetzner-D50C2D?style=for-the-badge&logo=hetzner&logoColor=white" />
  <img src="https://img.shields.io/badge/Linode-00A95C?style=for-the-badge&logo=linode&logoColor=white" />
</p>

Best for **production deployment**. Everything runs in a single Docker container with Chromium bundled.

#### Recommended VPS Specs

| Resource | Minimum | Recommended |
|---|---|---|
| **RAM** | 1 GB | 2 GB+ |
| **CPU** | 1 vCPU | 2 vCPU |
| **Disk** | 20 GB | 30 GB+ |
| **OS** | Ubuntu 22.04+ | Ubuntu 24.04 |

#### Step 1: Prepare Your VPS

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Install Docker Compose (if not bundled)
sudo apt install docker-compose-plugin -y

# Add your user to the docker group (log out & back in after)
sudo usermod -aG docker $USER
```

#### Step 2: Clone the Repository

```bash
git clone https://github.com/azhdaha-100kg/wwebjs-webui-dockerized.git
cd wwebjs-webui-dockerized
```

#### Step 3: Configure Environment Variables

```bash
cp .env.example .env
nano .env
```

Edit `.env` for production:

```env
NODE_ENV=production
PORT=3000
JWT_SECRET=generate_a_very_strong_random_string_here
ADMIN_PASSWORD=YourStrongAdminPassword
USER1_PASSWORD=YourStrongUser1Password
USER2_PASSWORD=YourStrongUser2Password
CORS_ORIGIN=http://YOUR_VPS_IP:3000
APP_HOST_PORT=3000
```

> **Replace** `YOUR_VPS_IP` with your actual VPS public IP address (e.g., `http://128.199.100.50:3000`).

> **Security Tip:** Generate a strong JWT secret with: `openssl rand -hex 32`

#### Step 4: Build and Start

```bash
docker compose up -d --build
```

This will:
1. Build the Vue frontend (Stage 1)
2. Set up the Node.js backend with Chromium (Stage 2)
3. Serve everything on the configured port

#### Step 5: Verify It's Running

```bash
# Check container status
docker compose ps

# View logs
docker compose logs -f app
```

#### Step 6: Access the Application

Open your browser and navigate to:

```
http://YOUR_VPS_IP:3000
```

#### Managing the Container

```bash
# Stop the application
docker compose down

# Restart the application
docker compose restart

# Rebuild after code changes
docker compose up -d --build

# View real-time logs
docker compose logs -f app

# Remove everything including volumes (WARNING: deletes session data)
docker compose down -v
```

#### Firewall Configuration

Make sure your VPS firewall allows traffic on the app port:

```bash
# UFW (Ubuntu)
sudo ufw allow 3000/tcp
sudo ufw enable

# Or if using port 80
sudo ufw allow 80/tcp
```

#### Session Data Persistence

WhatsApp session data is stored in a Docker volume (`.wwebjs_auth_data`). This persists across container restarts, so you don't need to re-scan QR codes after restarting the container.

---

### ☁️ Option 3: Heroku

<p>
  <img src="https://img.shields.io/badge/Heroku-430098?style=for-the-badge&logo=heroku&logoColor=white" />
</p>

Best for **quick deployment** without managing infrastructure. Uses Heroku's container stack.

> **Note:** Heroku's free tier has been discontinued. You'll need at least a **Basic** or **Standard** dyno plan. Heroku dynos restart periodically, which means WhatsApp sessions will need to be re-authenticated after restarts unless you configure persistent storage.

#### Method A: Heroku Button (Easiest)

If a "Deploy to Heroku" button is configured in the repo, click it and fill in the environment variables when prompted. The `app.json` file in the repository defines the required configuration.

#### Method B: Heroku CLI

##### Step 1: Install the Heroku CLI

```bash
# macOS
brew tap heroku/brew && brew install heroku

# Ubuntu/Debian
curl https://cli-assets.heroku.com/install.sh | sh

# Or via npm
npm install -g heroku
```

##### Step 2: Login and Create App

```bash
heroku login
heroku create your-app-name
```

##### Step 3: Set the Stack to Container

```bash
heroku stack:set container -a your-app-name
```

##### Step 4: Set Environment Variables

```bash
heroku config:set NODE_ENV=production -a your-app-name
heroku config:set JWT_SECRET=$(openssl rand -hex 32) -a your-app-name
heroku config:set ADMIN_PASSWORD=YourStrongAdminPassword -a your-app-name
heroku config:set USER1_PASSWORD=YourStrongUser1Password -a your-app-name
heroku config:set USER2_PASSWORD=YourStrongUser2Password -a your-app-name
heroku config:set CORS_ORIGIN=https://your-app-name.herokuapp.com -a your-app-name
```

> **Important:** On Heroku, `CORS_ORIGIN` should use `https://` since Heroku provides SSL by default. Do **not** include a port number.

##### Step 5: Deploy

```bash
git push heroku main
```

##### Step 6: Open the App

```bash
heroku open -a your-app-name
```

##### Step 7: Monitor Logs

```bash
heroku logs --tail -a your-app-name
```

#### Heroku Considerations

| Topic | Details |
|---|---|
| **Dyno Sleep** | Basic dynos sleep after 30 min of inactivity. Use Standard or higher for always-on. |
| **Ephemeral Filesystem** | Heroku's filesystem resets on every deploy/restart. WhatsApp sessions stored locally will be lost. Consider using an external storage add-on for persistence. |
| **PORT** | Heroku assigns a dynamic `PORT` via environment variable. The app already reads `process.env.PORT`, so this works automatically. |
| **WebSockets** | Heroku supports WebSockets. Socket.IO will work out of the box. |

---

## 🔐 Environment Variables

| Variable | Description | Default | Required |
|---|---|---|---|
| `NODE_ENV` | Application environment | `production` | No |
| `PORT` | Port inside the container | `3000` | No |
| `JWT_SECRET` | Secret key for JWT token signing | `defaultjwtsecretfordev` | **Yes (production)** |
| `ADMIN_PASSWORD` | Password for the `admin` user | `adminpassword` | **Yes** |
| `USER1_PASSWORD` | Password for `User1` | `user1password` | No |
| `USER2_PASSWORD` | Password for `User2` | `user2password` | No |
| `CORS_ORIGIN` | Allowed origin(s) for CORS. Comma-separated for multiple. | `http://localhost:3000` | **Yes** |
| `APP_HOST_PORT` | Host port mapping (Docker only) | `3000` | No |

---

## 👤 Default Users

The application comes with three pre-configured users. Passwords are set via environment variables.

| Username | Password Variable | Default Password |
|---|---|---|
| `admin` | `ADMIN_PASSWORD` | `adminpassword` |
| `User1` | `USER1_PASSWORD` | `user1password` |
| `User2` | `USER2_PASSWORD` | `user2password` |

> **Warning:** Always change the default passwords before deploying to production!

---

## 📖 Usage Guide

### 1. Login

Open the application URL in your browser and sign in with your credentials.

### 2. Create a Session

Enter a session name (e.g., `MyPhone`) in the "New Session ID" field and click **Add Device**.

### 3. Scan QR Code

A QR code will appear. Open WhatsApp on your phone, go to **Linked Devices**, and scan the QR code.

### 4. Start Using Features

Once the session shows **"Client Ready!"**, you can:

- **Send Text** — Send messages to any WhatsApp number
- **Send Image** — Upload or link to an image/video to send
- **Send Location** — Share GPS coordinates
- **Bulk Check Numbers** — Paste a list of numbers to verify WhatsApp registration
- **Bulk Send** — Send a message to multiple recipients with delay intervals
- **Get Contact Info** — Look up profile details of any contact
- **Set Status** — Update your WhatsApp "About" text
- **View Chats** — Browse your chat list and view messages in real-time

### 5. Session Settings

When a session is active, you can toggle:

- **Show "typing..."** — Simulates typing indicator before sending
- **Auto Send "seen"** — Automatically marks messages as read when you open a chat
- **Set Presence "Online"** — Keeps your WhatsApp status as "online"

---

## 🔧 Troubleshooting

### QR Code not appearing

- Check the backend logs for Chromium/Puppeteer errors.
- On local development, ensure Chrome/Chromium is installed.
- In Docker, the Dockerfile installs Chromium automatically.

### Session disconnects frequently

- Ensure your VPS has enough RAM (2 GB+ recommended).
- Check if WhatsApp has kicked the linked device from your phone settings.
- Verify that the Docker volume is persisting session data correctly.

### CORS errors in browser console

- Double-check that `CORS_ORIGIN` in your `.env` matches exactly what you see in the browser URL bar (including protocol and port).
- For local dev: `http://localhost:8787` (Vite) — **not** the backend port.
- For Docker/Heroku: use the URL you access in the browser.

### Heroku: Application Error (H10)

- Run `heroku logs --tail` to see the actual error.
- Ensure all required environment variables are set.
- Confirm the stack is set to `container`: `heroku stack -a your-app-name`

### Docker: Container keeps restarting

```bash
# Check logs for errors
docker compose logs app

# Common fixes:
# - Increase shared memory for Chromium
# - Add to docker-compose.yml under the app service:
#   shm_size: '1gb'
```

---

## 📄 License

This project is open source. See the repository for license details.

---

<p align="center">
  <sub>Built with ❤️ using Vue 3, Express, and whatsapp-web.js</sub>
</p>
