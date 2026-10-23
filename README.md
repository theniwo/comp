# 🐳 COMP – Compose Organization and Management Program

**COMP** is a lightweight Bash-based command-line tool for organizing and managing multiple Docker Compose projects from a single location.

It provides a consistent interface for starting, stopping, restarting, updating, editing, and removing Docker Compose deployments without manually navigating between project directories.

## ✨ Features

- 📁 Centralized management of multiple Docker Compose projects
- 🔍 Automatic detection of `compose.yml`, `compose.yaml`, `docker-compose.yml`, and `docker-compose.yaml`
- 🚀 Start, stop, restart, and recreate services
- 📜 Follow real-time Compose logs via `comp logs <project>`
- 📦 Pull container images
- ✏️ Edit existing configurations or create new projects
- 📋 Display Compose configurations and container status
- 🔒 Interactive safety confirmations for `purge` and `delete`
- 🛠️ Support for Docker Compose V2 and legacy `docker-compose`
- 🐚 Lightweight Bash implementation

## 📋 Requirements

- Bash 4.3+
- Docker Engine
- Docker Compose plugin or standalone `docker-compose`
- Environment variable `d_docker_comp`
- Text editor (`$EDITOR`, defaults to `vi`)

## ⚙️ Configuration

Configure the root directory containing your Compose projects:

```bash
export d_docker_comp="$HOME/docker"
```

Each project resides in a separate subdirectory:

```text
~/docker/
├── nextcloud/
│   └── compose.yaml
├── paperless/
│   └── docker-compose.yml
├── homeassistant/
│   └── compose.yml
└── vikunja/
    └── docker-compose.yaml
```

## 🚀 Installation and usage

Save the script as `comp` and make it executable:

```bash
chmod +x comp
```

Put it on your `PATH` or invoke it as `./comp` from its directory.

```bash
comp <action> <project...>
```

Examples:

```bash
comp start nextcloud paperless
comp restart homeassistant
comp pull vikunja
comp recreate paperless
comp status nextcloud
comp edit vikunja
comp logs nextcloud
```

## 📖 Available commands

| Command | Description |
|---|---|
| `start`, `up` | Start services in detached mode |
| `stop` | Stop running services |
| `restart`, `reload` | Restart services |
| `recreate`, `rebuild` | Run `down` followed by `up -d` (does **not** build images) |
| `remove`, `down` | Stop and remove services |
| `purge` | Remove services and Compose-managed volumes |
| `pause`, `halt` | Pause services |
| `unpause`, `unhalt` | Resume services |
| `pull` | Pull container images |
| `status`, `ps` | Display service status |
| `logs`, `log` | Follow Compose logs in real time (`logs -f`) |
| `cat`, `ls`, `list` | Display Compose configuration |
| `edit` | Edit or create a Compose configuration |
| `delete`, `rm` | Delete an entire project directory |
| `mod` | Edit the COMP script itself |
| `-h`, `--help` | Display help |
| `-u`, `--usage` | Display help |

Unknown actions are forwarded to `docker`, with project arguments treated as container names (for example, `comp inspect mycontainer`).

### 📜 Following logs

```bash
comp logs nextcloud
# Equivalent to: docker compose -f <detected-compose-file> logs -f
```

Use **Ctrl+C** to stop following the logs. Because `logs -f` remains attached, when multiple projects are supplied they are **not streamed simultaneously**: the first project's log stream continues until interrupted.

## 🔒 Safety features

COMP requires explicit confirmation for destructive operations.

| Operation | Confirmation |
|---|---|
| `delete`, `rm` | Required |
| `purge` | Required |
| `down`, `recreate`, `stop` | Not required |

The confirmation prompt defaults to **No**. Pressing Enter without confirming skips the destructive action.

`purge` runs `docker compose down -v --remove-orphans`, which can permanently remove persistent data in non-external Docker volumes.

`delete` removes the entire project directory, including bind-mounted data located inside it, but does not automatically stop running containers. Both destructive actions are confirmed per project.

## 🛠️ Customization

Supported configuration filenames, in search-priority order:

```bash
COMPOSE_FILES=("docker-compose" "compose")
COMPOSE_EXT=("yml" "yaml")
```

Default filename for new projects:

```bash
DEFAULT_COMPOSE_FILE="docker-compose.yml"
```

To suppress the fallback notification when using the Compose plugin:

```bash
export NOCOMPOSE=1
```

## ⚠️ Limitations

- Single-level project directories
- Sequential processing of multiple projects
- Only the first matching Compose file is selected
- No dedicated handling of additional Compose override files
- No automatic backup or rollback
- `rebuild` is an alias for `recreate` and does not build images
- Interactive confirmation requires terminal input
- Multiple project log streams are not followed in parallel

## 📄 License

COMP – Compose Organization and Management Program is licensed under the [MIT License](LICENSE).

Copyright © 2026 theniwo.

You may use, modify, and distribute the software, including commercially, provided that the copyright and license notices are preserved. See [LICENSE](LICENSE) for the full terms.

## 🤖 AI-Note

This project is based on a small personal project and was vibe coded further to implement improvements.
