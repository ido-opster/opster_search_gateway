Opster Search Gateway
=====================

Hello and thanks for using Opster Search Gateway. We have created this Ansible Role to help you deploy our application in several ways.

* Docker
* Packages (future)
* Kubernetes (future)

Requirements
------------

This role install all collections and role requirements by default. You have to only be sure to use Ansible 2.11 or above.

```bash
$ sudo apt|yum install python3-pip
$ pip3 install --upgrade ansible-core==2.11.*
```

Role Variables
--------------

| Parameter                                                   | Description                                                                                                            | Default       |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- | ------------- |
| `opster_sgw_version`                                      | Application version to deploy.                                                                                         | `latest`    |
| `opster_sgw_port`                                         | Application listen port                                                                                                | `9200`      |
|                                                             |                                                                                                                        |               |
| `opster_sgw_logger_url.protocol`                          | The ES protocol for destination logs. (http, https)                                                                    | `http`      |
| `opster_sgw_logger_url.host`                              | The ES hostname or IP for destination logs                                                                             | `localhost` |
| `opster_sgw_logger_url.port`                              | The ES port for destination logs                                                                                       | `9201`      |
| `opster_sgw_logger.index`                                 | The ES index of destination logs                                                                                       | `opster-sg` |
| `opster_sgw_logger.batch_size`                            | The log bulk size                                                                                                      | `10`        |
| `opster_sgw_logger.doc_type`                              |                                                                                                                        | `doc`       |
| `opster_sgw_logger.batch_timeout`                         | The timeout for bulk insert                                                                                            | `5000`      |
| `opster_sgw_logger.retry`                                 | The amount of bulk insert retries until log is thrown away                                                             | `3`         |
| `opster_sgw_logger.auth.username`                         | If basic authentication is needed, you can set the username here                                                       |               |
| `opster_sgw_logger.auth.password`                         | If basic authentication is needed, you can set the password here                                                       |               |
|                                                             |                                                                                                                        |               |
| `opster_sgw_health_checker.enable`                        | When true there will be monitor to see if cluster is responding to a simple Get request                                | `default`   |
| `opster_sgw_health_checker.threadPool`                    | The amount of threads, to monitor the backend clusters’ health. Usually, no more than 1 is needed                     | `1`         |
| `opster_sgw_health_checker.periodInMilliSeconds`          | Health checks interval                                                                                                 | `1000`      |
|                                                             |                                                                                                                        |               |
| `opster_sgw_route.userParameter`                          | A header name, to be used to tag user parameters                                                                       | `X-User-Id` |
| `opster_sgw_route.analyzeQuery`                           | When false no analyzing of searches occurs                                                                             | `default`   |
| `opster_sgw_route.searchConfigInElastic`                  | When true will search cost config in app cluster                                                                       | `false`     |
| `opster_sgw_route.useOpaque`                              | When true will add Opaque parameters                                                                                   | `false`     |
| `opster_sgw_route.logBody`                                | When true log messages will include request body                                                                       | `false`     |
| `opster_sgw_route.logPreSend`                             | When true additional log pre elastic search send will be written                                                       | `false`     |
| `opster_sgw_route.sniffNodeIntervalInSeconds`             | Indicate when to reset the round robin cursor                                                                          | `false`     |
|                                                             |                                                                                                                        |               |
| `opster_sgw_cacheconfiguration.expensiveQueriesCacheSize` | Expensive queries amount to cache                                                                                      | `1`         |
| `opster_sgw_cacheconfiguration.slowQueriesCacheSize`      | Slow queries amount to cache                                                                                           | `1`         |
| `opster_sgw_cacheconfiguration.cacheBulkFetchSize`        | Cache query size param. When set as default, no cache loading will occur                                               | `0`         |
| `opster_sgw_cacheconfiguration.maxFetching`               | Scrolling amount (how many times scrolled to get more cache results). When set as default, no cache loading will occur | `0`         |
|                                                             |                                                                                                                        |               |
| `opster_sgw_backends.url`                                 | Full Elasticsearch Url. For example,`http://localhost:9200`                                                          |               |
| `opster_sgw_backends.username`                            | Username for authentication (string)                                                                                   |               |
| `opster_sgw_backends.password`                            | Password for authentication (string)                                                                                   |               |
| `opster_sgw_backends.default`                             | Indicate if this backend is the default one (boolean)                                                                  |               |
| `opster_sgw_backends.sniff.allowNodeRoles`                | Filter node by its role (list)                                                                                         |               |
| `opster_sgw_backends.sniff.allowNodeNames`                | Filter node by its Ip (list)                                                                                           |               |
| `opster_sgw_backends.sniff.allowNodeIPs`                  | Filter node by its name (list)                                                                                         |               |
| `opster_sgw_backends.sniff.useDefaultElasticPort`         | When true port 9200 wil be us with the node ip to route request (boolean)                                              |               |
|                                                             |                                                                                                                        |               |
| `opster_sgw_searchgateway.heavySearchCostThreshold`       |                                                                                                                        | `1000`      |
| `opster_sgw_searchgateway.slowSearchTimeInMilliThreshold` |                                                                                                                        | `1000`      |
| `opster_sgw_searchgateway.features`                       | Free-form for all features configuration (map)                                                                         |               |

Dependencies
------------

This role automatically resolve all needed dependencies.

Example Playbook
----------------

You can deploy using this playbook example. Replace with your `host:` group and `backend_url`

```
- name: Deploy Opster Search Gateway
  hosts: gateway
  become: true  

  tasks:
  - name: Deploy Seach Gateway Service
    ansible.builtin.include_role:
      name: opster_search_gateway
      apply:
        tags:
          - sgw  
    vars:
      deploy_using: 'docker'

      opster_sgw_logger_url:
        protocol: "http"
        host: "{{ docker_network_gw }}"
        port: 9201
      opster_sgw_backends:
        - url: 'http://es-backend-hostname:9200'
          default: 'true'
          username: 'elastic'
          password: 'my_secret'
    tags:
      - sgw
```

License
-------

`GPL-3.0-only`

[Opster Search Gateway](https://opster.com/elasticsearch-search-gateway)
---------------------

[Book a Demo?](https://opster.com/book-a-demo-of-the-search-gateway/)
