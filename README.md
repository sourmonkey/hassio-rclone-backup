# Rclone Backup
Backup your Home Assistant configuration or backups to over 40 cloud providers using [Rclone](https://rclone.org/).

This pairs well with the custom integration [Auto Backup](https://github.com/jcwillox/hass-auto-backup) which provides a highly configurable way to create backups and have them deleted after a given period.

Rclone Backup can sync specific backups, e.g. backups starting with `AutoBackup*` to a cloud provider, and when that backup is deleted from Home Assistant it will be removed from the cloud provider as well.

You can also directly sync your Home Configuration e.g. `/config`, `/share`, `/ssl`, `/media` to a cloud service or to another machine using SFTP. Rclone is smart and will only upload changed files.

## Installation

[![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fjcwillox%2Fhassio-rclone-backup)

Add the repository URL under **Supervisor** → **Add-on store** → **⋮** → **Manage add-on repositories**

```
https://github.com/jcwillox/hassio-rclone-backup
```

## Example

Rclone is a powerful tool, you could for example use the `crypt` and `googledrive` remotes to automatically encrypt your backups and upload them to google drive.

**`rclone.conf`**

```ini
[google]
type = drive
scope = drive
token = REDACTED
; immediately delete backups instead of sending them to the trash
use_trash = false

[hassbackup]
type = crypt
remote = google:Backup/Home Assistant
filename_encryption = off
directory_name_encryption = false
password = REDACTED
password2 = REDACTED
```

**Addon configuration**

```yaml
schedule: 10 4 * * *
command: sync
sources:
  - /backup
destination: 'hassbackup:'
include:
  - DailyBackup*
# we can also disable google drive trash using flags
flags: 
  - --drive-use-trash=false
```
