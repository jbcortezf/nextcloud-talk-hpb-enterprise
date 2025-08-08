# Nextcloud Talk HPB (Signaling) Enterprise - Automated Installation

Automated Ansible playbook to deploy **Nextcloud Talk High Performance Backend (HPB) / Standalone Signaling Server** for **Nextcloud Enterprise** customers.

This script installs, configures, and prepares the signaling server to integrate with your existing Nextcloud Enterprise instance.

---

## 📋 Requirements

Before running this playbook, make sure you have:

1. **Nextcloud Server Enterprise** up and running.
2. **Struktur/Talk Key** –

   * Go to: [Nextcloud Customer Portal](https://portal.nextcloud.com/subscription)
   * Under **Nextcloud Talk** section, copy the value of **"Key"** – the playbook will ask for it.
3. **Fresh Ubuntu 24.04 AMD64** installation.
4. **DNS configured** for the HPB server hostname (must resolve to this server's public IP).
   Example:

   ```
   talk.yoursite.com  →  your-server-ip
   ```
5. Root SSH access to the HPB server.

---

## 📦 Initial Setup

Update packages and install dependencies for running this playbook:

```bash
sudo apt update && sudo apt install -y vim git ansible
```

---

## 🚀 How to Run

1. Clone this repository:

```bash
git clone https://github.com/jbcortezf/nextcloud-talk-hpb-enterprise.git
cd nextcloud-talk-hpb-enterprise
```

2. Run the playbook as root (it will prompt for configuration values):

```bash
sudo ansible-playbook talk-hpb-standalone-enterprise.yml
```

3. The playbook will ask for:

   * **Cloud URL** → Your Nextcloud main instance URL (e.g. `cloud.yoursite.com`)
   * **Talk HPB URL** → The DNS hostname of this HPB server (e.g. `talk.yoursite.com`)
   * **Talk HPB Repository Key** → The key from your customer portal

4. The script will:

   * Install **nginx**, **Certbot**, and all dependencies
   * Request an **SSL certificate** for your HPB URL (via Let's Encrypt)
   * Configure **nginx** reverse proxy for signaling
   * Add the **Spreed** repository (using your key)
   * Install the **Nextcloud Spreed full package**
   * Configure `server.conf` for the backend URL (your Nextcloud) and generate a random secret
   * Save credentials to `/root/talk-hpb-credentials.txt`
   * Schedule a reboot (required for final activation)

5. Once completed, **the system will reboot automatically**.
   After reboot, your Talk HPB server will be ready.

---

## 📑 Configuration on Nextcloud

After installation, go to your **Nextcloud Admin Settings** → **Talk** and configure:

* **Signaling server URL**:

  ```
  https://talk.yoursite.com/standalone-signaling
  ```
* **Secret**: (found in `/root/talk-hpb-credentials.txt`)

---

## 🛠 Troubleshooting

* Make sure DNS for the HPB hostname resolves to the server before running the playbook.
* Ensure ports **80** and **443** are open on your firewall.
* If SSL request fails, check `/var/log/letsencrypt/letsencrypt.log`.
* If `apt update` shows GPG key errors, ensure the key from Struktur is reachable from your network.

---

## 📬 Support & Contact

For help, bug reports, or questions, contact:

**João Cortez**
Sales Engineer at Nextcloud GmbH

For help with this script if you're Nextcloud Customer
📧 Professional Email: [joao.cortez@nextcloud.com](mailto:joao.cortez@nextcloud.com)

For any other subjects
📧 Personal Email: [joao@jbcortezf.com](mailto:joao@jbcortezf.com)

---

## ⚠️ Disclaimer

This playbook is intended **only for Nextcloud Enterprise customers** with a valid Talk HPB license.
Do not use without a valid subscription key.
