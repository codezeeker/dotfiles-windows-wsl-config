# Windows + WSL Ubuntu Setup Playbook

This playbook recreates the working terminal environment on another Windows desktop with Ubuntu on WSL. It includes Zsh, the current shell config, Alacritty config, fonts, and the main install commands used in the current setup.

## What this sets up

- Ubuntu on WSL with Zsh as the default shell.
- `eza`, `zoxide`, prompt customisation, history search, and shell plugins.
- Alacritty on Windows launching directly into Ubuntu home.
- MesloLGS Nerd Font for icons and prompt rendering.
- A reusable update alias and references for pending installs.

## Repo structure

```text
wsl/.zshrc
windows/alacritty/alacritty.yml
backups/current_live_.zshrc
backups/pending_installs.md
backups/productivity_tools.md
SETUP_PLAYBOOK.md
```

## 1. Clone this repo

Run in WSL Ubuntu:

```bash
cd ~
git clone https://github.com/codezeeker/dotfiles-windows-wsl-config.git
cd dotfiles-windows-wsl-config
```

## 2. Install base packages in Ubuntu

```bash
sudo apt update && sudo apt install -y \
  zsh \
  bat \
  gnupg \
  groovy \
  lynx \
  maven \
  neovim \
  pipx \
  tmux \
  tree \
  wget \
  zoxide \
  zsh-autosuggestions \
  zsh-syntax-highlighting \
  python3-pip \
  python3-dev \
  curl \
  unzip \
  git
```

## 3. Install Python 3.11 for pipx tools that require it

Ubuntu 22.04 ships with Python 3.10, but some tools need Python 3.11.

```bash
sudo add-apt-repository ppa:deadsnakes/ppa -y
sudo apt update
sudo apt install -y python3.11 python3.11-venv python3.11-dev
```

## 4. Install eza from its repo

```bash
sudo apt install -y gpg
sudo mkdir -p /etc/apt/keyrings
wget -qO- https://raw.githubusercontent.com/eza-community/eza/main/deb.asc | sudo gpg --dearmor -o /etc/apt/keyrings/gierens.gpg
echo "deb [signed-by=/etc/apt/keyrings/gierens.gpg] http://deb.gierens.de stable main" | sudo tee /etc/apt/sources.list.d/gierens.list
sudo chmod 644 /etc/apt/keyrings/gierens.gpg /etc/apt/sources.list.d/gierens.list
sudo apt update && sudo apt install -y eza
```

## 5. Install Node, npm, Rust, and cargo

### Node + npm via nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
nvm install --lts
nvm use --lts
node --version
npm --version
```

### Rust + cargo

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
source "$HOME/.cargo/env"
rustc --version
cargo --version
```

## 6. Set Zsh as default shell

```bash
chsh -s "$(which zsh)"
cp ~/dotfiles-windows-wsl-config/wsl/.zshrc ~/.zshrc
exec zsh
```

If WSL still opens Bash, shut it down from Windows and reopen:

```powershell
wsl --shutdown
```

## 7. Verify shell setup

```bash
echo $0
which zsh
cd ~
ls
lt2
z ~
```

## 8. Install MesloLGS Nerd Font on Windows

Run in Windows PowerShell:

```powershell
New-Item -ItemType Directory -Path "$env:USERPROFILE\Downloads\MesloLGS" -Force
cd "$env:USERPROFILE\Downloads\MesloLGS"
Invoke-WebRequest -Uri "https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf" -OutFile "MesloLGS NF Regular.ttf"
Invoke-WebRequest -Uri "https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf" -OutFile "MesloLGS NF Bold.ttf"
Invoke-WebRequest -Uri "https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf" -OutFile "MesloLGS NF Italic.ttf"
Invoke-WebRequest -Uri "https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf" -OutFile "MesloLGS NF Bold Italic.ttf"
explorer .
```

Open each `.ttf` file and click **Install**.

## 9. Configure Alacritty on Windows

For Alacritty 0.11, the config file is YAML.

```powershell
New-Item -ItemType Directory -Path "$env:APPDATA\alacritty" -Force
Copy-Item ".\windows\alacritty\alacritty.yml" "$env:APPDATA\alacritty\alacritty.yml" -Force
```

If running from Windows outside the repo folder, use the full repo path instead.

## 10. Test Alacritty

Open Alacritty and confirm:

- It opens directly into Ubuntu instead of PowerShell.
- It starts in `~` instead of `C:\Windows\System32`.
- The prompt shows a yellow lightning icon, the last two directories, and git branch when inside a repo.
- Icons in `eza` render correctly.

## 11. Optional productivity installs

See the included reference file:

```text
backups/productivity_tools.md
```

Recommended first installs:

```bash
pipx install httpie
cargo install ripgrep bottom du-dust
```

## 12. Optional pending tools

See the included reference file:

```text
backups/pending_installs.md
```

This contains AWS CLI, Terraform, kubectl, k9s, Powerlevel10k, and related commands.

## 13. Back up the new machine immediately

Once the second machine is working, create a dated backup:

```bash
cp ~/.zshrc ~/.zshrc.bak.$(date +%Y%m%d_%H%M%S)
mkdir -p ~/dotfiles_local_backup
cp ~/.zshrc ~/dotfiles_local_backup/
cp ~/.zhistory ~/dotfiles_local_backup/ 2>/dev/null
cp -r ~/.ssh ~/dotfiles_local_backup/ 2>/dev/null
cp -r ~/.aws ~/dotfiles_local_backup/ 2>/dev/null
```

## 14. Suggested future improvement

A stronger long-term setup would symlink live config files from the repo into the right locations, so changes are versioned automatically. Storing dotfiles in Git is a standard way to make Linux and WSL environments reproducible across machines.[web:100][web:103]
