# Laravel Queue Manager

Laravel Queues are great for dealing multiple time consuming processes. These job queues are processed by the queue worker. And Supervisor is a process monitor which handles the automatically starting the queue worker.

A queue worker processes the queued jobs and runs the tasks associated with them just by a simple artisan command.

> php artisan queue:work

> php artisan queue:work --queue=default,queue1,queue2

To start the queue worker, run the above artisan command. It will keep on running until it is stopped manually or the terminal is closed. After it is stopped you have to run it again.

It is a long-lived process and stores the booted application state in memory so after any changes to your code, you have to restart the worker. To automatically start the queue worker again, you have to install a process manager like Supervisor to manage queue workers.

---

## **Supervisor**

**The supervisor** is a process manager which Laravel suggests to use as a process monitor for queue workers. It will automatically start the queue worker in the background, even after the system has booted and will automatically restart the worker if the worker exits unexpectedly.

---

### Installing Supervisor on Linux

To install supervisor, use the following command.
1. Python pip
    > sudo pip install supervisor
2. Ubuntu  
    > sudo apt-get install supervisor
3. CentOS
    > yum install -y supervisor

### Supervisor configuration for Laravel

After installation, we have to create a configuration file for Laravel. Create a laravel-queue-worker.conf file in /etc/supervisor/conf.d directory. Add the following configuration lines.

> [program:laravel-queue-worker]
> 
> process_name=%(program_name)s_%(process_num)02d
> 
> command=php /home/path/to/yoursite.com/artisan queue:work --tries=3
> 
> autostart=true
> 
> autorestart=true
> 
> user=username
> 
> numprocs=3
> 
> redirect_stderr=true
> 
> stdout_logfile=/home/path/to/yoursite.com /queue-worker.log

### Start Supervisor

Run supervisord -c /etc/supervisord.conf. If it is started, use ps -ef to check PID and kill to finish the task.
1. Listen tasks and keep the queue running

    Run the following commends one by one.

    > sudo supervisorctl reread -- Restart all prgrams in configuration files
    >
    > sudo supervisorctl update -- Update configurations to supervisord
    >
    > sudo supervisorctl start laravel-worker:* -- Start a prgram (program_name=the program name you configured)

[//]: <> (  )

[//] *** Note: In case of laravel-worker: ERROR (no such group), check whether the path is /etc/supervisor/laravel-worker.conf and whether the admin account is user=root.

2. Check the status of Supervisor

    Run supervisorctl status to check the status.

3. Test whether it works



