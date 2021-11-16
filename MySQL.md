# ALTER

## ADD

```mysql
# 给已经存在的表并且非空 增加一个自增ID
ALTER TABLE tbl_name ADD id INT PRIMARY KEY AUTO_INCREMENT;
```

# pt-online-schema-change



# mysqld_multi

```
[root@localhost ~]# mysqld_multi --example
# This is an example of a my.cnf file for mysqld_multi.
# Usually this file is located in home dir ~/.my.cnf or /etc/my.cnf
#
# SOME IMPORTANT NOTES FOLLOW:
#
# 1.COMMON USER
#
#   Make sure that the MySQL user, who is stopping the mysqld services, has
#   the same password to all MySQL servers being accessed by mysqld_multi.
#   This user needs to have the 'Shutdown_priv' -privilege, but for security
#   reasons should have no other privileges. It is advised that you create a
#   common 'multi_admin' user for all MySQL servers being controlled by
#   mysqld_multi. Here is an example how to do it:
#
#   GRANT SHUTDOWN ON *.* TO multi_admin@localhost IDENTIFIED BY 'password'
#
#   You will need to apply the above to all MySQL servers that are being
#   controlled by mysqld_multi. 'multi_admin' will shutdown the servers
#   using 'mysqladmin' -binary, when 'mysqld_multi stop' is being called.
#
# 2.PID-FILE
#
#   If you are using mysqld_safe to start mysqld, make sure that every
#   MySQL server has a separate pid-file. In order to use mysqld_safe
#   via mysqld_multi, you need to use two options:
#
#   mysqld=/path/to/mysqld_safe
#   ledir=/path/to/mysqld-binary/
#
#   ledir (library executable directory), is an option that only mysqld_safe
#   accepts, so you will get an error if you try to pass it to mysqld directly.
#   For this reason you might want to use the above options within [mysqld#]
#   group directly.
#
# 3.DATA DIRECTORY
#
#   It is NOT advised to run many MySQL servers within the same data directory.
#   You can do so, but please make sure to understand and deal with the
#   underlying caveats. In short they are:
#   - Speed penalty
#   - Risk of table/data corruption
#   - Data synchronising problems between the running servers
#   - Heavily media (disk) bound
#   - Relies on the system (external) file locking
#   - Is not applicable with all table types. (Such as InnoDB)
#     Trying so will end up with undesirable results.
#
# 4.TCP/IP Port
#
#   Every server requires one and it must be unique.
#
# 5.[mysqld#] Groups
#
#   In the example below the first and the fifth mysqld group was
#   intentionally left out. You may have 'gaps' in the config file. This
#   gives you more flexibility.
#
# 6.MySQL Server User
#
#   You can pass the user=... option inside [mysqld#] groups. This
#   can be very handy in some cases, but then you need to run mysqld_multi
#   as UNIX root.
#
# 7.A Start-up Manage Script for mysqld_multi
#
#   In the recent MySQL distributions you can find a file called
#   mysqld_multi.server.sh. It is a wrapper for mysqld_multi. This can
#   be used to start and stop multiple servers during boot and shutdown.
#
#   You can place the file in /etc/init.d/mysqld_multi.server.sh and
#   make the needed symbolic links to it from various run levels
#   (as per Linux/Unix standard). You may even replace the
#   /etc/init.d/mysql.server script with it.
#
#   Before using, you must create a my.cnf file either in /www/server/mysql/my.cnf
#   or /root/.my.cnf and add the [mysqld_multi] and [mysqld#] groups.
#
#   The script can be found from support-files/mysqld_multi.server.sh
#   in MySQL distribution. (Verify the script before using)
#

[mysqld_multi]
mysqld     = /www/server/mysql/bin/mysqld_safe
mysqladmin = /www/server/mysql/bin/mysqladmin
user       = multi_admin
password   = my_password

[mysqld2]
socket     = /tmp/mysql.sock2
port       = 3307
pid-file   = /www/server/mysql/data2/hostname.pid2
datadir    = /www/server/mysql/data2
language   = /www/server/mysql/share/mysql/english
user       = unix_user1

[mysqld3]
mysqld     = /path/to/mysqld_safe
ledir      = /path/to/mysqld-binary/
mysqladmin = /path/to/mysqladmin
socket     = /tmp/mysql.sock3
port       = 3308
pid-file   = /www/server/mysql/data3/hostname.pid3
datadir    = /www/server/mysql/data3
language   = /www/server/mysql/share/mysql/swedish
user       = unix_user2

[mysqld4]
socket     = /tmp/mysql.sock4
port       = 3309
pid-file   = /www/server/mysql/data4/hostname.pid4
datadir    = /www/server/mysql/data4
language   = /www/server/mysql/share/mysql/estonia
user       = unix_user3

[mysqld6]
socket     = /tmp/mysql.sock6
port       = 3311
pid-file   = /www/server/mysql/data6/hostname.pid6
datadir    = /www/server/mysql/data6
language   = /www/server/mysql/share/mysql/japanese
user       = unix_user4

(base) [root@localhost ~]# mysqld_multi
mysqld_multi version 2.16 by Jani Tolonen

Description:
mysqld_multi can be used to start, reload, or stop any number of separate
mysqld processes running in different TCP/IP ports and UNIX sockets.

mysqld_multi can read group [mysqld_multi] from my.cnf file. You may
want to put options mysqld=... and mysqladmin=... there.  Since
version 2.10 these options can also be given under groups [mysqld#],
which gives more control over different versions.  One can have the
default mysqld and mysqladmin under group [mysqld_multi], but this is
not mandatory. Please note that if mysqld or mysqladmin is missing
from both [mysqld_multi] and [mysqld#], a group that is tried to be
used, mysqld_multi will abort with an error.

mysqld_multi will search for groups named [mysqld#] from my.cnf (or
the given --defaults-extra-file=...), where '#' can be any positive
integer starting from 1. These groups should be the same as the regular
[mysqld] group, but with those port, socket and any other options
that are to be used with each separate mysqld process. The number
in the group name has another function; it can be used for starting,
reloading, stopping, or reporting any specific mysqld server.

Usage: mysqld_multi [OPTIONS] {start|reload|stop|report} [GNR,GNR,GNR...]
or     mysqld_multi [OPTIONS] {start|reload|stop|report} [GNR-GNR,GNR,GNR-GNR,...]

The GNR means the group number. You can start, reload, stop or report any GNR,
or several of them at the same time. (See --example) The GNRs list can
be comma separated or a dash combined. The latter means that all the
GNRs between GNR1-GNR2 will be affected. Without GNR argument all the
groups found will either be started, reloaded, stopped, or reported. Note that
syntax for specifying GNRs must appear without spaces.

Options:

These options must be given before any others:
--no-defaults      Do not read any defaults file
--defaults-file=...  Read only this configuration file, do not read the
                   standard system-wide and user-specific files
--defaults-extra-file=...  Read this configuration file in addition to the
                   standard system-wide and user-specific files
Using:

--example          Give an example of a config file with extra information.
--help             Print this help and exit.
--log=...          Log file. Full path to and the name for the log file. NOTE:
                   If the file exists, everything will be appended.
                   Using: /www/server/data/mysqld_multi.log
--mysqladmin=...   mysqladmin binary to be used for a server shutdown.
                   Since version 2.10 this can be given within groups [mysqld#]
                   Using:
--mysqld=...       mysqld binary to be used. Note that you can give mysqld_safe
                   to this option also. The options are passed to mysqld. Just
                   make sure you have mysqld in your PATH or fix mysqld_safe.
                   Using:
                   Please note: Since mysqld_multi version 2.3 you can also
                   give this option inside groups [mysqld#] in ~/.my.cnf,
                   where '#' stands for an integer (number) of the group in
                   question. This will be recognised as a special option and
                   will not be passed to the mysqld. This will allow one to
                   start different mysqld versions with mysqld_multi.
--no-log           Print to stdout instead of the log file. By default the log
                   file is turned on.
--password=...     Password for mysqladmin user.
--silent           Disable warnings.
--tcp-ip           Connect to the MySQL server(s) via the TCP/IP port instead
                   of the UNIX socket. This affects stopping and reporting.
                   If a socket file is missing, the server may still be
                   running, but can be accessed only via the TCP/IP port.
                   By default connecting is done via the UNIX socket.
--user=...         mysqladmin user. Using: multi_admin
--verbose          Be more verbose.
--version          Print the version number and exit.
```

