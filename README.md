# 🐳 COMP – Compose Organization and Management Program

**COMP** is a lightweight Bash-based command-line tool for organizing and managing multiple Docker Compose projects from a single location.

It simplifies everyday container administration by providing a consistent interface for starting, stopping, restarting, updating, editing, and removing Docker Compose deployments without manually navigating between project directories.

## ✨ Features

- 📁 Centralized management of multiple Docker Compose projects
- 🔍 Automatic detection of `compose.yml`, `compose.yaml`, `docker-compose.yml`, and `docker-compose.yaml`
- 🚀 Start, stop, restart, and recreate services
- 📦 Pull container images
- 📜 Follow logs for all services or individual services, or display them once with `-N` / `--no-follow`
- ✏️ Edit existing configurations or create new projects
- 📋 Display Compose configurations and container status
- 🔒 Interactive safety confirmations for `purge` and `delete`
- 🛠️ Support for the Docker Compose plugin and legacy `docker-compose`
- 🐚 Lightweight Bash implementation without additional application dependencies beyond Docker and Compose

## 📋 Requirements

- Bash 4.3+
- Docker Engine
- Docker Compose plugin (`docker compose`) or standalone `docker-compose`
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

Save the executable script as `comp` somewhere in your `PATH` (or execute it as `./comp`).

```bash
chmod +x comp
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
comp log vikunja
comp logs vikunja
```

## 📖 Available commands

| Command | Description |
|---|---|
| `start`, `up` | Start services in detached mode |
| `stop` | Stop running services |
| `restart`, `reload` | Restart services |
| `recreate`, `rebuild` | Run `down` followed by `up -d` (does not build images) |
| `remove`, `down` | Stop and remove services |
| `purge` | Remove services and Compose-managed volumes |
| `pause`, `halt` | Pause services |
| `unpause`, `unhalt` | Resume paused services |
| `pull` | Pull container images |
| `status`, `ps` | Display service status |
| `log <project> [service] [-N\|--no-follow]` | Show a single service's logs (default: `app`) |
| `logs <project> [services...] [-N\|--no-follow]` | Show all services' logs, or selected services |
| `cat`, `ls`, `list` | Display Compose configuration |
| `edit` | Edit or create a Compose configuration |
| `delete`, `rm` | Delete an entire project directory |
| `mod` | Edit the COMP script itself |
| `-h`, `--help` | Display help |
| `-u`, `--usage` | Display help |

Unknown actions are forwarded to `docker`, with arguments treated as container names.

## 📜 Log streaming

Both `log` and `logs` follow output continuously (`docker compose logs -f`) by default; stop with **Ctrl+C**. Add `-N` or `--no-follow` to omit `-f`, display the currently available logs and exit. The option is accepted after the project name, including at the end of the command.

| Example | Equivalent Compose subcommand |
|---|---|
| `comp log vikunja` | `logs -f app` |
| `comp log vikunja db` | `logs -f db` |
| `comp logs vikunja` | `logs -f` (all services) |
| `comp logs vikunja app db` | `logs -f app db` |
| `comp log vikunja db -N` | `logs db` |
| `comp logs vikunja --no-follow` | `logs` (all services, no follow) |
| `comp logs vikunja app db -N` | `logs app db` |

The first positional argument after `log` or `logs` is the **project directory name**; subsequent arguments are **Compose service names**, not other projects. `log` uses only one service argument. The default service for `log` can be changed with `DEFAULT_LOG_SERVICE="app"` in the script.

## 🔒 Safety features

Only `delete`/`rm` and `purge` ask for confirmation. The default answer is **No**; pressing Enter skips the destructive action. Other commands, including `down` and `recreate`, execute without confirmation.

- **`delete`** removes the whole project directory. It also deletes data stored in bind mounts within that directory. It does *not* stop running containers first.
- **`purge`** runs `docker compose down -v --remove-orphans` and can permanently remove data in Compose-managed Docker volumes. External volumes and host bind-mount directories are not removed by `down -v`.

Maintain backups of important application data.

## 🛠️ Customization

File detection follows the order specified by these arrays:

```bash
COMPOSE_FILES=("docker-compose" "compose")
COMPOSE_EXT=("yml" "yaml")
```

The default filename for new projects is:

```bash
DEFAULT_COMPOSE_FILE="docker-compose.yml"
```

To suppress the Compose plugin fallback notice:

```bash
export NOCOMPOSE=1
```

## ⚠️ Limitations

- Single-level project directories; projects are processed sequentially
- The first matching Compose file is selected
- No dedicated detection of additional Compose override files
- `rebuild` is an alias for `recreate` and does not actually rebuild images
- `log` accepts at most one service; `logs` accepts multiple service names for one project
- `-N` / `--no-follow` applies only to `log` and `logs`
- No automatic backup or rollback
- Interactive confirmation requires terminal input

## 📄 License

COMP – Compose Organization and Management Program is licensed under the [MIT License](LICENSE).

Copyright © 2026 theniwo.

You may use, modify, distribute, and incorporate the software into personal or commercial projects, provided the original copyright and license notices are retained. See [LICENSE](LICENSE) for the full terms.

## 🤖 AI-Note

This project is based on a small personal project and was vibe coded further to implement improvements.
