ansible-maint-restore-mysql-es
==============================

This ansible role can be used to restore the mysql databases and Elasticsearch indices that were backed up with the companion role `ansible-maint-backup-mysql-es`. Please note that the current MySQL databases and Elasticsearch indices on the target server will be completely replaced by the backup contents (ensure that this is what you want to do before proceeding)

- Ensure that the target host has the backups in the directory `{{ maint_backup_basepath }}/current/` (default value: `/var/artefactual/maint-backups/current`). The backups should have been generated by the companion role `ansible-maint-backup-mysql-es` (if the backups were generated in a different host just copy the contents of the `{{ maint_backup_basepath }}/current/` directory from there)
- MySQL databases with same names as the backed up ones are deleted (dropped) from the server before restoring from the backups
- To restore the Elasticsearch indices, a snapshot repository is configured in the Elasticsearch server, the backed up repository tarball file (.tgz) is copied to the repository, and then a snapshot restore operation is done


Role Variables
--------------

Refer to [defaults/main.yml](defaults/main.yml).


Example Playbook
----------------

Backup target host (using the companion role `ansible-maint-backup-mysql-es`)

```yaml
- name: "Take backups of mysql/ES"
  hosts:
    - targethost
  become: yes
  tasks:
    - import_role: 
        name: "maint-backup-mysql-es"
      tags: maint-backup-mysql-es
```


Restore target from backup (using this role):

```yaml
- name: "Restore mysql/ES from backups"
  hosts:
    - targethost
  become: yes
  tasks:
    - import_role: 
        name: "maint-restore-mysql-es"
      tags: maint-restore-mysql-es
```

If you need to restore only the MySQL databases (or only the Elasticsearch indices), use tags when running the playbook (`maint-restore-mysql` or `maint-restore-es`)

License
-------

BSD

Author Information
------------------

Artefactual Systems