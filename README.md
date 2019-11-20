winlogbeat role
=========
[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-winlogbeat/develop/LICENSE)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-winlogbeat/badges/develop/pipeline.svg)](https://gitlab.com/lean-delivery/ansible-role-winlogbeat/pipelines)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.winlogbeat-blue.svg)](https://galaxy.ansible.com/lean_delivery/winlogbeat)
![Ansible](https://img.shields.io/ansible/role/d/role_id.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2Frole_id%2F&query=$.min_ansible_version)

## Summary


This role:
  - installs winlogbeat on Windows
  - copies prepared configuration file (log path, connect to elasticsearch etc.)


Role tasks
------------


- [Optional] Create folder(s) for custom paths
- Install winlogbeat
- Copy configuration file


Requirements
------------


- Minimal Version of the ansible for installation: 2.8
 - **Supported OS**:
   - Windows
     - 2016
     - 2019


## Role Variables
--------------


You can override any variable below by setting "variable: value" in playbook.


- `winlogbeat_version`
Is used to select main Winlogbeat branch to be installed. Default value is `7`.
- `winlogbeat_last_version`
Is used to select specific Winlogbeat version to be installed. Default value is `7.4.2`
- `winlogbeat_node_name`
Name of the winlogbeat node. Default value is `{{ inventory_hostname }}`. If this options is not defined, the hostname is used.
- `winlogbeat_ssl_enabled`
Turns on/off SSL connection between winlogbeat and logstash/elasticsearch. SSL options should be set by corresponding dict fields like shown below:
```
  ssl:
    key: 'c:\tls\private\server.key'
    certificate: 'c:\tls\certs\server.pem'
    certificate_authorities: 'c:\CA\ca-root.pem'
```


The `path` section of the configuration options defines where Winlogbeat looks for its files. For example, Winlogbeat looks for the Elasticsearch template file in the configuration path and writes log files in the logs path. Winlogbeat looks for its registry files in the data path. Default values for Linux host are set up this way:
```
path:
  home: 'c:\program files\winlogbeat'
  config: 'c:\program files\winlogbeat'
  data: 'c:\programdata\winlogbeat'
  logs: 'c:\programdata\winlogbeat\logs'
```
- `win_download_path`
Temp directory for Windows to download and upzip Winlogbeat package. Default value is `'{{ ansible_env.TEMP }}/winlogbeat'` (ansible_env.TEMP value solves idempotence issue)


## Output customization:
- `winlogbeat_output`
Is used to configure what output to use when sending data (`elasticsearch` or `logstash`). Default value is `elasticsearch`


- `elasticsearch.host`
Array of hosts to connect to. Default value is `localhost`
- `elasticsearch.port`
Value for setting custom port. Default value is `9200`


- `logstash.host`
Array of hosts to connect to. Default value is `localhost`
- `logstash.port`
Value for setting custom port. Default value is `5044`


## Advanced config parameters:


The `winlogbeat(systemd)\initd` section of the configuration  options defines which init script will be used to manage winlogbeat service depending on the *nix OS. Custom paths will be taken into account (if configured).
- `winlogbeat_service_name`
Name of nssm\init script, which manages winlogbeat service


- `winlogbeat_bulk_max_size`
Maximum number of events to bulk in a single Logstash request. Default value is `500`
- `winlogbeat_worker`
Number of workers per Elasticsearch host. Default value is `1`
- `winlogbeat_logging_to_syslog`
Send all logging output to syslog. Default value is `false`
- `winlogbeat_logging_to_files`
Send all logging output to rotating files. Default value is `true`
- `winlogbeat_rotateeverybytes`
Defines log file size limit. Defalt value is `104857600` = `100MB`
- `winlogbeat_keepfiles`
Number of log files to keep. Default value is `30`
- `winlogbeat_ignore_older`
Value (any time strings like 2h, 5m can be used) above which logs will be ignored. Default value is `0` (disabled)
- `winlogbeat_logname`
Name of the logging files. Default value is `"winlogbeat.log"`


## Dependencies
------------


ca-cert (only for installation with SSL)


Example Playbook
----------------


### Installing Winlogbeat 7.x version:


```yaml
- name: Install winlogbeat
  hosts: all
  roles:
    - role: ansible-role-winlogbeat
```


License
-------
Apache


Author Information
------------------


authors:
  - Lean Delivery Team <team@lean-delivery.com>
