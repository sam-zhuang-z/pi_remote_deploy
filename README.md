# IR_CAM deploy

Git-push-to-deploy template for a Python project running on a Raspberry Pi.

## One-time setup per Pi

From the project root on your dev machine:

```bash
./setup.sh
```

It will prompt for the Pi's host/IP, username, project name, and a git remote
name. SSH keys to the Pi must already be set up (`ssh-copy-id user@pi`).

It then:

1. SSHes into the Pi and installs `git`, Python build tools, and `uv`
2. Creates a bare repo at `/var/git/<project>.git` and a work-tree at `/var/www/<project>`
3. Installs a `post-receive` hook that checks out the code and runs `uv sync`
4. Adds the Pi as a git remote on your dev machine

## Deploying

```bash
git push <remote-name> <branch>
```

The hook on the Pi checks out the latest code and refreshes the venv.

## Running

```bash
ssh <user>@<pi> "cd /var/www/<project> && .venv/bin/python main.py"
```

`main.py` is a placeholder — replace it with your actual entry point.

## Adding project dependencies

- **Python packages**: add to `pyproject.toml` under `dependencies`. `uv sync`
  picks them up on the next push.
- **System packages** (apt): add them inside `setup.sh` in the `setup_pi`
  function, next to the existing `apt install` lines.
