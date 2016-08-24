# gossin-backup

This script makes a full backup in `basedest` the first time it is executed on 
a new day. Subsequent backups on the same day are level one incremental 
backups. It keeps all backups from the current day, the first backup made 
from each of the last `days`, `months` and `years`.

The key feature of this script is that all full backups are made using 
hardlinks to unchanged files (file-level deduplication), thereby being 
storage efficient.

Credits to Mike Rubel, http://www.mikerubel.org/computers/rsync_snapshots/
(and everyone who have published similar articles based on Mike Rubels work)

## Dependencies

Requires the packages `crudini` and `rsync`

## Installation

1. Put `gossin-backup` and `gossin-backup.conf` in the same directory (e.g.
`/opt/gossin-backup/`), make sure `gossin-backup` is executable (`chmod +x gossin-backup`)
2. Specify the location of the `rsyncexcludes` file in `gossin-backup.conf`
(default `/opt/gossin-backup/`)
3. Specify the location of `source` (where to copy from, e.g. `/home/`) and
`basedest` in `gossin-backup.conf`
4. Create the directory for `basedest` (where to copy to, e.g. `/backups`), and
make yourself the owner of it (`sudo chown USER:GROUP DIR`) if you will not be
running this as `root` (Note: `basedest` has to be either an empty directory,
or a directory where backups from the same source exists from previous use of
this script)
5. Add a `crontab` entry specifying when you want to run it, e.g. every half hour (quarter past and quarter to):
```bash
15,45 * * * * username /opt/gossin-backup/gossin-backup
```
 
## Limitations

+ This script has only been tested on Ubuntu (12.04, 14.04 and 16.04).
+ (issue) Every run generates "No such file or directory" for month and year
backups before they exist for the first time (I know, I really should fix this) 

## Author

gossin-backup was written by Erik Hjelmås, <erik.hjelmas@ntnu.no>
