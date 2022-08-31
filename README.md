# MariaDB Galera packaged by Bitnami
This is the revised version for multi-cluster from https://github.com/bitnami/charts/tree/master/bitnami/mariadb-galera

- added serviceexport.yaml
- changed statefulset.yaml
  - removed MARIADB_GALERA_CLUSTER_ADDRESS env item
  - added MARIADB_GALERA_CLUSTER_SERVICE env item
  - added MARIADB_GALERA_CLUSTER_ADDRESS deducing logic from MARIADB_GALERA_CLUSTER_SERVICE
```
                cluster_address=""
                read -r -a local_ips <<< "$(hostname -i)"
                for ip in $(getent ahosts "${MARIADB_GALERA_CLUSTER_SERVICE}" | awk '{print $1}' | uniq); do
                      for local_ip in "${local_ips[@]}"; do
                          if [[ "$ip" != "$local_ip" ]]; then
                              cluster_address="${cluster_address}${ip},"
                          fi
                      done
                done
                cluster_address=${cluster_address%?} 
                export MARIADB_GALERA_CLUSTER_ADDRESS="gcomm://${cluster_address}"
```
