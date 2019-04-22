# Ansible Drone

[![Auxilin.com — Production ready Node, React starter kit for building products at a warp speed](https://raw.githubusercontent.com/auxilincom/component-template/master/assets/cover-black.png)](https://github.com/auxilincom/auxilin)

[![All Contributors](https://img.shields.io/badge/all_contributors-2-orange.svg?style=flat-square)](#contributors)
[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-drone-blue.svg?style=flat-square)](https://galaxy.ansible.com/auxilincom/drone)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square)](https://github.com/auxilincom/ansible-drone/blob/master/LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
[![David Dependancy Status](https://david-dm.org/auxilincom/ansible-drone.svg)](https://david-dm.org/auxilincom/ansible-drone)

[![Watch on GitHub](https://img.shields.io/github/watchers/auxilincom/ansible-drone.svg?style=social&label=Watch)](https://github.com/auxilincom/ansible-drone/watchers)
[![Star on GitHub](https://img.shields.io/github/stars/auxilincom/ansible-drone.svg?style=social&label=Stars)](https://github.com/auxilincom/ansible-drone/stargazers)
[![Follow](https://img.shields.io/twitter/follow/auxilin.svg?style=social&label=Follow)](https://twitter.com/auxilin)
[![Tweet](https://img.shields.io/twitter/url/https/github.com/auxilincom/ansible-drone.svg?style=social)](https://twitter.com/intent/tweet?text=I%27m%20using%20Auxilin%20components%20to%20build%20my%20next%20product%20🚀.%20Check%20it%20out:%20https://github.com/auxilincom/ansible-drone)
[![@auxilin](https://img.shields.io/badge/%F0%9F%92%AC%20Telegram-t.me/auxilin-blue.svg)](https://t.me/auxilin)

An ansible role for the [drone](https://github.com/drone/drone) CI (version 1.0.0 and higher) deployment with Github integration and PostgreSQL database.

## Role Variables

Available variables:

|Name|Default|Description|
|:--:|:--:|:----------|
|**`drone_network_name`**|`drone_isolated_network`|Name of the docker network for postgres, drone and drone agent.|
|**`drone_version`**|`latest`|Version of Drone CI, see other versions [here](https://hub.docker.com/r/drone/drone/tags)|
|**`drone_users```|`""`|Optional comma-separated list of accounts. Registration is limited to users included in this list, read [more](https://docs.drone.io/reference/server/drone-user-filter/)|
|**`drone_admins`**|`""`|List of users with admin access to the drone, read [more](https://docs.drone.io/administration/user/admins/)|
|**`drone_agents`**|`[{name: "Nancy"}]`|Name of the docker agent container, you can add more than one agent|
|**`drone_secret`**|`hTirsXmrY4YsyK79ELgB`|Drone secret key, used for private communication between agent and web UI [more info](https://docs.drone.io/reference/server/drone-rpc-secret/)|
|**`drone_github_client_id`**|`""`|Github oauth application client identifier, [more info](https://docs.drone.io/installation/github/single-machine/)|
|**`drone_github_secret`**|`""`|Github oauth application client secret, [more info](https://docs.drone.io/installation/github/single-machine/)|
|**`drone_git_always_auth`**|`false`|Boolean value configures Drone to authenticate when cloning public repositories, [more info](https://docs.drone.io/reference/server/drone-git-always-auth/)|
|**`drone_github_server`**|`"https://github.com"`|A string containing your GitHub server address|
|**`drone_runner_capacity`**|`2`|An integer defining the maximum number of pipelines the agent should execute concurrently|
|**`drone_server_proto`**|`"https"`|A string containing your Drone server protocol scheme. This value should be set to http or https.|
|**`drone_server_host`**|`""`|A string containing your Drone server hostname or IP address.|
|**`drone_tls_autocert`**|`false`|An boolean indicating debug level logs should be use for automatic SSL certification generation and configuration|
|**`drone_database_secret`**|`""`|Drone supports aesgcm encryption of secrets stored in the database, [details](https://docs.drone.io/administration/server/database/)|
|**`drone_postgres_version`**|`"latest"`|Version of Postgres, see other versions [here](https://hub.docker.com/_/postgres?tab=tags)|
|**`use_remote_drone_postgres`**|`false`|Use remote postgres or install docker container (if `false` then create container in the network `drone_network_name`)|
|**`drone_postgres_host`**|`localhost`|Use this variable if `use_remote_postgres` is `true` to set postgres host|
|**`drone_postgres_port`**|`5432`|Use this variable if `use_remote_postgres` is `true` to set postgres port|
|**`drone_postgres_password`**|`droneRocks23@p`|A password to postgres db used by drone|
|**`drone_postgres_user`**|`drone`|A username to postgres db used by drone, [read more](https://docs.drone.io/administration/server/database/)|
|**`drone_postgres_db`**|`drone`|A name of to postgress db used by drone, [read more](https://docs.drone.io/administration/server/database/)|
|**`drone_postgres_data_dir`**|`/drone-postgres-data`|A directory on a host machine, where postgresql data stored|

## Dependencies

[Docker](https://www.docker.com/) must be installed on the server in order to use this role. If you don't have docker on your server we recommend [angstwad.docker_ubuntu](https://github.com/angstwad/docker.ubuntu) Ansible role.

Example of using `angstwad.docker_ubuntu`:
```yml
---
- name: Setup drone ci server
  hosts: drone
  become: true
  roles:
    - { role: angstwad.docker_ubuntu }
```

## Quick example

Example of the playbook file:

```yml
---
- name: Deploy drone CI server
  hosts: drone
  become_user: root
  become: true
  roles:
    - role: auxilincom.drone
      # Version of Drone CI, see other versions here: https://hub.docker.com/r/drone/drone/tags/
      drone_version: latest

      # the url, where drone instance will be publicly available
      # typically you would have nginx in front of Drone CI
      drone_host: http://178.62.116.103

      # Drone secret key, used for private communication between agent and web UI
      # more info: http://docs.drone.io/install-for-github/
      drone_secret: hTirsXmrY4YsyK79ELgB

      # Github oauth application client identifier and secret, more info http://docs.drone.io/install-for-github/
      drone_github_client:
      drone_github_secret:

      # A password to postgress db used by drone
      drone_postgress_password: droneRocks23@p
      # A username to postgress db used by drone, read more: http://docs.drone.io/database-settings/
      drone_postgress_user: drone
      # A name of to postgress db used by drone, read more: http://docs.drone.io/database-settings/
      drone_postgress_db: drone
      # a directory on a host machine, where postgresql data stored
      drone_postgress_data_dir: /drone-postgres-data
```

## Change Log

This project adheres to [Semantic Versioning](http://semver.org/).
Every release is documented on the Github [Releases](https://github.com/auxilincom/ansible-drone/releases) page.

## License

Ansible-drone is released under the [MIT License](https://github.com/auxilincom/ansible-drone/blob/master/LICENSE).

## Contributing

Please read [CONTRIBUTING.md](https://github.com/auxilincom/ansible-drone/blob/master/CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.

## Contributors

Thanks goes to these wonderful people ([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
| [<img src="https://avatars3.githubusercontent.com/u/681396?v=4" width="100px;"/><br /><sub><b>Andrew Orsich</b></sub>](https://github.com/anorsich)<br />[📖](https://github.com/auxilin/ansible-drone/commits?author=anorsich "Documentation") [🤔](#ideas-anorsich "Ideas, Planning, & Feedback") [💻](https://github.com/auxilin/ansible-drone/commits?author=anorsich "Code") [📖](https://github.com/auxilin/ansible-drone/commits?author=anorsich "Documentation") [🤔](#ideas-anorsich "Ideas, Planning, & Feedback") [👀](#review-anorsich "Reviewed Pull Requests") | [<img src="https://avatars2.githubusercontent.com/u/6461311?v=4" width="100px;"/><br /><sub><b>Evgeny Zhivitsa</b></sub>](https://github.com/ezhivitsa)<br />[📖](https://github.com/auxilin/ansible-drone/commits?author=ezhivitsa "Documentation") |
| :---: | :---: |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind welcome!
