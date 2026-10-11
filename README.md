NEXUS // Linux Operations for DevOps

<p align="center"> <img src="https://capsule-render.vercel.app/api?type=rect&color=0:050816,50:071B36,100:0066FF&height=220&section=header&text=NEXUS%20LINUX&fontSize=52&fontColor=FFFFFF&fontAlignY=42&desc=SYSTEMS%20%E2%80%A2%20AUTOMATION%20%E2%80%A2%20INFRASTRUCTURE&descSize=13&descAlignY=65&animation=fadeIn" width="100%" alt="Nexus Linux - blue and black DevOps banner"/> </p>

<p align="center"> <img src="https://img.shields.io/badge/PLATFORM-LINUX-080D1B?style=for-the-badge&logo=linux&logoColor=00A3FF" alt="Linux"/> <img src="https://img.shields.io/badge/FOCUS-DEVOPS-080D1B?style=for-the-badge&logo=devops&logoColor=00A3FF" alt="DevOps"/> <img src="https://img.shields.io/badge/LEVEL-BEGINNER%20%E2%86%92%20PRACTICAL-080D1B?style=for-the-badge&logo=gnubash&logoColor=00A3FF" alt="Learning level"/> </p>

The terminal is your control plane.
Learn the commands that keep servers running, deployments moving, and infrastructure under control.

Nexus Linux is a task-oriented field guide to everyday Linux operations. It brings together the commands used to inspect systems, manage access, diagnose failures, monitor resources, and maintain application environments.

Instead of memorizing isolated commands, use this handbook to understand what each command does and when it belongs in a real DevOps workflow.

<img src="https://api.iconify.design/lucide:terminal.svg?color=%2300A3FF" width="23" align="center" alt=""/> COMMAND CENTER
Module	Operational focus
01 — Identity	Accounts, users and groups
02 — Filesystem	Navigation, files and directories
03 — Access Control	Permissions and ownership
04 — Processes	Running applications and shell jobs
05 — Connectivity	Network tests and remote access
06 — Resources	Disk capacity, memory and storage
07 — Services	systemd, system status and logs
08 — Packages	Software installation and updates
09 — Archives	Compression and backup files
10 — Text Analysis	Searching, filtering and inspecting logs
11 — Editing	Terminal-based configuration changes
<img src="https://api.iconify.design/lucide:user-round-cog.svg?color=%2300A3FF" width="23" align="center" alt=""/> 01 / IDENTITY & ACCESS

Know which account is active before changing files, running scripts, or executing privileged operations.

Command	Function	Example
whoami	Print the effective username	whoami
id	Inspect user and group identifiers	id
groups	Display group memberships	groups
passwd	Change a password	passwd
# Inspect the current account
whoami
id
groups


Operations note: When troubleshooting permission failures, compare the active user and group membership with the ownership of the affected file.

<img src="https://api.iconify.design/lucide:folder-tree.svg?color=%2300A3FF" width="23" align="center" alt=""/> 02 / FILESYSTEM CONTROL

Move through the Linux directory hierarchy, organize application files, and locate configuration or log files efficiently.

Command	What it does	Example
pwd	Prints the current location	pwd
ls	Lists directory contents	ls -lah
cd	Changes the working directory	cd /var/log
mkdir	Creates directories	mkdir -p app/config
touch	Creates a file or updates its timestamp	touch app.log
cp	Copies files	cp app.conf app.conf.bak
mv	Moves or renames files	mv old.conf new.conf
rm	Removes files	rm temporary.txt
find	Searches a directory tree	find . -type f -name "*.log"
Field examples
# Reveal hidden files and readable file sizes
ls -lah

# Create a nested application directory
mkdir -p /opt/myapp/config

# Find log files under the current directory
find . -type f -name "*.log"


Caution: Review the target before using recursive deletion. rm -rf can remove an entire directory tree without prompting.

<img src="https://api.iconify.design/lucide:shield-check.svg?color=%2300A3FF" width="23" align="center" alt=""/> 03 / PERMISSIONS & OWNERSHIP

Control who can read, modify, or execute files. These settings matter for deployment scripts, SSH credentials, and application configuration.

Command	Function	Example
ls -l	Inspects permissions and ownership	ls -l deploy.sh
chmod	Changes permission bits	chmod +x deploy.sh
chown	Changes owner or group	sudo chown deploy:deploy app.log
# Allow the owner to execute a deployment script
chmod u+x deploy.sh

# Apply a standard configuration-file permission mode
chmod 644 app.conf

# Restrict a private SSH key to its owner
chmod 600 ~/.ssh/id_ed25519

Mode	Typical use
644	Readable configuration or text file
755	Executable script or accessible directory
600	Private file restricted to its owner

Apply recursive ownership or permission changes only after confirming the target path.

<img src="https://api.iconify.design/lucide:activity.svg?color=%2300A3FF" width="23" align="center" alt=""/> 04 / PROCESS CONTROL

Identify resource-heavy processes, inspect running applications, and manage jobs launched from the current shell.

Command	Function	Example
ps	Lists process information	ps aux
top	Displays live process statistics	top
pgrep	Finds processes by name	pgrep -af nginx
kill	Sends a signal to a process	kill 1234
jobs	Lists shell-managed jobs	jobs -l
fg	Brings a job to the foreground	fg %1
bg	Resumes a stopped job in the background	bg %1
Shutdown sequence for an unresponsive process
# Inspect processes associated with an application
pgrep -af myapp

# Request graceful termination
kill -TERM 1234

# Use only if graceful termination fails
kill -KILL 1234


Replace 1234 with the actual process ID. A forced kill prevents the process from handling normal cleanup signals.

<img src="https://api.iconify.design/lucide:network.svg?color=%2300A3FF" width="23" align="center" alt=""/> 05 / NETWORK & CONNECTIVITY

Check reachability, inspect network interfaces, examine listening ports, and connect to remote machines.

Command	Purpose	Example
ping	Tests ICMP reachability	ping -c 4 example.com
curl	Makes HTTP requests	curl -I https://example.com
ssh	Opens a remote shell	ssh deploy@server.example.com
ip	Displays interfaces and routes	ip addr
ss	Inspects sockets and listening ports	ss -ltn
# Display network addresses
ip addr

# Inspect the routing table
ip route

# Check listening TCP ports
ss -ltn

# Retrieve HTTP response headers
curl -I https://example.com

# Connect using a non-default SSH port
ssh -p 2222 deploy@192.0.2.10


Diagnostic tip: A failed ping alone doesn't prove a server is offline. Check DNS resolution, routes, firewall rules, and the relevant application port as well.

<img src="https://api.iconify.design/lucide:hard-drive.svg?color=%2300A3FF" width="23" align="center" alt=""/> 06 / DISK, MEMORY & STORAGE

Monitor resource availability before full disks or memory pressure disrupt applications.

Command	Function	Example
df	Reports filesystem capacity	df -h
du	Measures file or directory usage	du -sh /var/log
free	Reports RAM and swap usage	free -h
lsblk	Lists block devices	lsblk -f
# Check mounted filesystem capacity
df -h

# Summarize a directory's disk usage
du -sh /var/log

# Review memory and swap statistics
free -h

# Inspect disks and filesystems
lsblk -f

Resource investigation

When disk space is running low:

Use df -h to identify the affected filesystem.
Use du -sh on relevant directories to locate large consumers.
Check whether logs, caches, backups, or temporary files are responsible.
Remove or rotate files only after verifying that they are safe to delete.
<img src="https://api.iconify.design/lucide:server-cog.svg?color=%2300A3FF" width="23" align="center" alt=""/> 07 / SERVICES & SYSTEM LOGS

On systemd-based distributions, manage services and inspect journal entries to understand why an application failed or stopped responding.

Command	Function	Example
uname	Reports kernel and system details	uname -a
uptime	Shows uptime and load averages	uptime
systemctl status	Checks service state	systemctl status nginx
systemctl restart	Restarts a service	sudo systemctl restart nginx
systemctl enable	Enables startup at boot	sudo systemctl enable nginx
journalctl	Reads systemd journal records	sudo journalctl -u nginx
Incident investigation sequence
# Inspect service health
sudo systemctl status nginx

# Review the most recent service messages
sudo journalctl -u nginx -n 100

# Follow new service log entries
sudo journalctl -u nginx -f


Diagnose the underlying issue before restarting a production service. Restarting can interrupt traffic without resolving the original fault.

<img src="https://api.iconify.design/lucide:package.svg?color=%2300A3FF" width="23" align="center" alt=""/> 08 / PACKAGE OPERATIONS

The following examples use APT on Debian and Ubuntu systems. Package commands differ on other Linux distributions.

Command	Purpose	Example
apt update	Refreshes package metadata	sudo apt update
apt install	Installs software	sudo apt install nginx
apt upgrade	Upgrades installed packages	sudo apt upgrade
apt remove	Removes a package	sudo apt remove nginx
apt search	Searches package names and descriptions	apt search nginx
# Refresh available package information
sudo apt update

# Install a web server
sudo apt install nginx

# Confirm service status
sudo systemctl status nginx


Review the proposed package changes before confirming an upgrade or removal on a production host.

<img src="https://api.iconify.design/lucide:archive.svg?color=%2300A3FF" width="23" align="center" alt=""/> 09 / ARCHIVES & BACKUPS

Bundle application assets, compress logs, and inspect backup archives before transferring or extracting them.

Command	Function	Example
tar	Creates or extracts archives	tar -czf backup.tar.gz app/
gzip	Compresses a file	gzip access.log
zip	Creates ZIP archives	zip -r app.zip app/
unzip	Extracts ZIP archives	unzip app.zip
# Create a gzip-compressed archive
tar -czf app-backup.tar.gz app/

# Inspect its contents without extracting
tar -tzf app-backup.tar.gz

# Extract the archive
tar -xzf app-backup.tar.gz

# Package a directory as ZIP
zip -r app.zip app/


Check archive contents and extraction destinations when handling files from an untrusted source.

<img src="https://api.iconify.design/lucide:search-code.svg?color=%2300A3FF" width="23" align="center" alt=""/> 10 / TEXT ANALYSIS & LOG FORENSICS

Search application output, examine configuration files, and monitor changing logs using standard command-line utilities.

Command	Purpose	Example
cat	Prints file contents	cat app.conf
less	Browses long files	less app.log
head	Displays the first lines	head -n 20 app.log
tail	Displays the final lines	tail -n 50 app.log
grep	Searches for matching text	grep -i "error" app.log
echo	Prints text or variable values	echo "$HOME"
wc	Counts lines, words or bytes	wc -l app.log
Useful investigation patterns
# Find errors regardless of letter case
grep -i "error" app.log

# Search nested log directories
grep -rni "exception" /var/log/myapp/

# Follow newly written entries
tail -f app.log

# Count matching error lines
grep -ic "error" app.log

# Display the last 100 lines
tail -n 100 app.log

Output redirection
# Replace the contents of a file
echo "Deployment started" > deploy.log

# Append a new line without replacing existing content
echo "Deployment completed" >> deploy.log


> overwrites the destination file, while >> appends to it. Verify the target filename before redirecting important data.

<img src="https://api.iconify.design/lucide:terminal-square.svg?color=%2300A3FF" width="23" align="center" alt=""/> 11 / TERMINAL EDITORS

Edit configuration files directly on a remote host without relying on a graphical interface.

Editor	Recommended for	Launch command
nano	Straightforward file edits	nano app.conf
vim	Modal editing and advanced terminal workflows	vim app.conf

Before changing a production configuration, create a backup when appropriate and validate the resulting file before reloading or restarting its service.

<img src="https://api.iconify.design/lucide:shield-alert.svg?color=%2300A3FF" width="23" align="center" alt=""/> OPERATIONS SAFETY CHECKLIST

Use this checklist before executing commands that can affect a live system.

Confirm the host and current working directory.
Check the target before deleting or overwriting files.
Use elevated privileges only when necessary.
Prefer graceful process termination over forced termination.
Verify permissions before making SSH keys or secrets accessible.
Inspect service logs before restarting failed applications.
Check available storage before creating large backups.
Validate configuration changes before applying them.

<p align="center"> <img src="https://capsule-render.vercel.app/api?type=rect&color=0:0066FF,50:071B36,100:050816&height=100&section=footer&text=MASTER%20THE%20SYSTEM.%20AUTOMATE%20THE%20WORK.&fontSize=18&fontColor=FFFFFF&fontAlignY=55" width="100%" alt="Master the system. Automate the work."/> </p>

<p align="center"> NEXUS LINUX / A practical reference for modern infrastructure operations. </p>
