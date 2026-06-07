# Pending Installs & Configuration — WSL Ubuntu Dev Setup

## Overview

These tools were part of the original Homebrew install list and are yet to be installed or configured on WSL Ubuntu. Each entry includes the install method, apt availability, and next steps.

---

## 1. AWS CLI v2

**Status:** Not installed (apt installs outdated v1)

**Install:**
```bash
sudo apt install -y unzip curl
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
rm -rf awscliv2.zip aws
aws --version
```

**Configure:**
```bash
aws configure
```

**Add to `.zshrc`:**
```bash
# AWS CLI completions
autoload bashcompinit && bashcompinit
autoload -Uz compinit && compinit
complete -C '/usr/local/bin/aws_completer' aws
```

---

## 2. kubectl (Kubernetes CLI)

**Status:** Not installed — package name is `kubectl`, not `kubernetes-cli`

**Install:**
```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

**Add to `.zshrc`:**
```bash
# kubectl completions
source <(kubectl completion zsh)
alias k="kubectl"
```

---

## 3. Terraform

**Status:** Not installed — needs HashiCorp apt repo

**Install:**
```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install -y terraform
```

**Add to `.zshrc`:**
```bash
# Terraform completions
autoload -U +X bashcompinit && bashcompinit
complete -o nospace -C /usr/bin/terraform terraform
```

---

## 4. k9s (Kubernetes TUI)

**Status:** Not in default apt — install via binary

**Install:**
```bash
K9S_VERSION=$(curl -s https://api.github.com/repos/derailed/k9s/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -sL "https://github.com/derailed/k9s/releases/download/${K9S_VERSION}/k9s_Linux_amd64.tar.gz" | sudo tar -xz -C /usr/local/bin k9s
k9s version
```

---

## 5. dive (Docker image explorer)

**Status:** Not in default apt — install via binary

**Install:**
```bash
DIVE_VERSION=$(curl -sL "https://api.github.com/repos/wagoodman/dive/releases/latest" | grep '"tag_name":' | sed -E 's/.*"v([^"]+)".*/\1/')
curl -OL "https://github.com/wagoodman/dive/releases/download/v${DIVE_VERSION}/dive_${DIVE_VERSION}_linux_amd64.deb"
sudo apt install -y ./dive_${DIVE_VERSION}_linux_amd64.deb
rm dive_*.deb
```

---

## 6. stern (Kubernetes log tailer)

**Status:** Not in default apt — install via binary

**Install:**
```bash
STERN_VERSION=$(curl -s https://api.github.com/repos/stern/stern/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -sL "https://github.com/stern/stern/releases/download/${STERN_VERSION}/stern_linux_amd64.tar.gz" | sudo tar -xz -C /usr/local/bin stern
stern --version
```

---

## 7. MongoDB Atlas CLI

**Status:** Not installed — needs MongoDB apt repo

**Install:**
```bash
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-7.0.gpg
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt update && sudo apt install -y mongodb-atlas
atlas --version
```

---

## 8. buku (Browser bookmark manager)

**Status:** Not in default apt — install via pip or binary

**Install:**
```bash
sudo apt install -y python3-pip
pip3 install buku
# or
pipx install buku
```

---

## 9. tlrc (tldr client in Rust)

**Status:** Not in default apt — install via binary release

**Install:**
```bash
TLRC_VERSION=$(curl -s https://api.github.com/repos/tldr-pages/tlrc/releases/latest | grep tag_name | cut -d '"' -f 4)
curl -sL "https://github.com/tldr-pages/tlrc/releases/download/${TLRC_VERSION}/tlrc-${TLRC_VERSION}-x86_64-unknown-linux-musl.tar.gz" | sudo tar -xz -C /usr/local/bin tldr
tldr --version
```

---

## 10. Powerlevel10k (Zsh prompt theme)

**Status:** Not in apt — manual install via git

**Install:**
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >> ~/.zshrc
source ~/.zshrc
# Run the configuration wizard:
p10k configure
```

**Prerequisites — install a Nerd Font first:**
Download and install one of these in Windows Terminal:
- [MesloLGS NF](https://github.com/romkatv/powerlevel10k#fonts) (recommended by p10k)

Then set the font in Windows Terminal → Settings → Ubuntu profile → Font face → `MesloLGS NF`.

---

## Quick Reference

| Tool | Method | Priority |
|------|--------|----------|
| AWS CLI v2 | Binary installer | High |
| kubectl | apt (k8s repo) | High |
| Terraform | apt (HashiCorp repo) | High |
| k9s | Binary release | Medium |
| dive | .deb release | Medium |
| stern | Binary release | Medium |
| MongoDB Atlas CLI | apt (MongoDB repo) | Medium |
| buku | pipx | Low |
| tlrc | Binary release | Low |
| Powerlevel10k | git clone | High (aesthetic) |

