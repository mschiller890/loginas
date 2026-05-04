# loginas

A tiny CLI script to quickly open a login shell as another user.

will anyone ever see this??... noo....  
![my git profile](https://media1.tenor.com/m/IUbgjn8Ky74AAAAd/palla-deserto.gif)

## Why this exists
- Because switching users should be faster than typing the same sudo incantations every time.
- cuz i wanna

## Basic install / location
- Drop the `loginas` script into a directory on your PATH (e.g., `/usr/local/bin`) and make
  it executable: `chmod +x loginas`.

## Usage
```sh
# open a login shell as 'alice'
loginas alice

# list non-system users on the host
loginas --list

# override the shell for the target user
loginas --shell /bin/bash alice

# disable colored output (useful for piping or cron)
loginas --no-color alice

# enable debug logging
LOGINAS_DEBUG=1 loginas alice
```

## Options
- `--list`, `-l` — list non-system users (UID >= 1000)
- `--shell <path>` — force a specific shell for the target user
- `--no-color` — disable ANSI colour sequences
- `--help`, `-h` — show help

## Behavior & safety
- If run as root, the script uses `su -` to switch. Otherwise it attempts `sudo` and will
  prompt for credentials if needed.
- An audit entry is written via `logger` and appended to `/var/log/loginas.log` when possible.
  That file is only written if the script can write into `/var/log`.
- Running this will perform a real login shell (exec replaces the process). Type `exit` or
  press Ctrl-D to return.
  
## Troubleshooting
- "User does not exist"  
    use `loginas --list` to confirm available users.
- If your shell is a no-login shell (e.g., `/sbin/nologin`), pass `--shell /bin/bash`.
