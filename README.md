# Linux Operations Handbook for DevOps

---

## Table of Contents

1. [User and Account Management](https://www.google.com/search?q=%231-user-and-account-management)
2. [Files and Directories](https://www.google.com/search?q=%232-files-and-directories)
3. [Permissions and Ownership](https://www.google.com/search?q=%233-permissions-and-ownership)
4. [Process Management](https://www.google.com/search?q=%234-process-management)
5. [Networking and Connectivity](https://www.google.com/search?q=%235-networking-and-connectivity)
6. [Disk and Memory Monitoring](https://www.google.com/search?q=%236-disk-and-memory-monitoring)
7. [System Services and Logs](https://www.google.com/search?q=%237-system-services-and-logs)
8. [Package Management](https://www.google.com/search?q=%238-package-management)
9. [Archives and Compression](https://www.google.com/search?q=%239-archives-and-compression)
10. [Text Processing and Log Analysis](https://www.google.com/search?q=%2310-text-processing-and-log-analysis)
11. [Terminal Editors](https://www.google.com/search?q=%2311-terminal-editors)
12. [Linux Administration Best Practices](https://www.google.com/search?q=%2312-linux-administration-best-practices)

---

## 1. User and Account Management

 Account-related commands help identify the active user, inspect group membership, and manage authentication.

| Command | Description | Example |
| --- | --- | --- |
| `whoami` | Displays the effective username | `whoami` |
| `id` | Shows user and group identifiers | `id deploy` |
| `groups` | Lists a user's group memberships | `groups` |
| `passwd` | Changes a password | `passwd` |

### Example

whoami
id
groups

> **DevOps Use Case:** Check your account and permissions before executing administrative commands or troubleshooting access problems.

---

## 2. Files and Directories

 Filesystem commands are used to navigate server directories, manage application files, and locate configuration or log files.

| Command | Description | Example |
| --- | --- | --- |
| `pwd` | Prints the current directory | `pwd` |
| `ls` | Lists directory contents | `ls -lah` |
| `cd` | Changes the current directory | `cd /var/log` |
| `mkdir` | Creates directories | `mkdir -p app/config` |
| `touch` | Creates a file or updates its timestamp | `touch app.log` |
| `cp` | Copies files or directories | `cp app.conf app.conf.bak` |
| `mv` | Moves or renames files | `mv old.conf new.conf` |
| `rm` | Removes files | `rm temporary.txt` |
| `find` | Searches for matching files | `find . -type f -name "*.log"` |

### Useful Options

| Option | Purpose |
| --- | --- |
| `ls -a` | Includes hidden entries |
| `ls -lh` | Displays readable file sizes |
| `mkdir -p` | Creates missing parent directories |
| `cp -r` | Copies directories recursively |
| `find -type f` | Matches regular files |

### Example: Find Application Logs

cd /var/log
find . -type f -name "*.log"

> **Caution:** Verify the target path before running destructive commands such as `rm -rf`.

---

## 3. Permissions and Ownership

 Linux access controls determine which users can read, modify, or execute files. Correct permissions are particularly important for deployment scripts and sensitive configuration files.

| Command | Description | Example |
| --- | --- | --- |
| `ls -l` | Displays permissions and ownership | `ls -l deploy.sh` |
| `chmod` | Changes file permissions | `chmod +x deploy.sh` |
| `chown` | Changes file ownership | `sudo chown deploy:deploy app.log` |

### Common Permission Modes

chmod 644 app.conf
chmod 755 deploy.sh
chmod 600 ~/.ssh/id_ed25519

| Mode | Meaning |
| --- | --- |
| `644` | Owner can read and write; others can read |
| `755` | Owner can read, write, and execute; others can read and execute |
| `600` | Only the owner has read and write permissions |

> **Note:** Use recursive permission changes carefully, especially on production systems.

---

## 4. Process Management

 Process-management tools help investigate high resource usage, inspect running applications, and control processes or shell jobs.

| Command | Description | Example |
| --- | --- | --- |
| `ps` | Lists running processes | `ps aux` |
| `top` | Monitors processes interactively | `top` |
| `pgrep` | Searches for processes by name | `pgrep -af nginx` |
| `kill` | Sends a signal to a process | `kill 1234` |
| `jobs` | Lists jobs in the current shell | `jobs -l` |
| `fg` | Brings a job to the foreground | `fg %1` |
| `bg` | Resumes a stopped job in the background | `bg %1` |

### Stopping an Unresponsive Process

# Request normal termination

kill -TERM 1234

# Force termination if necessary

kill -KILL 1234

> **Tip:** Replace `1234` with the actual process ID. Prefer graceful termination because a forced kill does not allow normal cleanup.

---

## 5. Networking and Connectivity

 Networking commands help verify connectivity, inspect listening ports, test HTTP endpoints, and access remote machines.

| Command | Description | Example |
| --- | --- | --- |
| `ping` | Tests network reachability | `ping -c 4 example.com` |
| `curl` | Sends HTTP requests or retrieves data | `curl -I [https://example.com](https://example.com)` |
| `ssh` | Connects to a remote host | `ssh deploy@server.example.com` |
| `ss` | Inspects network sockets | `ss -ltn` |
| `ip` | Displays network interfaces and routes | `ip addr` |

### Common Networking Tasks

# Display network interfaces

ip addr

# Inspect routing information

ip route

# List listening TCP ports

ss -ltn

# Retrieve HTTP response headers

curl -I [https://example.com](https://www.google.com/url?sa=E&source=gmail&q=https://example.com)

# Connect using a custom SSH port

ssh -p 2222 deploy@192.0.2.10

> **Note:** A failed ping does not necessarily indicate that a server is offline. ICMP traffic may be blocked by firewalls or network policies.

---

## 6. Disk and Memory Monitoring

 Resource-monitoring commands help identify storage shortages, inspect memory consumption, and review available block devices.

| Command | Description | Example |
| --- | --- | --- |
| `df` | Reports filesystem capacity | `df -h` |
| `du` | Measures file or directory usage | `du -sh /var/log` |
| `free` | Displays RAM and swap statistics | `free -h` |
| `lsblk` | Lists block devices | `lsblk -f` |

### Resource Inspection

# Check filesystem usage

df -h

# Summarize log-directory size

du -sh /var/log

# Display memory and swap usage

free -h

# Inspect disks and filesystem types

lsblk -f

> **Troubleshooting Workflow:** Use `df -h` to identify a full filesystem, then investigate the relevant directories with `du` to locate large files or accumulated logs.

---

## 7. System Services and Logs

 On systemd-based Linux distributions, `systemctl` manages services while `journalctl` provides access to system and application logs.

| Command | Description | Example |
| --- | --- | --- |
| `uname` | Displays kernel information | `uname -a` |
| `uptime` | Reports uptime and load averages | `uptime` |
| `systemctl status` | Checks service status | `systemctl status nginx` |
| `systemctl restart` | Restarts a service | `sudo systemctl restart nginx` |
| `systemctl enable` | Enables startup at boot | `sudo systemctl enable nginx` |
| `journalctl` | Reads systemd journal entries | `sudo journalctl -u nginx` |

### Investigating Service Failures

# Inspect service status

sudo systemctl status nginx

# Display recent service logs

sudo journalctl -u nginx -n 100

# Follow new log entries

sudo journalctl -u nginx -f

### Useful Journal Options

* `-n 100` — Display the latest 100 entries.
* `-f` — Follow new log entries.
* `-u nginx` — Filter entries for the specified service.

> **Caution:** Review logs and identify the cause of a failure before restarting a production service.

---

## 8. Package Management

 The following commands use APT, the package-management system available on Debian and Ubuntu.

| Command | Description | Example |
| --- | --- | --- |
| `apt update` | Refreshes package metadata | `sudo apt update` |
| `apt install` | Installs software | `sudo apt install nginx` |
| `apt upgrade` | Upgrades installed packages | `sudo apt upgrade` |
| `apt remove` | Removes an installed package | `sudo apt remove nginx` |
| `apt search` | Searches available packages | `apt search nginx` |

### Installing a Web Server

# Refresh package information

sudo apt update

# Install Nginx

sudo apt install nginx

# Verify the service

sudo systemctl status nginx

> **Note:** Package managers differ between Linux distributions. For example, Fedora uses DNF, while Arch Linux uses Pacman.

---

## 9. Archives and Compression

 Archive utilities package files for backups, application releases, and transfers between environments.

| Command | Description | Example |
| --- | --- | --- |
| `tar` | Creates or extracts archives | `tar -czf backup.tar.gz app/` |
| `gzip` | Compresses files | `gzip access.log` |
| `zip` | Creates ZIP archives | `zip -r app.zip app/` |
| `unzip` | Extracts ZIP archives | `unzip app.zip` |

### Working with TAR Archives

# Create a compressed archive

tar -czf app-backup.tar.gz app/

# List archived files

tar -tzf app-backup.tar.gz

# Extract the archive

tar -xzf app-backup.tar.gz

### Important TAR Options

* `-c` — Create an archive.
* `-x` — Extract an archive.
* `-z` — Use gzip compression.
* `-t` — List archive contents.
* `-f` — Specify the archive filename.

---

## 10. Text Processing and Log Analysis

 Text utilities make it easier to inspect configuration files, search application output, and investigate errors on Linux servers.

| Command | Description | Example |
| --- | --- | --- |
| `cat` | Prints file contents | `cat app.conf` |
| `less` | Opens a file for interactive viewing | `less app.log` |
| `head` | Displays the beginning of a file | `head -n 15 app.log` |
| `tail` | Displays the end of a file | `tail -n 50 app.log` |
| `grep` | Searches for matching text | `grep -i "error" app.log` |
| `echo` | Prints text or variable values | `echo "$HOME"` |
| `wc` | Counts lines, words, or bytes | `wc -l app.log` |

### Log-Analysis Examples

# Search for failed operations

grep -i "failed" app.log

# Search recursively through application logs

grep -rni "exception" /var/log/myapp/

# Follow a live log file

tail -f app.log

# Count lines containing errors

grep -ic "error" app.log

### Redirecting Command Output

# Write output to a file, replacing existing contents

echo "Deployment started" > deploy.log

# Append output without replacing existing contents

echo "Deployment completed" >> deploy.log

* `>` overwrites the destination file.
* `>>` appends output to the destination file.

> **Warning:** Check the destination before using redirection, as overwriting an important file can cause data loss.

---

## 11. Terminal Editors

 Terminal editors are useful when modifying configuration files on remote servers or working in environments without a graphical desktop.

| Editor | Description | Example |
| --- | --- | --- |
| `nano` | Simple terminal-based editing | `nano app.conf` |
| `vim` | Modal editor with advanced capabilities | `vim app.conf` |

> **Tip:** Before editing production configuration, consider making a backup and validating the changes before reloading the affected service.

---

## 12. Linux Administration Best Practices

 Follow these practices to reduce mistakes when working with servers and production infrastructure.

* **Verify the target:** Confirm the hostname, user, and working directory before changing files.
* **Avoid unnecessary deletion:** Inspect paths before executing recursive removal commands.
* **Apply least privilege:** Use `sudo` only when elevated permissions are required.
* **Investigate before restarting:** Check service status and logs before taking corrective action.
* **Secure credentials:** Restrict access to SSH private keys and sensitive configuration files.
* **Monitor resources:** Check storage and memory before running resource-intensive tasks.
* **Validate configuration:** Test configuration changes before applying them to production services.

---

## Continue Learning

Linux proficiency develops through regular practice. Start with basic filesystem operations, move on to permissions and process control, and then use networking and log-analysis commands to troubleshoot realistic scenarios.

This reference can serve as a starting point for Linux administration, cloud infrastructure, CI/CD pipelines, and routine DevOps operations.

> **Understand the command. Verify the result. Automate with confidence.**
