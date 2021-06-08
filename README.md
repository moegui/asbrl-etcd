ASBRL-ETCD
=========

Deploy ETCD

Requirements
------------

Need to have Docker engine installed.

Role Variables
--------------

- default_user: 'ubuntu'
- ROOT_USERNAME: 'postgres'
- ROOT_PASSWORD: 'Pa$$w0rd'
- BUILD: '12'
- LISTEN_ADDRESSES: '*'
- PORT: 5432
- MAX_CONNECTIONS: 100
- SHARED_BUFFERS: '128MB' # 15-25% of Total RAM
- WAL_BUFFERS: '4MB' # 3% of shared_buffers
- EFFECTIVE_CACHE_SIZE: '512MB' # 50-75% of Total RAM.
- WORK_MEM: '4MB' # 25% of Total RAM / max_connections
- MAINTENANCE_WORK_MEM: '64MB' # 5% of Total RAM
- DOCKER_NAME: 'postgres'
- DOCKER_CPU_PERIOD: 0
- DOCKER_CPU_QUOTA: 0
- DOCKER_MEMORY: 0
- CONTAINER_STATE: 'started'
- VOLUME_STATE: 'present'
- REPLICA_USERNAME: 'replica_usr'
- REPLICA_PASSWORD: 'replica1234'
- REPLICA_ROLE: 'NONE' # NONE/MASTER/SLAVE
- REPLICA_SLOT: 'replicator'
- REPLICA_SOURCE: '0.0.0.0/0'
- REPLICA_METHOD: 'md5'

Dependencies
------------

None

Example Playbook
----------------

      - name: Deploy PostgreSQL Master
        include_role:
          name: asbrl-postgresql
        vars:
          default_user: ubuntu
          DOCKER_NAME: pg1
          PORT: 15432
          ROOT_USERNAME: "postgres"
          ROOT_PASSWORD: "1234"
          REPLICA_ROLE: "MASTER"
          REPLICA_USERNAME: 'replica_usr'
          REPLICA_PASSWORD: 'replica1234'

      - name: Deploy PostgreSQL Slave
        include_role:
          name: asbrl-postgresql
        vars:
          default_user: ubuntu
          DOCKER_NAME: pg2
          PORT: 25432
          ROOT_USERNAME: "postgres"
          ROOT_PASSWORD: "1234"
          REPLICA_ROLE: "SLAVE"
          REPLICA_MASTER_HOST: '172.17.0.1'
          REPLICA_MASTER_PORT: 15432
          REPLICA_USERNAME: 'replica_usr'
          REPLICA_PASSWORD: 'replica1234'

License
-------

BSD

Author Information
------------------

Moegui.com