# Systemd timers

This post is to reflect on systemd timers, replacing cron jobs, monitoring, useful references.

### Cron jobs
Cron jobs is easy to understand scheduler standard unix scheduler. Common commands are:
- `crontab -e` edit jobs
- `crontab -l` list jobs
Crontab can be defined in the following locations:
- `/etc/cron.d`
- `/etc/cron.hourly`, `daily`, `monthy` etc.
```
cat /etc/cron.d/e2scrub_all 
30 3 * * 0 root test -e /run/systemd/system || SERVICE_MODE=1 /usr/lib/arm-linux-gnueabihf/e2fsprogs/e2scrub_all_cron
10 3 * * * root test -e /run/systemd/system || SERVICE_MODE=1 /sbin/e2scrub_all -A -r
```
Useful site to test schedules: 
- https://crontab.guru/
- https://www.baeldung.com/cron-expressions
- `man 5 crontab`

### Systemd Timers

Systemd timers is a great replacement of cron jobs, however a tad more complicated.
#### Timer types
- Monotonic - periodic timers without specific time
- Realtime - Start periodically at explicit time

One-off transient timer can be activated as:
- `systemd-run --on-active=30 /bin/touch /tmp/foo`
- `systemd-run --on-active="12h 30m" --unit someunit.service` - to run specific unit

Timers live in `/var/lib/systemd/timers`
Timers are designed to work with services. For each .timer file, a matching .service file exists. For example: 
#### Definition
```
cat ./system/timers.target.wants/test-minute.timer
[Unit]
Description=Test timer: every minute

[Timer]
OnCalendar=*-*-* *:*:00
Persistent=true


[Install]
WantedBy=timers.target
```

#### Service
```
cat /lib/systemd/system/test-minute.service
[Unit]
Description=test minute timer %a %H

[Service]
Type=oneshot
ExecStart=/bin/true
#User=nobody
Group=systemd-journal
```
#### Timer Control
- `systemctl list-timers --all` - list all timers
- `systemctl list-timers` - list active timers
- `systemctl enable test-minute.timer` - enable timer to run including after reboot
- If not activated, timer is shown as dead
```
# systemctl status test-minute.timer
● test-minute.timer - Test timer: every minute
     Loaded: loaded (/lib/systemd/system/test-minute.timer; enabled; vendor preset: enabled)
     Active: inactive (dead)
    Trigger: n/a
   Triggers: ● test-minute.service
```
- start timer: `# systemctl start test-minute.timer`
- timer is active now
```
# systemctl status test-minute.timer
● test-minute.timer - Test timer: every minute
     Loaded: loaded (/lib/systemd/system/test-minute.timer; enabled; vendor preset: enabled)
     Active: active (waiting) since Sun 2023-02-19 13:28:47 PST; 5s ago
    Trigger: Sun 2023-02-19 13:29:00 PST; 6s left
   Triggers: ● test-minute.service
   Feb 19 13:28:47 <host> systemd[1]: Started Test timer: every minute.
```
- `systemctl daemon-reload` - reload systemd to refresh recently added timers

#### Monitoring
```
journalctl -r |grep  test-minute|head
Feb 19 13:32:01 <host> systemd[1]: test-minute.service: Succeeded.
Feb 19 13:31:01 <host> systemd[1]: test-minute.service: Succeeded.
```
#### Nits
- Systemd specifiers is a great set of variables that can be used in configuring timers and including into log messages. 
E.g. the following timer definition results in the logging below:
```
Description=test minute timer %a %H
```
```
Feb 19 13:34:01 <host> systemd[1]: Finished test minute timer arm <host>.
Feb 19 13:34:01 <host> systemd[1]: test-minute.service: Succeeded.
Feb 19 13:34:01 <host> systemd[1]: Starting test minute timer arm <host>...
```
- Explaining timer schedule: `systemd-analyze calendar "*-*-* *:*:00"`

## References
These are great resources about systemd timers:
- [Archlinux Timers](https://wiki.archlinux.org/title/systemd/Timers)
- [Systemd Specifiers](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Specifiers)
