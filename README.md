[![CI](https://github.com/de-it-krachten/ansible-role-kind/workflows/CI/badge.svg?event=push)](https://github.com/de-it-krachten/ansible-role-kind/actions?query=workflow%3ACI)


# ansible-role-kind

Installs & manages kind (Kubernetes in Docker)<br><br>
Optionally it can setup one or more clusters including the nginx ingress controller<br>
Project: https://kind.sigs.k8s.io<br>
Github: https://github.com/kubernetes-sigs/kind<br>



## Dependencies

#### Roles
- deitkrachten.python
- deitkrachten.docker

#### Collections
- community.general
- kubernetes.core
- kubernetes.core

## Platforms

Supported platforms

- Red Hat Enterprise Linux 7<sup>1</sup>
- Red Hat Enterprise Linux 8<sup>1</sup>
- Red Hat Enterprise Linux 9<sup>1</sup>
- RockyLinux 8
- RockyLinux 9
- OracleLinux 8
- OracleLinux 9
- AlmaLinux 8
- AlmaLinux 9
- SUSE Linux Enterprise<sup>1</sup>
- openSUSE Leap 15<sup>1</sup>
- Debian 10 (Buster)
- Debian 11 (Bullseye)
- Ubuntu 20.04 LTS
- Ubuntu 22.04 LTS
- Fedora 37
- Fedora 38

Note:
<sup>1</sup> : no automated testing is performed on these platforms

## Role Variables
### defaults/main.yml
<pre><code>
# Github CLI - API
kind_api: https://api.github.com/repos/kubernetes-sigs/kind

# Github CLI - repo
kind_repo: https://github.com/kubernetes-sigs/kind

# Lookup table for architecture
kind:
  architecture:
    x86_64: amd64
    i386: i386
    armv6l: arm
    armv7l: arm
    aarch64: arm64
  system:
    Linux: linux
    Darwin: darwin

# Construct filename based on the system & architecture
kind_file: "kind-{{ kind_system }}-{{ kind_architecture }}"

# Version of the CLI to install
kind_version: latest

# Location/ownership/permissions of the binary
kind_path: /usr/local/bin/kind
kind_owner: root
kind_group: root
kind_mode: '0755'

# List of clusters to create
kind_cluster_names: []

# OS packages
kind_os_packages: []
kind_pip_packages:
  - kubernetes
  # - "oauthlib==3.2.2"
</pre></code>




## Example Playbook
### molecule/default/converge.yml
<pre><code>
- name: sample playbook for role 'kind'
  hosts: all
  become: "yes"
  vars:
    python_package_install_optional: True
    docker_compose_type: pip
    kind_cluster_names: [{'name': 'cluster1', 'ingress': True, 'roles': ['control-plane']}]
  roles:
    - deitkrachten.kubectl
  tasks:
    - name: Include role 'kind'
      ansible.builtin.include_role:
        name: kind
</pre></code>
