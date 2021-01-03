toni.buildimages
===========

Ansible role to build docker images. Usefull if no CI/CD System like Gitlab is available.
Images can be build on the fly. Images will be deployed to ansible nodes by rsync and loaded by docker, so that they are available locally.
No docker registry is necessary.

Requirements
------------

Docker installed on host. 

Role Variables
--------------

```bash
  user_path:   # the user path where to store data during image building process. 
  images.namespace  # Namespace of registry where finished images will be referenced to
  images.name # Name of destination image
  images.tag # Tag of destination image
  images.basenamespace # Namespace of source image where the new image will be based on. (Namespace found in docker registry (p.e. DockerHub,...)
  images.basename # Name of source image
  images.basetag # Tag of source image
```

Dependencies
------------

Host with docker installed.

Example Playbook to create a customized Openresty Image
----------------------------------------------------

```yaml
- hosts: builder
  become: True
  gather_facts: yes
  roles:
    - {
        role : buildimages
      }
  vars:
    images:
      - { namespace: "registry.jidu.eu", name: "openresty_base", tag: "1.19.3.1", basenamespace: "", basename: "alpine", basetag: "3.12" }
      - { namespace: "registry.jidu.eu", name: "openresty_base_fat", tag: "1.19.3.1-alpine-fat", basenamespace: "", basename: "openresty_base", basetag: "1.19.3.1" }
      - { namespace: "registry.jidu.eu", name: "openresty", tag: "1.19.3.1-alpine-fat", basenamespace: "", basename: "openresty_base_fat", basetag: "1.19.3.1-alpine-fat" }
```

License
-------

MIT

Author Information
------------------

T.Nissen
