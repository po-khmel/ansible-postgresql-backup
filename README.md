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

License
-------

BSD

Author Information
------------------

Polina Khmelevskaia
