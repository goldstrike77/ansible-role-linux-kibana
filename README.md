![](https://img.shields.io/badge/Ansible-kibana-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_kibana.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Logging vs Monitoring](#logging-vs-monitoring)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Kibana Versions](#kibana-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
Kibana is the 'K' in the ELK Stack, the world’s most popular open-source log analysis platform, and provides users with a tool for exploring, visualizing, and building dashboards on top of the log data stored in Elasticsearch clusters. Kibana’s core feature is data querying and analysis. Using various methods, users can search the data indexed in Elasticsearch for specific events or strings within their data for root cause analysis and diagnostics. Based on these queries, users can use Kibana’s visualization features which allow users to visualize data in a variety of different ways, using charts, tables, geographical maps and other types of visualizations.

## Logging vs Monitoring
The key difference between the Grafana and Kibana. Grafana is designed for analyzing and visualizing metrics such as system CPU, memory, disk and I/O utilization. Grafana does not allow full-text data querying. Kibana, on the other hand, runs on top of Elasticsearch and is used primarily for analyzing log messages. If you are building a monitoring system, both can do the job pretty well, though there are still some differences that will be outlined below. If it’s logs you’re after, for any of the use cases that logs support troubleshooting, forensics, development, security, Kibana is your choice.

## Requirements
### Operating systems
This Ansible role installs Kibana on Linux operating system, including establishing a filesystem structure and server configuration with some common operational features, Will works on the following operating systems:

  * CentOS 7

### Kibana versions

The following list of supported the kibana releases:

  * Kibana 6.8+, 7.1+

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `kibana_version`: Specify the Kibana version.
* `kibana_auth`: A boolean value, Enable or Disable authentication.
* `kibana_user`: Authorization user name, do not modify it.
* `kibana_pass`: Authorization user password.
* `kibana_https`: A boolean value, whether Encrypting HTTP client communications.
* `kibana_proxy`: Whether running behind a proxy.

##### Role dependencies
* `kibana_elastic_dept`: A boolean value, whether ElasticSearch use the same environment.
* `kibana_ngx_dept`: A boolean value, whether proxy web interface and API traffic using NGinx.

##### Listen port
* `kibana_port_server`: Kibana server port.

##### Elasticsearch parameters
* `kibana_elastic_host`: Elasticsearch instances.
* `kibana_elastic_port`: Elasticsearch REST port.

##### NGinx parameters
* `kibana_ngx_block_agents`: Enables or disables block unsafe User Agents.
* `kibana_ngx_block_string`: Enables or disables block includes Exploits / File injections / Spam / SQL injections.
* `kibana_ngx_compress`: Enables or disables compression.
* `kibana_ngx_domain`: Defines domain name.
* `kibana_ngx_port_http`: NGinx HTTP listen port.
* `kibana_ngx_port_https`: NGinx HTTPs listen port.
* `kibana_ngx_ssl_protocols`: intermediate or modern, defines SSL protocol profile.
* `kibana_ngx_site_path`: Specify the NGinx site directory.
* `kibana_ngx_logs_path`: Specify the NGinx logs directory.

##### Elastic parameters
* `kibana_elastic_https`: A boolean value, whether Encrypting HTTP client communications.
* `kibana_elastic_cluster`: Specify name for your Elastic cluster name.
* `kibana_elastic_path`: Specify the ElasticSearch data directory.
* `kibana_elastic_port_rest`: Elasticsearch REST port.
* `kibana_elastic_node_type`: Type of nodes: default, master, data, ingest and coordinat.
* `kibana_elastic_heap_size`: Specify the maximum memory allocation pool for a Java virtual machine.
* `kibana_elastic_memory_lock`: A boolean value, whether lock the process address space into memory on startup.

##### Plugins
* `kibana_plugins`: Plug-in List.

##### Service Mesh
* `environments`: Define the service environment.
* `datacenter`: Define the DataCenter.
* `domain`: Define the Domain.
* `customer`: Define the customer name.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

### Other parameters
There are some variables in vars/main.yml:

## Dependencies
- Ansible versions >= 2.8
- Python >= 2.7.5
- [NGinx](https://github.com/goldstrike77/ansible-role-linux-nginx.git)
- [Elasticsearch](https://github.com/goldstrike77/ansible-role-linux-elasticsearch.git)

## Example

### Hosts inventory file
See tests/inventory for an example.

    node01 ansible_host='192.168.1.10'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
- hosts: all
  roles:
     - role: ansible-role-linux-kibana
       kibana_version: '7.9.3'
```

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`.

```yaml
kibana_version: '7.9.3'
kibana_proxy: false
kibana_auth: true
kibana_user: 'elastic'
kibana_pass: 'changeme'
kibana_https: true
kibana_elastic_dept: false
kibana_ngx_dept: false
kibana_port_server: '5601'
kibana_elastic_host: '{{ ansible_default_ipv4.address }}'
kibana_elastic_port: '9200'
kibana_ngx_block_agents: false
kibana_ngx_block_string: false
kibana_ngx_compress: false
kibana_ngx_domain: 'navigate.example.com'
kibana_ngx_port_http: '80'
kibana_ngx_port_https: '443'
kibana_ngx_ssl_protocols: 'modern'
kibana_ngx_site_path: '/data/nginx_site'
kibana_ngx_logs_path: '/data/nginx_logs'
kibana_elastic_https: true
kibana_elastic_cluster: 'ossec'
kibana_elastic_path: '/data'
kibana_elastic_port_rest: '9200'
kibana_elastic_node_type: 'default'
kibana_elastic_heap_size: '3g'
kibana_elastic_memory_lock: false
kibana_plugins:
  - 'http://packages.wazuh.com/wazuhapp/wazuhapp-3.9.2_{{ kibana_version }}.zip'
environments: 'prd'
datacenter: 'dc01'
domain: 'local'
customer: 'demo'
tags:
  subscription: 'default'
  owner: 'nobody'
  department: 'Infrastructure'
  organization: 'The Company'
  region: 'China'
exporter_is_install: false
consul_public_register: false
consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
consul_public_http_prot: 'https'
consul_public_http_port: '8500'
consul_public_clients:
  - '127.0.0.1'
```

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
