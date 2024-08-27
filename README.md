# Linux_memory-issues


1. Check Disk Usage by Directories

`sudo du -h / | grep -E '^[0-9\.]+G' | sort -hr | head -n 20`

2. Check for Hidden Files and Directories Sometimes hidden files or directories (those starting with a dot .) consume space.

`sudo du -sh /.* 2>/dev/null`

3. Check for Deleted Files Still in Use
Sometimes, files are deleted, but their space is not freed because a process is still using them. You can check for such files using:

`sudo lsof | grep deleted`

4. Clear System Logs
Logs can grow significantly over time. You can clear old logs using:

`sudo journalctl --vacuum-time=2d`

This will remove logs older than 2 days. Adjust the time as needed.

You can also manually remove large log files in /var/log/ if necessary.

`sudo rm -rf /var/log/*.log`

Or truncate a log file:

`sudo truncate -s 0 /var/log/your-log-file.log`

5. Check for Unnecessary Files in /`tmp`
The `/tmp` directory often contains temporary files that can be removed.

`sudo rm -rf /tmp/*`


6. Clear Package Cache
Over time, the package manager cache can consume a lot of space. Clear it with:

`sudo apt-get clean`

You can also check how much space the cache is taking with:

`du -sh /var/cache/apt`

7. Check for Large Core Dump Files
Core dumps can consume a lot of space. To find and remove them:

`sudo find / -name "*.core" -exec rm -f {} \;`

8. Examine Mounted Filesystems

`df -h`

9. Check for Unlinked but Open Files
A file that is deleted but still open by a process will not free up space. You can use lsof to identify such files:

`sudo lsof +L1`

10. Check Inode Usage
Sometimes the issue is not with space but with inodes. Check inode usage with:

`df -i`

11. Look for Large Files
To find large files that might be consuming disk space:

`sudo find / -type f -size +1G -exec ls -lh {} \;`

12. Reboot the System
If you have deleted files that were in use or performed major cleanup operations, a reboot might be necessary to fully reclaim the disk space.


13. Investigate Filesystem Corruption
In rare cases, filesystem corruption might cause space to appear unavailable. Running a filesystem check might help:

`sudo fsck -f /`
