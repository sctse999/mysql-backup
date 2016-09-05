# mysql-backup

This image runs mysqldump to backup data using cronjob to folder `/backup`. This fork use the container environment variables of root password MYSQL_ENV_MYSQL_ROOT_PASSWORD.

## Usage:

    docker run --rm -d \
        --env MYSQL_DB=dbname
        --env CRON_TIME='0 * * * *'
        --env MAX_BACKUPS=7
        --link db.myapp:mysql
        --volume host.folder:/backup
        proactivehk/mysql-backup

Moreover, if you link `proactivehk/mysql-backup` to the official mysql container with an alias named mysql, this image will try to auto load the `host`, `port`, default mysql root password if possible.


## Parameters

    MYSQL_HOST      the host/ip of your mysql database
    MYSQL_PORT      the port number of your mysql database
    MYSQL_USER      the username of your mysql database
    MYSQL_PASS      the password of your mysql database
    MYSQL_DB        the database name to dump. Default: `--all-databases`
    EXTRA_OPTS      the extra options to pass to mysqldump command
    CRON_TIME       the interval of cron job to run mysqldump. `0 0 * * *` by default, which is every day at 00:00
    MAX_BACKUPS     the number of backups to keep. When reaching the limit, the old backup will be discarded. No limit by default
    INIT_BACKUP     if set, create a backup when the container starts
    INIT_RESTORE_LATEST if set, restores latest backup

## Restore from a backup

See the list of backups, you can run:

    docker exec backup-container-name ls /backup

To restore database from a certain backup, simply run:

    docker exec backup-container-name /restore.sh /backup/2015.08.06.171901
