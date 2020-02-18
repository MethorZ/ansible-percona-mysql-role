ansible-percona-mysql-role
=========

Support for percona mysql management for Debian

Requirements
------------

None

Role Variables
--------------
```YAML
##
# Percona MySQL server configuration
##
percona_mysql_enabled: false
percona_mysql_version: 5.7

# Enable jemalloc (and disable transparent huge pages)
percona_mysql_jemalloc_enabled: false

##
# Package configuration
##
percona_mysql_packages_mode: latest

# Install the mysql toolkit
percona_mysql_toolkit_enabled: false

##
# File and directory path configuration
##

# Local directory used for temporary files
percona_mysql_local_temp_dir: /tmp
percona_mysql_root_my_cnf_file: /root/.my.cnf

##
# Default user configuration
##

# Always overwrites existing password when provided
percona_mysql_root_password: ''

# Triggers automatic renewal of root password
percona_mysql_renew_existing_password: false

# Creates a file containing the current root password on server
percona_mysql_create_root_passwd_file: true
percona_mysql_root_passwd_file: /root/.pwd-mysql

# Create a read only user for security purposes
percona_mysql_read_only_user_enabled: true
percona_mysql_read_only_user: root-ro
percona_mysql_read_only_user_priv: "*.*:SELECT"

##
# Database and user / access configuration
##

# Databases to manage
percona_mysql_databases: []
#  - name: example_database
#    collation: utf8_bin
#    encoding: utf8
#    state: present

# Users to manage
percona_mysql_users: []
#  - name: example_user
#    host: localhost
#    password: my_password
#    priv: "*.example_database:USAGE"
#    append_privs: no
#    encrypted: no
#    state: present

##
# Percona MySQL settings
##
percona_mysql_user: mysql
percona_mysql_pid: /var/run/mysqld/mysqld.pid
percona_mysql_socket: /var/run/mysqld/mysqld.sock
percona_mysql_port: 3306
percona_mysql_basedir: /usr
percona_mysql_datadir: /var/lib/mysql
percona_mysql_tmpdir: /tmp
percona_mysql_lc_messages_dir: /usr/share/mysql

# Transaction isolation level
percona_mysql_transaction_isolation: REPEATABLE-READ

# Encoding settings
percona_mysql_collation: utf8_bin
percona_mysql_character_set: utf8

# Listening IP - address binding
percona_mysql_bind_address: 1.2.3.4

# Additional settings and defaults
percona_mysql_use_explicit_defaults_for_timestamp: true
percona_mysql_sql_mode: NO_ENGINE_SUBSTITUTION,STRICT_ALL_TABLES,NO_ZERO_DATE,NO_ZERO_IN_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER

##
# Safety configuration
##
percona_mysql_max_allowed_packet: 128M
percona_mysql_skip_name_resolve: true
percona_mysql_wait_timeout: 60
percona_mysql_interactive_timeout: 60
percona_mysql_max_connections: 150

##
# Cache configuration
##
percona_mysql_query_cache_size: 0
percona_mysql_query_cache_type: 0
percona_mysql_thread_cache_size: 10

##
# Logging configuration
##
percona_mysql_error_log: /var/log/mysql/error.log

# General query log
percona_mysql_general_log_enabled: 0
percona_mysql_general_log: /var/log/mysql/general.log
percona_mysql_slow_query_log_enabled: 0
percona_mysql_slow_query_log: /var/log/mysql/slow-query.log
percona_mysql_long_query_time: 10
percona_mysql_log_query_not_using_indexes: 0

##
# Replication and binary logs
##

# Server id
percona_mysql_server_id: 1

percona_mysql_server_role: master # slave

# Enable binary logs
percona_mysql_binary_log_enabled: false
percona_mysql_binary_log_dir: /var/log/mysql
percona_mysql_binary_log_file: mysql-bin.log

# Enable slave logging to own binary logs (requires enabled binary log on slave)
percona_mysql_log_slave_updates: 1

# Global transaction id
percona_mysql_gtid_mode: ON
percona_mysql_gtid_enforce_consistency: ON

# Parallel slave workers
percona_mysql_slave_parallel_type: LOGICAL_CLOCK
percona_mysql_slave_parallel_workers: 5
percona_mysql_binlog_group_commit_sync_delay: 2000

# Binlog settings
percona_mysql_binlog_format: MIXED
percona_mysql_expire_log_days: 7
percona_mysql_max_binlog_size: 1G

# Replication settings
percona_mysql_replicate_ignore_db:
  - sys
  - information_schema

##
# InnoDB configuration
##
percona_mysql_innodb_file_per_table: 1

# Buffer pool settings
percona_mysql_innodb_buffer_pool_size: 1G
percona_mysql_innodb_buffer_pool_instanes: 4
percona_mysql_innodb_lru_scan_depth: 1024

# Transaction log settings
percona_mysql_innodb_flugh_log_at_trx_commit: 1
percona_mysql_innodb_log_file_size: 2G
percona_mysql_innodb_flush_method: O_DIRECT
percona_mysql_innodb_log_buffer_size: 16M

# I/O settings
percona_mysql_innodb_read_io_threads: 8
percona_mysql_innodb_write_io_threads: 8
percona_mysql_innodb_io_capacity: 4000
percona_mysql_innodb_io_capacity_max: 6000

# Double write buffer settings
percona_mysql_innodb_doublewrite: 1

# Thread concurrency
percona_mysql_innodb_thread_concurrency: 0

##
# Additional settings
##
percona_mysql_group_concat_max_len: 2048

# Back log settings. Values >128 require kernel reconfig
percona_mysql_back_log: -1 # -1 for auto sizing. Will be set omitting entry
```
Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: methorz.percona_mysql }

License
-------

BSD

Author Information
------------------

https://twitter.com/methor_z
