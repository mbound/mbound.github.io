# Fixing Upgrade failure on Ubiquiti EdgeRouter X (ER-X)

Ubiquiti stuff is generally cool, but sometimes they make significant design fuckups.
This is one of such cases.


A typical scenario is that the ER-X root partition doesn't have enough space. In this case, just about `64.2M`. 
```
ubnt@EdgeRouter-X-5-Port:~$ df -h
Filesystem                Size      Used Available Use% Mounted on
ubi0_0                  214.9M    146.0M     64.2M  69% /root.dev
overlay                 214.9M    146.0M     64.2M  69% /
devtmpfs                123.0M         0    123.0M   0% /dev
tmpfs                   123.6M         0    123.6M   0% /dev/shm
tmpfs                   123.6M      1.8M    121.9M   1% /run
tmpfs                     5.0M         0      5.0M   0% /run/lock
tmpfs                   123.6M         0    123.6M   0% /sys/fs/cgroup
tmpfs                   123.6M         0    123.6M   0% /lib/init/rw
tmpfs                   123.6M      4.0K    123.6M   0% /tmp
tmpfs                   123.6M         0    123.6M   0% /run/shm
tmpfs                   123.6M     92.0K    123.5M   0% /var/log
none                    123.6M    104.0K    123.5M   0% /opt/vyatta/config
overlay                 123.6M      4.0K    123.6M   0% /opt/vyatta/config/tmp/new_config_3278da8d79cf4be99a49c317a079f833
tmpfs                    24.7M         0     24.7M   0% /run/user/1000

```

This is not enough to host the ER-X upgrade image, which happens to be `71M`:
```
71M	ER-e50.v2.0.9-hotfix.7.5622731.tar
```
Upgrade from GUI will fail at upload time, and attempting a manual upgrade via CLI with `add system image` or `scp`'ing the image to the router's home directory will both fail.

The solution is to `scp` the upgrade image to `/tmp` ramfs (which is larger) and then perform the system upgrade from there.
```
$ scp ER-e50.v2.0.9-hotfix.7.5622731.tar ubnt@192.168.1.1:/tmp/
```

From here, the upgrade should succeed:
```
ubnt@EdgeRouter-X-5-Port:~$ add system image /tmp/ER-e50.v2.0.9-hotfix.7.5622731.tar 
Checking upgrade image...Done
Preparing to upgrade...Done
Copying upgrade image...Done
Removing old image...Done
Checking upgrade image...Done
Copying config data...Done
Finishing upgrade...Done
Upgrade completed

```
Lastly, reboot.
