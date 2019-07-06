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

An ansible role for the [drone](https://github.com/drone/drone) CI deployment with Github integration and PostgreSQL database.

## Role Variables

Available variables:

|Name|Default|Description|
|:--:|:--:|:----------|
|**`drone_version`**|`1`|Version of Drone CI, see other versions [here](https://hub.docker.com/r/drone/drone/tags)|
|**`drone_user_filter`**|`""`|Comma-separated list of accounts. Registration is limited to users included in this list, or users that are members of organizations included in this list.|
|**`drone_user_create`**|`""`|List of users with admin access to the drone, readme [more](https://docs.drone.io/administration/user/admins)|
|**`drone_runners`**|`[{name: "Nancy"}]`|Name of the docker agent container, you can add more than one agent|
|**`drone_runner_capacity`**|`""`|Unsigned integer value configures the number of concurrent pipelines an agent can execute. Concurrency is disabled if this value is not set.|
|**`drone_rpc_server`**|`""`|The url, where drone instance will be publicly available. Typically you would have nginx in front of Drone CI. Example: https://ci.auxilin.com|
|**`drone_rpc_secret`**|`hTirsXmrY4YsyK79ELgB`|Required string literal value provides the drone shared secret. This is used to authenticate the rpc connection to the server. The server and agent must be provided the same secret value, [more info](https://docs.drone.io/reference/server/drone-rpc-secret)|
|**`drone_server_host`**|`""`|Required string literal value specifies the Drone server hostname. Example: ci.auxilin.com|
|**`drone_server_proto`**|`http`|Required string literal value specifies the Drone server protocol.|
|**`drone_tls_autocert`**|`"false"`|Automatically generates an SSL certificate using Lets Encrypt, and configures the server to accept HTTPS requests.|
|**`drone_logs_debug`**|`"false"`|Enable debug-level logging.|
|**`drone_logs_trace`**|`"false"`|Enable trace logs.|
|**`drone_cookie_secret`**|`""`|A secret key used to sign authentication cookies. This value is optional. If unset, a random value is generated each time the server is started.|
|**`drone_cookie_timeout`**|`72h`|Duration value sets the authentication cookie expiration.|
|**`drone_github_client_id`**|`""`|Github oauth application client identifier, [more info](https://docs.drone.io/installation/github/multi-machine)|
|**`drone_github_secret`**|`""`|Github oauth application client secret, [more info](https://docs.drone.io/installation/github/multi-machine)|
|**`drone_git_always_auth`**|`"false"`|Boolean value configures Drone to authenticate when cloning public repositories. This is only required when your source code management system (e.g. GitHub Enterprise) has private mode enabled.|
|**`drone_github_server`**|`https://github.com`|String literal value provides the github server address.|
|**`drone_database_secret`**|`""`|Drone supports aesgcm encryption of secrets stored in the database. You must enable encryption before any secrets are stored in the database, [read more](https://docs.drone.io/install/server/storage/encryption)|
|**`drone_postgress_password`**|`droneRocks23@p`|A password to postgress db used by drone|
|**`drone_postgress_user`**|`drone`|A username to postgress db used by drone, [read more](https://docs.drone.io/install/server/storage/postgres/)|
|**`drone_postgress_db`**|`drone`|A name of to postgress db used by drone, [read more](https://docs.drone.io/install/server/storage/postgres/)|
|**`drone_postgress_data_dir`**|`/drone-postgres-data`|A directory on a host machine, where postgresql data stored|
|**`drone_isolated_network`**|`drone_isolated_network`|Docker network for `postgres`, `drone server` and `drone agent` containers.|
|**`ansible_drone_deploy_agents`**|`true`|The boolean value indicates whether start deploy of the `drone` agents or not.|
|**`ansible_drone_deploy_server`**|`true`|The boolean value indicates whether start deploy of the `drone` server or not.|
|**`ansible_drone_deploy_postgresql`**|`true`|The boolean value indicates whether start deploy of the `postgres` database or not.|

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
      drone_version: 1

      # List of users with admins or orgnizations, who has access to the instance:
      # https://docs.drone.io/reference/server/drone-user-filter/
      drone_user_filter: "auxilincom"

      # use this to create first admin for the drone server
      # read more: https://docs.drone.io/administration/user/admins/
      drone_user_create: "ezhivitsa,anorsich"

      # Name of the docker agent container, you can add more than one agent
      drone_runners: [{name: "Nancy"}]
      drone_runner_capacity: 2
      # Url to your instance with drone admin, e.x.: https://drone.org.com
      drone_rpc_server: https://ci.auxilin.com
      # Drone secret key, used for private communication between agent and web UI
      # more info: https://docs.drone.io/reference/server/drone-rpc-secret/
      drone_rpc_secret: hTirsXmrY4YsyK79ELgB

      # the url, where drone instance will be publicly available
      # typically you would have nginx in front of Drone CI
      drone_server_host: "ci.auxilin.com"
      drone_server_proto: "https"
      drone_cookie_secret: "cookie-secret"

      # Github oauth application client identifier
      # more info https://docs.drone.io/installation/github/multi-machine
      drone_github_client_id:
      # Github oauth application client secret
      # more info https://docs.drone.io/installation/github/multi-machine
      drone_github_client_secret:

      # Drone supports aesgcm encryption of secrets stored in the database.
      # details https://docs.drone.io/administration/server/database/
      drone_database_secret: "0c549fd39ae397333761d2cb0c53c219"

      # A password to postgress db used by drone
      drone_postgress_password: droneRocks23@p
      # A username to postgress db used by drone, read more: https://docs.drone.io/install/server/storage/postgres/
      drone_postgress_user: drone
      # A name of to postgress db used by drone, read more: https://docs.drone.io/install/server/storage/postgres/
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
<table>
  <tr>
    <td align="center"><a href="https://github.com/anorsich"><img src="https://avatars3.githubusercontent.com/u/681396?v=4" width="100px;" alt="Andrew Orsich"/><br /><sub><b>Andrew Orsich</b></sub></a><br /><a href="https://github.com/auxilin/ansible-drone/commits?author=anorsich" title="Documentation">📖</a> <a href="#ideas-anorsich" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/auxilin/ansible-drone/commits?author=anorsich" title="Code">💻</a> <a href="https://github.com/auxilin/ansible-drone/commits?author=anorsich" title="Documentation">📖</a> <a href="#ideas-anorsich" title="Ideas, Planning, & Feedback">🤔</a> <a href="#review-anorsich" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="https://github.com/ezhivitsa"><img src="https://avatars2.githubusercontent.com/u/6461311?v=4" width="100px;" alt="Evgeny Zhivitsa"/><br /><sub><b>Evgeny Zhivitsa</b></sub></a><br /><a href="https://github.com/auxilin/ansible-drone/commits?author=ezhivitsa" title="Documentation">📖</a> <a href="https://github.com/auxilin/ansible-drone/commits?author=ezhivitsa" title="Code">💻</a></td>
  </tr>
</table>

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors) specification. Contributions of any kind welcome!
