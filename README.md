# spatie_back_up_for_disi_documentation
Project Backup Schedule For DISI Using Spatie Documentation

```
composer require spatie/laravel-backup
```
To publish the config file to config/backup.php run:
```
php artisan vendor:publish --provider="Spatie\Backup\BackupServiceProvider"
```
# Scheduling
After you have performed the basic installation you can start using the backup:run, backup:clean, backup:list and backup:monitor-commands. In most cases you'll want to schedule these commands so you don't have to manually run backup:run everytime you need a new backup.

The commands can be scheduled in Laravel's console kernel, just like any other command.

// app/Console/Kernel.php

```
protected function schedule(Schedule $schedule)
{
   $schedule->command('backup:clean')->daily()->at('01:00');
   $schedule->command('backup:run')->daily()->at('01:30');
}
```
# Schedue config using Supervisor
Create a supervisor config for schedule worker
```
sudo  nano /etc/supervisor/conf.d/schedule-worker.conf
```
in this schedule-worker.conf add these lines
```
process_name = %(program_name)s_%(process_num)02d
directory=/var/www/html/DISI/backend/
command=php /var/www/html/DISI/backend/artisan schedule:work
autostart=true
autorestart=true
user=disi
numprocs=1
redirect_stderr=true
stdout_logfile= /var/www/html/work-logs/schedule-worker.log
```
update and reload the supervisor config
```
sudo service supervisor reload
```
and restart the apache server
```
sudo service apache2 restart
```

