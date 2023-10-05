**This command displays the last 100 log entries without paging using journalctl**
```bash
journalctl -n 100 --no-pager
```
Useful flags:
- `--unit=my.service`: Show logs for a specific systemd unit
- `-p`: Filter logs by priority (0-7, where 0 is emergency and 7 is debug).
- `-k`: Display kernel messages.
- `--since` and `--until`: Show logs within a specific time range.
- `--reverse`: Display logs in reverse order (oldest first).
- `-f`: Continuously show new log entries as they are added.
- `--user-unit`: Show logs for user units.
- `--disk-usage`: Display disk usage statistics of the journal.
**Kill process based name**
```bash
pkill -f "{name, arguments or both}"
```
**Create the compressed disk image**
```bash
dd if=/dev/sdX | pv -s $(sudo blockdev --getsize64 /dev/sdX) | gzip -9 > filename.img.gz
```
To recover this in future, run the following command:
```bash
zcat filename.img.gz | pv -s $(sudo blockdev --getsize64 /dev/sdX) | dd of=/dev/sdX
```
**Check hash**
```bash
echo {hash} file.txt | shaXsum -c
```
**Find words in multiple files and directories with grep**
```bash
grep -rin 'word' /path/to/files
```
**Replaces 'foo' with 'bar' in the last executed command.**
```perl
!!:s/foo/bar/
```
**Compress with tar**
```bash
tar -czvf files.tar.gz files
```
**Uncompressed with tar**
```bash
tar -xzvf files.tar.gz
```
