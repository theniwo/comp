# 🐳 COMP – Compose Organization and Management Program

**COMP** is a lightweight Bash-based command-line tool for organizing and managing multiple Docker Compose projects from a single location.

It simplifies everyday container administration by providing a consistent interface for starting, stopping, restarting, updating, editing, and removing Docker Compose deployments without manually navigating between project directories.

## ✨ Features

- 📁 Centralized management of multiple Docker Compose projects
- 🔍 Automatic detection of `compose.yml`, `compose.yaml`, `docker-compose.yml`, and `docker-compose.yaml`
- 🚀 Start, stop, restart, and recreate services
- 📦 Pull container images
- ✏️ Edit existing configurations or create new projects
- 📋 Display Compose configurations and container status
- 🔒 Interactive safety confirmations for `purge` and `delete`
- 🛠️ Support for Docker Compose V2 and legacy `docker-compose`
- 🐚 Lightweight Bash implementation without additional runtime dependencies beyond Docker

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

## 🚀 Usage

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
```

## 📖 Available Commands

| Command | Description |
|---|---|
| `start`, `up` | Start services in detached mode |
| `stop` | Stop running services |
| `restart`, `reload` | Restart services |
| `recreate`, `rebuild` | Run `down` followed by `up -d` |
| `remove`, `down` | Stop and remove services |
| `purge` | Remove services and Compose-managed volumes |
| `pause`, `halt` | Pause services |
| `unpause`, `unhalt` | Resume services |
| `pull` | Pull container images |
| `status`, `ps` | Display service status |
| `cat`, `ls`, `list` | Display Compose configuration |
| `edit` | Edit or create a Compose configuration |
| `delete`, `rm` | Delete an entire project directory |
| `mod` | Edit the COMP script itself |
| `-h`, `--help` | Display help |

Unknown actions are forwarded to `docker`, with project arguments treated as container names.

## 🔒 Safety Features

COMP requires explicit confirmation for destructive operations.

| Operation | Confirmation |
|---|---|
| `delete`, `rm` | Required |
| `purge` | Required |
| `down`, `recreate`, `stop` | Not required |

The confirmation prompt defaults to **No**. Pressing Enter without explicitly confirming skips the destructive action.

`purge` executes `docker compose down -v --remove-orphans`, which can permanently remove persistent data in Docker volumes.

`delete` removes the entire project directory, including files and bind-mounted data stored within it, but does not automatically stop running containers.

## 🛠️ Customization

Supported configuration filenames:

```bash
COMPOSE_FILES=("docker-compose" "compose")
COMPOSE_EXT=("yml" "yaml")
```

Default filename for newly created projects:

```bash
DEFAULT_COMPOSE_FILE="docker-compose.yml"
```

To suppress the legacy Compose fallback notification:

```bash
export NOCOMPOSE=1
```

## ⚠️  Limitations

- Single-level project directories
- Sequential processing of multiple projects
- Only the first matching Compose file is selected
- No dedicated handling of additional Compose override files
- No automatic backup or rollback
- `rebuild` is an alias for `recreate` and does not rebuild images
- Interactive confirmation requires terminal input

## 📄 License

COMP – Compose Organization and Management Program is free and open-source software licensed under the [MIT License](LICENSE).

Copyright © 2026 theniwo.

You are free to use, modify, distribute, and incorporate this software into personal or commercial projects, provided that the original copyright and license notices are preserved.

See the [LICENSE](LICENSE) file for the full license text.


## 🤖 AI-Note
This project is based on a small personal project and was vibe coded further to implement improvements.

