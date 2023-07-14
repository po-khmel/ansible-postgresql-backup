usegalaxy-it.postgresql_backup
=========
The role creates cron jobs to do a PostgreSQL dump daily and weekly. The backups will be stored on the backup server. Old backups are cleaned.  

Requirements
------------

Ansible >= 2.7  
PostgreSQL >= 12  
 
2 VM's are required. Database server must have:  
- installed PostgreSQL  
- postgres user  
- some database  

Check that IP address for backup VM is defined in hostsvars or other vars files.  
SSH connection between 2 servers must be configured in advance.  

Role Variables
--------------
`backup_role`(required) - specify whether you configure DB or backup. May be set to `database` or `backup`  
`backup_ip` (required) - IP address of a backup machine. May be set to `groups['backup'][0]`  where 'backup' is the group host name of database VM    
`db_name`(required) - name of the database to backup. Default to `galaxy`  
`backup_scripts_path` - path to store backup scripts  
`local_backup_dir` - path to dump postgres locally  
`backup_dir_daily` - directory for daily backups on a backup VM  
`backup_dir_weekly` - directory for weekly backups on a backup VM  
`cleanup_log_path` - path to store cleanup cron job logs  
`backup_log_path` - path to store backup cron jobs logs

Example Playbook
----------------

The role must be executed as root user.  
How to use the role:
```yaml
- hosts: database
  become: true
  vars: 
    backup_role: database
  roles:
     - usegalaxy-it.postgresql_backup

- hosts: backup
  become: true
  vars: 
    backup_role: backup
  roles:
     - usegalaxy-it.postgresql_backup
```
Database Restoration Workflow
----------------
To restore the backup created using the `pg_dump` command, you can follow these steps:

1. Ensure that PostgreSQL is installed and running.
2. Transfer the backup file to the server where PostgreSQL is installed.
3. To restore the backup file, use the `pg_restore` command.
```bash
pg_restore -U <postgresql_user_name> -C -d <database_name> <backup_file>
```
- `-U <postgresql_user_name>`: PostgreSQL user name to connect to the database.  
- `-C`: Creates the database before restoring the backup.  
- `-d <database_name>`: the name of the database to which the backup should be restored. Use the same name for the database as was used for the dump.  
- `<backup_file>`: the path and name of the backup file to be restored.

You can verify the restoration by connecting to the PostgreSQL database and querying the data.

License
-------

BSD

Author Information
------------------

Polina Khmelevskaia
