# Description

This role is intended for syncing folders with S3 buckets.

# Installation

In your `requirements.yml` file:
```yaml
- name: infra-role-s3cmd-sync
  src: git@github.com:status-im/infra-role-s3cmd-sync.git
  scm: git
```

# Usage

Should be included by another role and ran with necessary variables:
```yaml
- name: Configure Ghost backups
  include_role:
    name: infra-role-s3cmd-sync
  vars:
    s3cmd_sync_name: 'my-app-logs'
    s3cmd_sync_bucket_name: 'logs-backup'
    s3cmd_sync_bucket_path: '/subdir/'
    s3cmd_sync_directory: '/var/log/my-app'
    s3cmd_sync_timer_frequency: 'daily'
    s3cmd_sync_include_regexes: ['.*\.gz$']
    s3cmd_sync_exclude_regexes: ['.*\.log$']
    s3cmd_sync_access_key: 'ACCESS_KEY'
    s3cmd_sync_secret_key: 'SECRET_KEY'
```

# Administration

You can manage the Systemd service and timer using `systemctl`:
```
 > sudo systemctl status -cat sync-my-app-logs.service
● sync-my-app-logs.service - Sync /data/my-app/logs/. to s3://logs-backup/
     Loaded: loaded (/etc/systemd/system/sync-my-app-logs.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Thu 2022-01-27 18:53:09 UTC; 57s ago
TriggeredBy: ● sync-my-app-logs.timer
       Docs: https://github.com/status-im/infra-role-systemd-timer
    Process: 230052 ExecStart=/usr/bin/s3cmd sync --stats /data/my-app/logs/. s3://logs-backup/subdir/ (code=exited, status=0/SUCCES>
   Main PID: 230052 (code=exited, status=0/SUCCESS)

Starting Sync /data/my-app/logs/. to s3://logs-backup/...
upload: '/data/my_app/logs/./20220127_1800.log.gz' -> 's3://logs-backup/subdir/20220127_1800.log.gz' (3670016 bytes in 0.3 seconds, 10.97 MB/s) [1 of 1]
Done. Uploaded 3670016 bytes in 1.0 seconds, 3.50 MB/s.
Stats: Number of files: 29 (327578277 bytes)
Stats: Number of files transferred: 1 (3670016 bytes)
sync-my_app-logs.service: Succeeded.
Finished Sync /data/my-app/logs/. to s3://logs-backup/.
```
