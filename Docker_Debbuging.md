# 🔍 Docker Events Debugger

Real-time, low-level insights into your Docker containers.

Most developers use `docker logs`, `docker inspect`, or even `docker exec` when debugging containers. But those tools only tell part of the story.  
This tool leverages `docker events` to expose **everything** happening at the Docker daemon level—healthcheck failures, OOM kills, volume mount issues, and more.

---

## 📦 Features

- ✅ Live stream of Docker daemon events
- 🔎 Filter by container ID, event type, or timeframe
- 📄 JSON-formatted output for automation/logging
- 🛠️ Shell wrappers for faster incident analysis

---

## 🚀 Quick Start

1. Clone the repo:

   ```bash
   git clone https://github.com/yourusername/docker-events-debugger.git
   cd docker-events-debugger
````

2. Run the debugger script:

   ```bash
   ./docker-events-debugger.sh
   ```

3. Example with filters:

   ```bash
   ./docker-events-debugger.sh --container my_container --since 5m --format json
   ```

---

## ⚙️ Options

| Option         | Description                                        |
| -------------- | -------------------------------------------------- |
| `--container`  | Filter by container name or ID                     |
| `--since`      | Show events since a time (e.g. `10m`, `1h`)        |
| `--until`      | Show events until a time                           |
| `--format`     | Output format: `json` or `plain`                   |
| `--event-type` | Filter by event type: `start`, `stop`, `die`, etc. |

---

## 📚 Example Use Cases

* Diagnosing failing health checks
* Detecting out-of-memory container terminations
* Troubleshooting volume or network mount issues
* Capturing events during CI/CD deployments

---

## 🛠️ Script: `docker-events-debugger.sh`

```bash
#!/bin/bash

CONTAINER=""
SINCE=""
UNTIL=""
FORMAT="plain"
EVENT_TYPE=""

while [[ $# -gt 0 ]]; do
  case "$1" in
    --container) CONTAINER="$2"; shift ;;
    --since) SINCE="$2"; shift ;;
    --until) UNTIL="$2"; shift ;;
    --format) FORMAT="$2"; shift ;;
    --event-type) EVENT_TYPE="$2"; shift ;;
    *) echo "Unknown option: $1"; exit 1 ;;
  esac
  shift
done

CMD="docker events"

[[ -n "$CONTAINER" ]] && CMD="$CMD --filter container=$CONTAINER"
[[ -n "$EVENT_TYPE" ]] && CMD="$CMD --filter event=$EVENT_TYPE"
[[ -n "$SINCE" ]] && CMD="$CMD --since $SINCE"
[[ -n "$UNTIL" ]] && CMD="$CMD --until $UNTIL"

if [[ "$FORMAT" == "json" ]]; then
  $CMD --format '{{json .}}'
else
  $CMD
fi
```

Make sure to give execute permission to the script:

```bash
chmod +x docker-events-debugger.sh
```

---

## 🧑‍💻 Contributing

Feel free to fork the repo and submit a PR. Open issues for bugs or suggestions.

---

## 🛡️ Requirements

* Docker installed and running
* Bash shell (Unix-like systems)

---

## 📄 License

MIT License

```

---

Let me know if you'd like a version with advanced features like logging to a file, colored output, or integration with monitoring tools.
```
