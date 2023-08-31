ansible-percona-mysql-role
=========

Support for Percona MySQL 8 for Debian

Requirements
------------

None

Role Variables: MySQL Daemon configuration
--------------
```YAML
##
# Percona MySQL Daemon configuration
##
# Basic MySQL configuration
percona_mysql_config_user: mysql
percona_mysql_config_pid: /var/run/mysqld/mysqld.pid
percona_mysql_config_socket: /var/run/mysqld/mysqld.sock
percona_mysql_config_datadir: /var/lib/mysql
percona_mysql_config_basedir: /usr
percona_mysql_config_tmpdir: /tmp
percona_mysql_config_lc_messages_dir: /usr/share/mysql

# Transaction isolation level
percona_mysql_config_transaction_isolation: REPEATABLE-READ # Consistent reads

# Encoding settings
percona_mysql_config_collation: utf8mb4_0900_ai_ci
percona_mysql_config_character_set: utf8mb4
percona_mysql_config_skip_client_character_set: true

# Listening IP - Address binding
percona_mysql_config_bind_address: '127.0.0.1'
percona_mysql_config_port: 3306

# Strict mode configuration
percona_mysql_config_use_explicit_defaults_for_timestamp: true # Handles null and default values for timestamps: keep it default = ON
percona_mysql_config_sql_mode: TRADITIONAL # Includes strict mode and throws errors instead of warnings

##
# Timeouts and connections
##
percona_mysql_config_skip_name_resolve: true
percona_mysql_config_wait_timeout: 28800
percona_mysql_config_interactive_timeout: 28800
percona_mysql_config_max_connections: 150

##
# Cache configuration
##
percona_mysql_config_thread_cache_size: 10 # Default: 8 + (max_connections / 100) - Capped at 100

##
# Logging configuration
##
percona_mysql_config_error_log: /var/log/mysql/error.log

# General query log
percona_mysql_config_general_log_enabled: 'OFF'
percona_mysql_config_general_log: /var/log/mysql/general.log
percona_mysql_config_slow_query_log_enabled: 'OFF'
percona_mysql_config_slow_query_log: /var/log/mysql/slow-query.log
percona_mysql_config_long_query_time: 10
percona_mysql_config_log_query_not_using_indexes: 'OFF'

##
# Replication and binary logs
##
# Server id
percona_mysql_config_server_id: 1

# Role of the server
percona_mysql_config_server_role: master

# Enable binary logs
percona_mysql_config_binary_log_enabled: false
percona_mysql_config_binary_log_file: mysql-bin.log

# Binlog settings
percona_mysql_config_binlog_format: ROW
percona_mysql_config_binlog_space_limit: 100G
percona_mysql_config_binlog_expire_logs_seconds: 604800 # 7 days
percona_mysql_config_max_binlog_size: 1G
percona_mysql_config_binlog_group_commit_sync_delay: 2000
percona_mysql_config_binlog_transaction_dependency_tracking: COMMIT_ORDER

# Enable slave logging to own binary logs (requires binary logs to be enabled on slave)
percona_mysql_config_log_replica_updates: 'ON'

# Global transaction id
percona_mysql_config_gtid_mode: 'ON'
percona_mysql_config_gtid_enforce_consistency: 'ON'

# Parallel slave workers
percona_mysql_config_replica_parallel_type: LOGICAL_CLOCK  # Can be removed after MySQL ≥ 8.0.27
percona_mysql_config_replica_parallel_workers: 5
percona_replica_pending_jobs_size_max: 16777216

# Replication settings
percona_mysql_config_replica_preserve_commit_order: 0
percona_mysql_config_replicate_ignore_db:
  - sys
  - information_schema

# Crash safe replication for GTID and Multi-Threaded Replication
percona_mysql_config_sync_binlog: 1
percona_mysql_config_sync_relay_log: 1
percona_mysql_config_sync_source_info: 1
percona_mysql_config_sync_relay_log_info: 1
percona_mysql_config_relay_log_recovery: 'ON'

##
# InnoDB configuration
##
percona_mysql_config_innodb_file_per_table: 'ON'

# Buffer pool settings
percona_mysql_config_innodb_buffer_pool_size: 4G # 80 %
percona_mysql_config_innodb_buffer_pool_instances: 8
percona_mysql_config_innodb_lru_scan_depth: 1024

# Transaction log settings
percona_mysql_config_innodb_flush_log_at_trx_commit: 0
percona_mysql_config_innodb_flush_method: O_DIRECT
percona_mysql_config_innodb_log_buffer_size: 16M
percona_mysql_config_innodb_redo_log_capacity: 21474836480

# I/O settings
percona_mysql_config_innodb_read_io_threads: 8
percona_mysql_config_innodb_write_io_threads: 8
percona_mysql_config_innodb_io_capacity: 4000
percona_mysql_config_innodb_io_capacity_max: 6000

# Double write buffer settings
percona_mysql_config_innodb_doublewrite: 'ON'

# Thread concurrency
percona_mysql_config_innodb_thread_concurrency: 0

##
# Additional settings
##
percona_mysql_config_group_concat_max_len: 2048
percona_mysql_config_max_allowed_packet: 128M # Max allowed value is 1, default 64MB

# Back log settings. Values >128 require kernel re-config
percona_mysql_config_back_log: -1 # -1 for auto sizing. Default = max_connections
```

Role Variables: MySQL Server configuration
--------------
```YAML
##
# Percona MySQL server configuration
##
percona_mysql_enabled: false
percona_mysql_version: 8.0

# Enable jemalloc (and disable transparent huge pages)
percona_mysql_jemalloc_enabled: true

# Sets additional sysctl settings
percona_mysql_sysctl_settings:
  net.core.somaxconn: 4096 # (/proc/sys/net/core/somaxconn) at least 2048 (or straight to 65536 if multipurpose server like Redis oder Webserver)
  net.ipv4.tcp_max_syn_backlog: 4096 # (/proc/sys/net/ipv4/tcp_max_syn_backlog) at least 2048
  vm.zone_reclaim_mode: 1 # /proc/sys/vm/zone_reclaim_mode (change from 0 to 1) - Dependency for numa_interleave: ON
  vm.vfs_cache_pressure: 1000 # /proc/sys/vm/vfs_cache_pressure (for example from 100 to 1000) - Dependency for numa_interleave: ON

##
# Package configuration
##
percona_mysql_packages_mode: present
percona_mysql_toolkit_enabled: false

##
# MySQL additional setting (plugins, etc)
##

# MySQL plugins folder
percona_mysql_plugins_dir: /usr/lib/mysql/plugin

# Files need to exist within the role's "files" directory
percona_mysql_plugins: []

##
# File and directory path configuration
##

# Local directory used for temporary files
percona_mysql_local_temp_dir: /tmp
percona_mysql_root_my_cnf_file: /root/.my.cnf

##
# Default ROOT user configuration
##

# Sets the provided password for root, overrides existing one
percona_mysql_root_password: ''

# Triggers automatic renewal of root password
percona_mysql_renew_existing_password: false

# Creates a file containing the current root password on server
percona_mysql_create_root_passwd_file: true
percona_mysql_root_passwd_file: /root/.pwd-mysql

##
# Creation of an additional read only root user
##
percona_mysql_read_only_user_enabled: true
percona_mysql_read_only_user: root-ro
percona_mysql_read_only_user_priv: "*.*:SELECT"

##
# Database and user / access configuration
##
percona_mysql_default_schema: ''

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
#    priv: "*.example_database:SELECT"
#    append_privs: no
#    encrypted: no
#    state: present
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
