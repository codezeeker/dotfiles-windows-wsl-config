# Terminal Productivity Tools — npm, pip, cargo

A reference guide for modern CLI productivity tools installable via npm, pipx, and cargo. Suited for a DevOps, Kubernetes, and research-heavy workflow on Ubuntu/WSL.

---

## npm Globals

**Install all:**
```bash
npm install -g \
  tldr \
  prettier \
  http-server \
  npm-check-updates \
  fkill-cli
```

| Tool | Command | What it does |
|------|---------|--------------|
| tldr | `tldr <command>` | Simplified, example-driven man pages |
| prettier | `prettier --write .` | Opinionated code formatter for JS/TS/JSON/YAML |
| http-server | `http-server` | Instant local HTTP server in any directory |
| npm-check-updates | `ncu` | Shows and updates outdated npm package versions |
| fkill-cli | `fkill` | Interactive fuzzy process killer |

---

## pip / pipx

**Install all:**
```bash
pipx install httpie
pipx install rich-cli
pipx install posting
pipx install yt-dlp
pipx install thefuck
```

| Tool | Command | What it does |
|------|---------|--------------|
| httpie | `http GET api.example.com` | Beautiful, human-friendly HTTP client |
| rich-cli | `rich file.py` | Syntax-highlighted file viewer in terminal |
| posting | `posting` | TUI API client — Postman inside your terminal |
| yt-dlp | `yt-dlp <url>` | Download video/audio from YouTube and 1000+ sites |
| thefuck | `fuck` | Auto-corrects your last mistyped command |

---

## cargo (Rust — fastest binaries)

**Install all:**
```bash
cargo install \
  ripgrep \
  fd-find \
  bottom \
  du-dust \
  tokei \
  procs
```

| Tool | Command | What it does |
|------|---------|--------------|
| ripgrep | `rg <pattern>` | Blazing fast `grep` replacement, respects `.gitignore` |
| fd-find | `fd <name>` | Faster, friendlier `find` replacement |
| bottom | `btm` | Beautiful `htop` replacement with graphs and charts |
| du-dust | `dust` | Visual disk usage — intuitive `du` replacement |
| tokei | `tokei` | Counts lines of code by language in a project |
| procs | `procs` | Modern coloured `ps` replacement |

---

## Top 3 to Install First

Given a DevOps + research workflow, these three deliver immediate daily value:

```bash
cargo install ripgrep bottom du-dust
pipx install httpie
```

| Tool | Why first |
|------|-----------|
| `rg` (ripgrep) | Replaces `grep` everywhere — faster and smarter |
| `btm` (bottom) | Replaces `htop` — better system visibility |
| `http` (httpie) | Replaces `curl` for all API testing and debugging |

---

## Add to `updateall` alias

Keep all tools updated by appending to your existing `updateall` alias in `~/.zshrc`:

```bash
alias updateall="sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y && \
  nvm install --lts --reinstall-packages-from=current 2>/dev/null && \
  npm update -g 2>/dev/null && \
  pip3 install --upgrade pip 2>/dev/null && \
  pipx upgrade-all 2>/dev/null && \
  rustup update 2>/dev/null && \
  cargo install-update -a 2>/dev/null && \
  echo '✅ All updated'"
```

> Note: `cargo install-update` requires `cargo install cargo-update` first.

```bash
cargo install cargo-update
```

