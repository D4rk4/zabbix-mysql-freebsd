# zabbix-mysql-freebsd
Zabbix MySQL monitoring plugin (FreeBSD adaptation)

Based on Template MySQL (800+ items) - https://share.zabbix.com/databases/mysql/template-mysql-800-items

Instructions:
You need specify on host level (which will inherit objects from the attached template) some macros:
```shell
{$MYSQL_PWD} - MySQL password.
{$MYSQL_USER} - MySQL user.
{$DATABASE_NAME} - For getting size of the database.
```

Also please look to the script, there is some parameters which can be/must be modified for your installation:

```perl
my $dsn = "DBI:mysql:mysql;host=127.0.0.1";
my $dsn = "DBI:mysql:mysql;mysql_socket=/var/run/mysqld/mysqld.sock";
my $zabbix_sender = '/usr/bin/zabbix_sender';
my $agentd_config = '/etc/zabbix/zabbix_agentd.conf';
```

Also define UserParameter in the zabbix_agent.conf in database side for perl script like this example:

```
UserParameter=mysql[*],/opt/mysql_check.pl $1 $2 $3 $4
```
Valuemappings:
```
MySQL - Status
0 ⇒ No
1 ⇒ Yes
```
For correct working "monitoring" MySQL user must have "SELECT,REPLICATION CLIENT,PROCESS ON grants":
```sql
CREATE USER 'monitoring'@'localhost' IDENTIFIED BY 'P@SSW0RD';
GRANT SELECT,REPLICATION CLIENT,PROCESS ON *.* TO 'monitoring'@'localhost';
```
Also you may need install perl support packages:
```
# pkg install p5-DBD-mysql p5-DBI
```
