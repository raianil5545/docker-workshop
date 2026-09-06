# Installing Docker on macOS, Windows, and Ubuntu

A quick guide to installing Docker — suitable for personal projects and teaching.

- [macOS](#macos)
- [Windows](#windows)
- [Ubuntu](#ubuntu)

---

## macOS

### Prerequisites

- macOS 12 (Monterey) or later
- Apple Silicon (M1/M2/M3/M4) or Intel processor
- At least 4GB RAM available (8GB+ recommended)
- Admin access on your Mac

### Option 1: Install via Homebrew (recommended)

If you have [Homebrew](https://brew.sh/) installed, this is the fastest method.

```bash
brew install --cask docker
```

Once installed:

1. Open **Docker** from your Applications folder (or Spotlight search)
2. Approve any system permission prompts (it may ask for your password to install networking components)
3. Wait for the whale icon in the menu bar to stop animating — that means Docker Engine is running

### Option 2: Install manually

1. Go to [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
2. Download the version for your chip:
   - **Mac with Apple Silicon**
   - **Mac with Intel chip**
3. Open the downloaded `.dmg` file
4. Drag the Docker icon into the **Applications** folder
5. Launch Docker from Applications
6. Follow the on-screen setup steps and approve permissions

### Verify the installation

Open Terminal and run:

```bash
docker --version
docker compose version
```

You should see version numbers printed for both. Then test that Docker is actually working:

```bash
docker run hello-world
```

If you see a "Hello from Docker!" message, you're all set.

### Troubleshooting (macOS)

- **Docker command not found** — Make sure Docker Desktop is actually running (check the menu bar whale icon), and restart your terminal.
- **Permission errors on `/var/run/docker.sock`** — Restart Docker Desktop from the menu bar.
- **Slow performance** — Go to Docker Desktop → Settings → Resources and increase allocated CPU/Memory.

---

## Windows

### Prerequisites

- Windows 10 64-bit (Build 19045+) or Windows 11
- WSL 2 (Windows Subsystem for Linux) enabled
- Virtualization enabled in BIOS/UEFI
- At least 4GB RAM (8GB+ recommended)
- Admin access

### Step 1: Enable WSL 2

Open **PowerShell as Administrator** and run:

```powershell
wsl --install
```

This installs WSL 2 along with a default Ubuntu distribution. Restart your PC if prompted.

Verify WSL is on version 2:

```powershell
wsl -l -v
```

### Step 2: Install Docker Desktop

**Option A — via winget (recommended)**

```powershell
winget install Docker.DockerDesktop
```

**Option B — manual download**

1. Go to [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/)
2. Download **Docker Desktop for Windows**
3. Run the installer, keep "Use WSL 2 instead of Hyper-V" checked
4. Restart when prompted

### Step 3: Launch and configure

1. Open Docker Desktop from the Start menu
2. Accept the service agreement
3. In **Settings → Resources → WSL Integration**, make sure integration is enabled for your default distro
4. Wait for the whale icon in the system tray to show Docker is running

### Verify the installation

Open PowerShell or Command Prompt:

```powershell
docker --version
docker compose version
docker run hello-world
```

### Troubleshooting (Windows)

- **"WSL 2 installation is incomplete"** — Update the WSL kernel: `wsl --update`
- **Virtualization not enabled** — Reboot into BIOS/UEFI and enable Intel VT-x / AMD-V
- **Docker Desktop won't start** — Ensure Hyper-V and Virtual Machine Platform Windows features are enabled (`Turn Windows features on or off`)

### Uninstalling (Windows)

Uninstall via **Settings → Apps → Docker Desktop**, or:

```powershell
winget uninstall Docker.DockerDesktop
```

---

## Ubuntu

Ubuntu runs Docker Engine natively (no VM needed), so this uses the **Docker Engine** package rather than Docker Desktop, though Docker Desktop for Linux is also available if you want the GUI.

### Prerequisites

- Ubuntu 22.04, 24.04, or later (64-bit)
- Sudo privileges

### Step 1: Remove old versions (if any)

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### Step 2: Set up Docker's official repository

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

### Step 3: Install Docker Engine

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Step 4: Run Docker without sudo (optional but recommended for teaching)

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Log out and back in for this to take full effect.

### Step 5: Enable Docker to start on boot

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

### Verify the installation

```bash
docker --version
docker compose version
docker run hello-world
```

### Troubleshooting (Ubuntu)

- **Permission denied on docker.sock** — You likely skipped Step 4, or need to log out/in after adding yourself to the `docker` group.
- **Docker service not running** — `sudo systemctl status docker`, then `sudo systemctl start docker` if inactive.
- **Old `docker.io` package conflicts** — Fully purge with `sudo apt-get purge docker.io` before installing Docker CE.

### Uninstalling (Ubuntu)

```bash
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

---

See `commands.md` for a quick reference of common Docker commands.