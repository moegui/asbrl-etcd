ASBRL-ETCD
=========

Deploy ETCD

Requirements
------------

Need to have Docker engine installed.

Role Variables
--------------

- CONTAINER_NAME: etcd
- IMAGE: bitnami/etcd
- IMAGE_TAG: '3.4.3'
- DOCKER_CPU_PERIOD: 0
- DOCKER_CPU_QUOTA: 0
- DOCKER_MEMORY: 0
- CONTAINER_STATE: 'started'
- VOLUME_STATE: 'present'
- DOCKER_LOG_DRIVER: "json-file"
- DOCKER_LOG_OPTIONS: "{}"
- DOCKER_IMAGE_PUSH: false
- CLIENT_PORT: '2379'
- PEER_PORT: '2380'

Dependencies
------------

None

Example Playbook
----------------

    - name: Run etcd1
      include_role:
        name: asbrl-etcd
      vars:
        ETCD_INITIAL_CLUSTER: etcd1=http://3.80.73.102:2380,etcd2=http://34.228.9.16:2380,etcd3=http://34.228.77.225:2380
        ETCD_URL: http://3.80.73.102
        ETCD_TOKEN: pgEtcdCluster1
        ETCD_NAME: etcd1

    - name: Run etcd2
      include_role:
        name: asbrl-etcd
      vars:
        ETCD_INITIAL_CLUSTER: etcd1=http://3.80.73.102:2380,etcd2=http://34.228.9.16:2380,etcd3=http://34.228.77.225:2380
        ETCD_URL: http://34.228.9.16
        ETCD_TOKEN: pgEtcdCluster1
        ETCD_NAME: etcd2

    - name: Run etcd3
      include_role:
        name: asbrl-etcd
      vars:
        ETCD_INITIAL_CLUSTER: etcd1=http://3.80.73.102:2380,etcd2=http://34.228.9.16:2380,etcd3=http://34.228.77.225:2380
        ETCD_URL: http://34.228.77.225
        ETCD_TOKEN: pgEtcdCluster1
        ETCD_NAME: etcd3

License
-------

BSD

Author Information
------------------

Moegui.com