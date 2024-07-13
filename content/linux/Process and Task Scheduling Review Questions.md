## Process and Task Scheduling Review Questions 

Q. What are the two commands to list the PID of a specific process? 
A. The `pidof` and `pgrep` commands. 

Q. By default the \*.allow files exist. True or False? 
A. False. By default, the \*.deny files exist. 

Q. Where do the scheduling daemons store log information of executed jobs? 
A. The scheduling daemons store log information of executed jobs in the /var/log/cron file. 

Q. You must restart the crond service after modifying the /etc/crontab file. True or False? 
A. False. The crond daemon does not need a restart after a crontable is modified. 

Q. What are the background service processes normally referred to in Linux? 
A. The background service processes are referred to as daemons. 

Q. What is the default nice value? 
A. The default nice value is zero. 

Q. Which service runs missed scheduled tasks? 
A. The anacron service executes any missed at and cron jobs.

Q. The parent process gets the nice value of its child process. True or False?
A. False.  The child process inherits its parent’s niceness.

Q. When would the cron daemon execute a job that is submitted as */10 * 2-6 6 * /home/user1/script1.sh? 
A. The cron daemon will run the script every tenth minute past the hour on the 2nd, 3rd, 4th, 5th, and 6th day of every sixth month. 

Q. What is the other command besides ps to view running processes? 
A. The top command. 

Q. Every process that runs on the system has a unique identifier called UID. True or False? 
A. False. It is called the PID. 

Q. Why would you use the renice command? 
A. The renice command is used to change the niceness of a running process. 

Q. Which user does not have to be explicitly defined in either *.allow or *.deny file to run the at and cron jobs? 
A. The root user. 

Q. When would the at command execute a job that is submitted as at 01:00 12/12/2020? 
A. The at command will run it at 1am on December 12, 2020. 

Q. What are the two commands that you can use to terminate a process? 
A. The kill and pkill commands. 

Q. What is the directory location where user crontab files are stored? 
A. The user crontab files are stored in the /var/spool/cron directory. 

Q. What would the nice command display without any options or arguments?
A. The nice command displays the default nice value when executed without any options. 

Q. Which command can be used to edit crontables? 
A. You can use the crontab command with the -e option to edit crontables. 

Q. The default location to send application error messages is the system log file. True or False? 
A. False. The default location is the user screen where the program is initiated. 

Q. What are the five process states? 
A. The five process states are running, sleeping, waiting, stopped, and zombie. 

Q. Signal 9 is used for a hard termination of a process. True or False?
A. True.

### Review Questions {.style3}

\

1\. The default location to send application error messages is the
system log file. True or False?

2\. What are the five process states?

3\. Signal 9 is used for a hard termination of a process. True or False?

4\. You must restart the crond service after modifying the /etc/crontab
file. True or False?

5\. What are the background service processes normally referred to in
Linux?

6\. What is the default nice value?

7\. The parent process gets the nice value of its child process. True or
False?

8\. When would the cron daemon execute a job that is submitted as \*/10
\* 2-6 6 \* /home/user1/script1.sh?

9\. What is the other command besides ps to view running processes?

10\. Every process that runs on the system has a unique identifier
called UID. True or False?

11\. Why would you use the renice command?

12\. What are the two commands to list the PID of a specific process?

13\. By default the \*.allow files exist. True or False?

14\. Where do the scheduling daemons store log information of executed
jobs?

15\. Which user does not have to be explicitly defined in either
\*.allow or \*.deny file to run the at and cron jobs?

16\. When would the at command execute a job that is submitted as at
01:00 12/12/2020?

17\. What are the two commands that you can use to terminate a process?

18\. What is the directory location where user crontab files are stored?

19\. What would the nice command display without any options or
arguments?

20\. Which command can be used to edit crontables?

::: {style="page-break-before: always;"}
:::

[]{#chapter0272.html}

### Answers to Review Questions {.style3}

\

1\. False. The default location is the user screen where the program is
initiated.

2\. The five process states are running, sleeping, waiting, stopped, and
zombie.

3\. True.

4\. False. The crond daemon does not need a restart after a crontable is
modified.

5\. The background service processes are referred to as daemons.

6\. The default nice value is zero.

7\. False. The child process inherits its parent's niceness.

8\. The cron daemon will run the script every tenth minute past the hour
on the 2nd, 3rd, 4th, 5th, and 6th day of every sixth month.

9\. The top command.

10\. False. It is called the PID.

11\. The renice command is used to change the niceness of a running
process.

12\. The pidof and pgrep commands.

13\. False. By default, the \*.deny files exist.

14\. The scheduling daemons store log information of executed jobs in
the /var/log/cron file.

15\. The root user.

16\. The at command will run it at 1am on December 12, 2020.

17\. The kill and pkill commands.

18\. The user crontab files are stored in the /var/spool/cron directory.

19\. The nice command displays the default nice value when executed
without any options.

20\. You can use the crontab command with the -e option to edit
crontables.

::: {style="page-break-before: always;"}
:::

[]{#chapter0273.html}