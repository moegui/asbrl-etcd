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

Suggested setup script to create root user
----------------

# Disable Auth

docker run -it --rm --network host \
--env ALLOW_NONE_AUTHENTICATION=yes \
quay.io/coreos/etcd:v3.4.16 etcdctl --endpoints http://some-ip:2379 user add root

docker run -it --rm --network host \
--env ALLOW_NONE_AUTHENTICATION=yes \
quay.io/coreos/etcd:v3.4.16 etcdctl --endpoints http://some-ip:2379 role add root

docker run -it --rm --network host \
--env ALLOW_NONE_AUTHENTICATION=yes \
quay.io/coreos/etcd:v3.4.16 etcdctl --endpoints http://some-ip:2379 role grant-permission root --prefix=true readwrite /

docker run -it --rm --network host \
--env ALLOW_NONE_AUTHENTICATION=yes \
quay.io/coreos/etcd:v3.4.16 etcdctl --endpoints http://some-ip:2379 user grant-role root root

docker run -it --rm --network host \
--env ALLOW_NONE_AUTHENTICATION=yes \
quay.io/coreos/etcd:v3.4.16 etcdctl --endpoints http://some-ip:2379 auth enable

# Create root user for v2 API and disable guess

curl --data '{"user":"root","password":"abc","roles": ["root"]}' -X PUT http://some-ip:2379/v2/auth/users/root
curl  http://some-ip:2379/v2/auth/enable
curl -X DELETE http://root:abc@some-ip:2379/v2/auth/enable
curl -X PUT http://some-ip:2379/v2/auth/enable
curl -X DELETE 'http://root:abc@some-ip:2379/v2/auth/roles/guest'

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