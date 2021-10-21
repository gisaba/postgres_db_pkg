## Ansible playbook for Postgres DBA

### Introduction

This code provides many tasks for activities on cluster database postgresql. It uses [Postgres Ansible module](https://docs.ansible.com/ansible/2.8/modules/list_of_database_modules.html) and is organized with [Ansible Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html).

### Tasks

All task use  [privilege escalation](https://docs.ansible.com/ansible/latest/user_guide/become.html) so you must use --ask-become-pass parameter. To run a single task uncomment this in /roles/database/tasks/main.yml and comment others and in a playbook.yml file use relative variable file in /roles/database/vars/ directory.

* **new_database.yml** : Create a new database on a cluster and provide to set up also users to manage him. One user <newdbname>_web for application and <newdbname>_owner to administration. This playbook uses **psycopg2** pip module so it needs ** ansible_python_interpreter: "/usr/bin/python3 ** in eache file into **postgres_db_pkg\group_vars** directory. Operating System supported: **Linux CentOS**. ASAP i'll try to add also Ubuntu distribution.

* **create_PG_cluster.yml** : Create a Potgres Cluser (Version indicated in /var/cluster_params.yml) with 2 hot-standby and one witness-Server. Cluster uses [repmgr](https://repmgr.org/) tool. CLuster needs configuration for automatic-failover. This playbook uses **yum** ansible module so it needs ** ansible_python_interpreter: "/usr/bin/python2 ** in eache file into **postgres_db_pkg\group_vars** directory. Operating System supported: **Linux CentOS 7 and CentOS 8**. ASAP i'll try to add also Ubuntu distribution. It uses cluster_params.yml variable file.

This playbook it can also be used to add nodes to the cluster one at a time or add another replica. To do that you have to run [**ansible-playbook**](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html) commando with **--limit** parameter and specify **group** i.e. sudo ansible-playbook playbook.yml -i inventory.ini --limit **pg_slaves**  --ask-become-pass . It uses cluster_params.yml variable file.

* **set_parameters.yml** : Set all parameters included in **roles\database\vars\set_parameters_var.yml** file where **required: True**. Setting is   registered in postgresql.auto.conf
