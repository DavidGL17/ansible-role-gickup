Role Name
=========

An ansible role that creates a docker container with [gickup](https://github.com/cooperspencer/gickup) for automatic github, gitlab,... backup. 

Requirements
------------

There are no special requirements for this role

Role Variables
--------------

These are the variables available for this role, alongside their default value : 

```yaml
base_folder: ~/gickup
timezone: Europe/Zurich
```
Variables to control the folder where all the files will be located on the destination server, and what timezone the server is under.

### Github

```yaml
enable_github_sync: false
```
Set this variable to true if you want to backup a github user's repository. If this variable is set to true, you need to give a value to the following variables.


```yaml
github_user: "none"
```

The user from which you want to backup the repositories.

```yaml
github_user_token: "none"
github_username: "none"
github_password: "none"
github_ssh_enabled: false
github_ssh_key:  # can be a path, if none given will use .ssh/id_rsa
```

Gickup gives you 3 ways to authenticate your connection :

- username + password (fill `github_username` and `github_password`)
- sshkey (fill `github_ssh_key`)
- token (fill `github_user_token`)

Depending on which option you choose you need to fill the according variables to login. If you want to use ssh you need to enable it by setting `github_ssh_enabled` to true.

```yaml
github_exclude_list: []
github_include_list: []
github_exclude_orgs: []
```

List of repositories to include/exclude, and organisations to exclude from the backup.

```yaml
github_wiki_enable: false
```

If you want to enable the backup of the wikis from each repository.

### Gitlab

```yaml
enable_gitlab_sync: false
```

Set this variable to true if you want to backup a gitlab user's repository. If this variable is set to true, you need to give a value to the following variables.

```yaml
gitlab_user: "none"
gitlab_url: "https://gitlab.com"
```

The user from which you want to backup the repositories, and the url where the repositories are located.

```yaml
gitlab_user_token: "none"
gitlab_username: "none"
gitlab_password: "none"
gitlab_ssh_enabled: false
gitlab_ssh_key:  # can be a path, if none given will use .ssh/id_rsa
```

Gickup gives you 3 ways to authenticate your connection :

- username + password (fill `gitlab_username` and `gitlab_password`)
- sshkey (fill `gitlab_ssh_key`)
- token (fill `gitlab_user_token`)

Depending on which option you choose you need to fill the according variables to login. If you want to use ssh you need to enable it by setting `gitlab_ssh_enabled` to true.

```yaml
gitlab_exclude_list: []
gitlab_include_list: []
gitlab_exclude_orgs: []
```

List of repositories to include/exclude, and organisations to exclude from the backup.

```yaml
gitlab_wiki_enable: false
```

If you want to enable the backup of the wikis from each repository.

### Destination

```yaml
destination_local_path: "{{ base_folder }}/output"
destination_local_structured: true
```

The path were you want to save the backup. If `destination_local_structured` is set to true, gickup will checkout out the repositories like this : hostersite/user|organization/repo

### Cron

```yaml
enable_cron: true
cron_value: "0 22 * * *"
```

If you want to enable a scheduled backup, enable this. If cron is not enabled, the app will do a backup imeddiatly the kill the container. If cron is enabled, it will do it's first backup when the cron schedule warns him.

### Metrics

```yaml

metrics_endpoint: "/metrics"

metrics_listen_address: "6178"
```

Gickup makes a prometheus metrics point available. These two variables allow you to customise what port it's located at. The `metrics_endpoint` var needs to be put in the url to access these data (for example do `localhost:6178/metrics`).

Dependencies
------------

To run this playbook you need to have docker and docker-compose installed on your remote machine. One way to install them with ansible is to use these two roles : 

- geerlingguy.pip
- geerlingguy.docker

An example of usage would be : 

```yaml
- hosts: all

  vars:
    pip_install_packages:
      - name: docker-compose

  roles:
    - geerlingguy.pip
    - geerlingguy.docker
```

Example Playbook
----------------

```yaml
- hosts: all

  vars:
    pip_install_packages:
      - name: docker-compose

  roles:
    - geerlingguy.pip
    - geerlingguy.docker
    - davidgl17.gickup
```


License
-------

MIT

Tests
-----

[![CI](https://github.com/DavidGL17/ansible-role-gickup/actions/workflows/molecule-ci.yml/badge.svg)](https://github.com/DavidGL17/ansible-role-gickup/actions/workflows/molecule-ci.yml)

Author Information
------------------

This role was created by [David González León](https://github.com/DavidGL17).
