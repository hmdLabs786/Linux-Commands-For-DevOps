Linux Operations Handbook for DevOps ⚙️

A practical reference for working with Linux servers, managing system resources, troubleshooting issues, and automating everyday infrastructure tasks.

Linux is a core part of modern DevOps workflows. Whether you're deploying applications, investigating production errors, or maintaining cloud instances, knowing your way around the terminal makes your work faster and more reliable.

This handbook organizes essential commands by real-world tasks, with short explanations, useful options, and examples you can practice on your own machine.

🧭 Quick Navigation
Account & User Administration
Navigating the Filesystem
Permissions and Access Control
Process and Job Control
Network Diagnostics
Storage and RAM Monitoring
System Services and Logs
Software Installation
Archives and Backups
Working with Text and Logs
Terminal Editing Tools
Operational Safety Tips
👤 Account & User Administration

Use these commands to identify the active account and inspect user or group membership.

Command	Purpose	Example
whoami	Identify the current user	whoami
id	Display user and group IDs	id deploy
groups	List group memberships	groups
passwd	Change a user's password	passwd

Practical scenario: Before changing file ownership or running administrative tasks, confirm which account you're using.

📁 Navigating the Filesystem

These commands help you explore directories, create files, and organize application or configuration files.

Command	Purpose	Example
pwd	Display the current directory	pwd
ls	Inspect directory contents	ls -lah
cd	Move to another directory	cd /var/log
mkdir	Create a directory	mkdir -p app/config
touch	Create a file or refresh its timestamp	touch deploy.log
cp	Copy a file	cp app.conf app.conf.bak
mv	Move or rename a file	mv old.conf new.conf
rm	Delete files	rm temporary.txt
find	Locate files by criteria	find /var/log -type f -name "*.log"
Useful options
ls -a — include hidden entries.
ls -lh — show file sizes in readable units.
mkdir -p — create missing parent directories.
cp -r — copy directories and their contents.
find ... -type f — restrict a search to regular files.

Practical scenario: Locate application logs, create a configuration directory, or back up a file before editing it.

🔐 Permissions and Access Control

Linux permissions determine who can read, modify, or execute files. Correct permissions are especially important for deployment scripts, SSH keys, and application directories.

Command	Purpose	Example
ls -l	Inspect permissions and ownership	ls -l deploy.sh
chmod	Modify access permissions	chmod +x deploy.sh
chown	Change file owner or group	sudo chown deploy:deploy app.log
Common permission examples
chmod 644 app.conf
chmod 755 deploy.sh
chmod 600 ~/.ssh/id_ed25519

644 — owner can read and write; others can read.
755 — owner can read, write, and execute; others can read and execute.
600 — only the owner has read and write access.

Remember: Avoid applying broad permissions recursively without checking the target path and the security implications.

🖥️ Process and Job Control

When a server becomes slow or an application stops responding, process-management commands help identify what is running and take appropriate action.

Command	Purpose	Example
ps	Inspect running processes	ps aux
top	Monitor processes interactively	top
pgrep	Find processes by name	pgrep -af nginx
kill	Send a signal to a process	kill 1234
jobs	List jobs in the current shell	jobs -l
fg	Resume a shell job in the foreground	fg %1
bg	Continue a stopped shell job in the background	bg %1
Terminating a process
kill 1234
kill -TERM 1234
kill -KILL 1234


Prefer the default termination signal or -TERM first. Use -KILL only when a process will not stop gracefully, because it cannot perform normal cleanup.

🌐 Network Diagnostics

Network commands are useful for checking connectivity, inspecting HTTP responses, and accessing remote Linux hosts.

Command	Purpose	Example
ping	Test basic network reachability	ping -c 4 example.com
curl	Send HTTP requests or retrieve URLs	curl -I https://example.com
ssh	Connect to a remote server	ssh deploy@server.example.com
ss	Inspect sockets and listening ports	ss -tulpn
ip	Inspect network interfaces and routes	ip addr
Common options
curl -sS https://example.com
ssh -p 2222 deploy@192.0.2.10
ip route
ss -ltn

curl -I — request HTTP headers.
ssh -p — connect using a specified port.
ss -ltn — display listening TCP sockets.
ip route — show the system's routing table.

Troubleshooting tip: A failed ping does not always mean a host is unavailable; firewalls and network policies may block ICMP traffic.

💾 Storage and RAM Monitoring

Monitor disk capacity, directory sizes, and available memory before resource shortages affect a running service.

Command	Purpose	Example
df	Check filesystem capacity	df -h
du	Measure directory or file usage	du -sh /var/log
free	View RAM and swap statistics	free -h
lsblk	List block devices	lsblk
Quick resource checks
df -h
du -sh /var/log/*
free -h
lsblk -f


Practical scenario: If a server runs out of disk space, use df -h to identify the affected filesystem and du to investigate which directories consume the most space.

🛠️ System Services and Logs

On Linux distributions using systemd, systemctl controls services while journalctl helps investigate service and system events.

Command	Purpose	Example
uname	Display kernel information	uname -a
uptime	Show uptime and load averages	uptime
systemctl status	Inspect a service	systemctl status nginx
systemctl restart	Restart a service	sudo systemctl restart nginx
systemctl enable	Configure a service to start at boot	sudo systemctl enable nginx
journalctl	Read systemd journal entries	journalctl -u nginx
Log investigation workflow
sudo systemctl status nginx
sudo journalctl -u nginx -n 100
sudo journalctl -u nginx -f

-n 100 — display the latest 100 journal entries.
-f — follow incoming log entries.
-u nginx — filter logs for the specified service.

Practical scenario: When a deployment fails, inspect the service status and recent logs before attempting a restart.

📦 Software Installation on Debian/Ubuntu

APT is the standard package-management tool on Debian-based systems. The following examples apply to Debian and Ubuntu; other distributions use different package managers.

Command	Purpose	Example
apt update	Refresh available package metadata	sudo apt update
apt install	Install software	sudo apt install nginx
apt upgrade	Upgrade installed packages	sudo apt upgrade
apt remove	Remove an installed package	sudo apt remove nginx
apt search	Search available packages	apt search nginx
Typical installation sequence
sudo apt update
sudo apt install nginx
sudo systemctl status nginx


Review package changes before confirming upgrades or removals, especially on production servers.

🗜️ Archives and Backups

Archives are commonly used to bundle configuration files, package application artifacts, and transfer backups between systems.

Command	Purpose	Example
tar	Create or extract archives	tar -czf backup.tar.gz app/
gzip	Compress a file	gzip access.log
zip	Create a ZIP archive	zip -r app.zip app/
unzip	Extract a ZIP archive	unzip app.zip
Common archive operations
# Create a compressed archive
tar -czf logs-backup.tar.gz /var/log/myapp/

# List archive contents
tar -tzf logs-backup.tar.gz

# Extract an archive
tar -xzf logs-backup.tar.gz


The tar options commonly used here are -c for creating an archive, -x for extracting, -z for gzip compression, -f for the archive filename, and -t for listing its contents.

📝 Working with Text and Logs

Text-processing commands are essential for inspecting configuration files, searching application errors, and following log output during troubleshooting.

Command	Purpose	Example
cat	Print file contents	cat app.conf
less	Browse large files interactively	less /var/log/syslog
head	Display the beginning of a file	head -n 15 app.log
tail	Display the end of a file	tail -n 50 app.log
grep	Find matching text	grep -i "error" app.log
echo	Print text or variable values	echo "$HOME"
wc	Count lines, words, or bytes	wc -l app.log
Examples for log analysis
# Search without case sensitivity
grep -i "failed" app.log

# Search all log files in a directory
grep -rni "exception" /var/log/myapp/

# Follow newly written log entries
tail -f app.log

# Count matching error messages
grep -ic "error" app.log

Writing output to files
echo "Deployment started" > deploy.log
echo "Deployment completed" >> deploy.log

> replaces the destination file's existing contents.
>> appends output to the end of the file.

Be careful with redirection: using > on an important configuration or log file can erase its previous contents.

✍️ Terminal Editing Tools

Use a terminal editor when you need to update a configuration file directly on a server.

Tool	Best suited for	Example
nano	Straightforward editing	nano app.conf
vim	Modal editing and advanced workflows	vim app.conf

Before modifying production configuration, consider creating a backup and validating the file before restarting the related service.

🧯 Operational Safety Tips

A few habits can prevent common mistakes while administering Linux systems.

Inspect before deleting. Run ls or find to verify which files match your intended target.
Avoid unnecessary force. Commands such as rm -rf can permanently remove large directory trees.
Use least privilege. Run administrative commands with sudo only when required.
Check service logs first. Diagnose the cause of a failure before repeatedly restarting a service.
Protect credentials. Keep SSH private keys and secret configuration files readable only by authorized users.
Verify disk capacity. Check available storage before generating large archives or deploying artifacts.
Understand the environment. Confirm the hostname and target directory before making changes on a production machine.
🚀 Keep Practicing

The best way to learn Linux is to use these commands in a test environment, understand their output, and gradually combine them into troubleshooting workflows.

This handbook is intended to be a practical reference for Linux administration, cloud infrastructure, CI/CD operations, and everyday DevOps work.

Found something useful? Improve the guide with additional examples, distribution-specific commands, or troubleshooting scenarios.

Learn the command. Understand the impact. Automate the workflow.
