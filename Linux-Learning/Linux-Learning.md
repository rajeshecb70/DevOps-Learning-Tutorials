# Prerequisites

- Basic understanding of computers and networking concepts (IP, DNS basics).

- Comfortable with using terminal (text editors like vim/nano) and copying/pasting commands.

- Access to virtualization (VirtualBox, VMWare) or cloud VMs (AWS, GCP, Azure).

- A Linux distribution that aligns with Red Hat (CentOS Stream, Rocky Linux, AlmaLinux) for lab practice.

# Environment setup
- Minimum: 2 VMs (one small for RHCSA tasks, one for RHCE more advanced services).

- VM specs: 2 vCPUs, 4 GB RAM, 20–40 GB disk (increase for DBs).

- Snapshot before major changes.

- Enable password-less sudo for your lab user for convenience (but practice with root too).

# RHCSA Modules & Labs

RHCSA focuses on core sysadmin tasks. Each module includes topics, commands to practice, and lab exercises.

# Module 1 — Linux Fundamentals & Shell

## Topics

- Filesystem hierarchy (/, /etc, /var, /home, /usr, /opt, /tmp)

- Basic commands: ls, cd, pwd, cp, mv, rm, mkdir, rmdir, date, cal, rm

- File viewing/editing: cat, less, head, tail, nano, vim

- Wildcards, pipes, redirection: >, >>, |, &, 2>

- File permissions and ownership: user/group/other, chmod, chown, chgrp

### Practice Commands
```
ls -la /etc
cat /etc/os-release
chmod 640 /tmp/testfile
chown root:wheel /tmp/testfile
```

### Labs

- Create a directory tree in /srv/myapp and set proper ownership & permissions.

- Create text files, redirect outputs to files, append logs.

# Module 2 — Users, Groups, and Authentication

## Topics

- Add/remove users and groups: useradd, userdel, usermod, groupadd

- Password management: passwd

- Sudo configuration (/etc/sudoers, visudo)

- Home directories, skeleton files (/etc/skel)

## Practice Commands
```
sudo useradd -m -s /bin/bash mary
sudo passwd mary
sudo usermod -aG wheel mary
sudo visudo
```

## Labs

- Create 3 users with different shells and groups, allow one user sudo access via /etc/sudoers.d/.

# Module 3 — Filesystems & Storage

## Topics

- Partitioning and formatting: fdisk, parted, mkfs.xfs (or ext4)

- Mounting: mount, umount, /etc/fstab

- LVM basics: pvcreate, vgcreate, lvcreate, lvextend, lvremove

- Disk usage: df -h, du -sh

- Swap management

## Practice Commands

```
lsblk
sudo fdisk /dev/xvdb
sudo mkfs.xfs /dev/xvdb1
sudo mount /dev/xvdb1 /mnt/data
```
## Labs

- Create an LVM volume group and logical volume, mount it, add an entry to /etc/fstab, resize the LV.

# Module 4 — SELinux & Firewalls

## Topics

- SELinux modes: Enforcing, Permissive, Disabled

- Tools: sestatus, setenforce, semanage, restorecon, audit2why

- Firewalld basics: zones, services, ports, firewall-cmd, permanent vs runtime

## Practice Commands
```
sestatus
getenforce
sudo setenforce 0
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

## Labs

- Serve a simple HTTP page, configure SELinux boolean and firewall to allow HTTP, then revert.

# Module 5 — Networking Basics

## Topics

- View and configure network interfaces: ip addr, ip route

- Hostname, DNS configuration (/etc/hosts, /etc/resolv.conf)

- Network troubleshooting: ping, traceroute, ss, netstat (if installed)

- NetworkManager CLI: nmcli, nmtui basics

## Practice Commands
```
ip addr show
ip route show
nmcli connection show
```
## Labs

- Assign static IP to an interface, add DNS server, test connectivity to external site.

# Module 6 — Process & Service Management

## Topics

- Systemd basics: systemctl start/stop/restart/status, enable, disable

- Inspect logs: journalctl -u servicename, journalctl -b

- Process management: ps aux, top, htop, kill, nice, renice

## Practice Commands
```
sudo systemctl status sshd
sudo systemctl enable httpd
journalctl -u httpd --since "1 hour ago"
```
## Labs

- Create a systemd service unit for a small script, enable and test restart behavior.

# Module 7 — Packages & Repositories

## Topics

- RPM and DNF/YUM: dnf install, dnf remove, rpm -qa, repositories /etc/yum.repos.d/

- Managing modules and packages

## Practice Commands
```
sudo dnf install -y httpd
rpm -qa | grep httpd
sudo dnf update -y
```

## Labs

- Create a local repo (createrepo) or point to a custom repo, install a package from it.

# Module 8 — Basic Shell Scripting & Automation

## Topics

- Bash scripting basics: variables, loops, conditionals, functions

- Cron jobs and timers (crontab -e, systemd timers)

- File manipulation in scripts

## Practice Commands
```
cat > /usr/local/bin/hello.sh <<'EOF'
#!/bin/bash
echo "Hello, $(whoami)"
EOF
sudo chmod +x /usr/local/bin/hello.sh
crontab -l
```

## Labs

- Write a script that rotates logs older than 7 days; schedule it via cron.

# Module 9 — Basic Storage Services & Backups

## Topics

- NFS basics (server/client), exportfs

- rsync for backups

- tar and gzip

## Practice Commands
```
sudo dnf install nfs-utils
sudo systemctl enable --now nfs-server
rsync -av /var/www/ backup:/srv/backup/
```
## Labs

- Configure NFS share and mount it from another VM; perform a backup with rsync.


# Module 10 — Advanced Networking & Services

## Topics

- Network bonding, bridges, VLANs

- Configure and troubleshoot advanced network services

- Nginx/Apache advanced config, virtual hosts, reverse proxy

## Labs

- Configure a reverse proxy in Nginx to forward to a backend app.

# Module 11 — System Security & Hardening

## Topics

- Advanced SELinux contexts, booleans, troubleshooting AVC denials

- Auditing: auditd, ausearch, aureport

- SSH hardening (key-based auth, disabling root login, Fail2ban basics)

## Labs

- Create a custom SELinux policy for a simple application; deploy and test.

# Module 12 — Identity Management & Authentication

## Topics

- LDAP basics, SSSD integration (conceptual for RHCE)

- Kerberos basics (conceptual)

- Centralized authentication concepts

## Labs

- Configure SSSD to authenticate against a test LDAP (optional lab depending on environment).

# Module 13 — Automation with Ansible

## Topics

- Ansible basics: inventory, playbooks, ad-hoc commands, modules

- Idempotency and roles

- Using Ansible to automate repetitive RHCE tasks

## Practice Commands
```
ansible all -m ping -i inventory
ansible-playbook -i inventory site.yml
```
## Labs

- Write an Ansible playbook to deploy an HTTP server, create users, and push a config file.

# Module 14 — Containers , Docker & Podman

## Topics

- Containers concept, docker, podman basics (build, run, images)

- Basic container networking and volumes

## Labs

- Build a simple container image for a web app and run it with podman.

# Module 15 — Advanced Storage & High Availability (optional)

## Topics

- DRBD, GlusterFS (conceptual), multipath (conceptual)

- Basic HA service patterns

## Labs

- Practice creating and mounting network storage, simulate failover scenarios (conceptual).

# Module 16 — Troubleshooting & Performance Tuning

## Topics

- Use strace, lsof, perf basics, iotop, iostat

- Kernel tuning via sysctl

- Logs correlation and root cause analysis

## Labs

- Simulate high CPU or I/O load and analyze cause with top, iotop.

# Additional 
## Common Commands & Quick Cheatsheet

- Files: ls -la, cp -r, mv, rm -rf

- Permissions: chmod, chown, chgrp, stat

- Services: systemctl status|start|stop|restart|enable|disable

- Network: ip a, ip r, ss -tulpn, nmcli

- Disk: lsblk, df -h, du -sh /path

- Logs: journalctl -xe, tail -f /var/log/messages

- Package: dnf install, dnf remove, rpm -qa

- SELinux: sestatus, setenforce, semanage fcontext -a -t httpd_sys_content_t '/var/www(/.*)?', restorecon -Rv /var/www

- Users: useradd, passwd, usermod -aG

# Further Study & Resources

- Official Red Hat documentation (use for authoritative guidance).

- Books and interactive labs for RHCSA/RHCE.

- Practice on community OSes compatible with Red Hat (Rocky Linux, AlmaLinux, CentOS Stream, Ubuntu).

- Online lab platforms offering hands-on scenarios (search for "Red Hat labs" or "Linux hands-on labs").

