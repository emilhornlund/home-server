# GitHub Runner

This service runs a self-hosted GitHub Actions runner inside Docker using the `myoung34/github-runner` image.

## Service Info

- **Image**: `myoung34/github-runner:2.324.0`
- **Mode**: Host network mode (required for certain CI jobs)
- **Volumes**:
  - `/var/run/docker.sock` (to run Docker jobs)
  - `/runner/data` (for config state)
  - `/tmp/runner` (working directory)
- **Scope**: Repository-level runner for `emilhornlund/quiz`

## Usage

Start the runner:

```bash
docker compose up -d
````

Stop it:

```bash
docker compose down
```

## Configuration

The `.env` file must define a personal access token:

```env
GH_RUNNER_PAT=ghp_XXX
```

> This token must have `repo` and `admin:repo_hook` scopes.

## Folder Structure

```
github-runner/
├── docker-compose.yml
├── .env.example          # GitHub PAT for runner registration
└── README.md
```

## Networking

* Uses host networking (`network_mode: host`) to avoid network isolation issues.
* No ports are explicitly mapped.

## Cleanup

To remove the runner and all local state:

```bash
docker compose down -v
```

> ⚠️ This removes all cached runner data and unlinks it from GitHub.
