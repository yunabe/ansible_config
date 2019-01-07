# ansible_config

## Ubuntu Desktop on Parallels for Mac

```shell
ansible-playbook -i 192.168.64.1, parallels_ubuntu_desktop.yml --ask-pass --ask-become-pass
```

Notes:
- The comma after the IP addr tells to ansible-playbook command that the string is a list of hosts, not a filename.
- To avoid type passwords repeatedly, create a inventory file with `600` permission
  and write `192.168.64.5 ansible_become_pass=passwd` to the file.
  Then, pass the file with `-i` flag.

## Ubuntu instances on AWS/GCE
```shell
ansible-playbook cloud_ubuntu_desktop.yml -i 110.111.112.113,
```

## Chromebook (crouton)
```shell
ansible-playbook -i localhost playbook.yml -K -c local
```

# Not covered with these public Ansibe playbooks
## User registration
- npm
  - `npm adduser`
- docker
  - [`docker login`](https://docs.docker.com/engine/reference/commandline/login/)

## Enable Chrome Remote Desktop
Go to [this page on google.com](https://remotedesktop.google.com/headless) and get the command to enable CRD.
I found the page in [this post](https://groups.google.com/d/msg/gce-discussion/tN9oZs8xWps/b2PtOBTeAQAJ).
I couldn't find any official information from Google. But it's hosted on google.com.

```shell
/opt/google/chrome-remote-desktop/start-host --code="abcdefg" --redirect-url="https://remotedesktop.google.com/_/oauthredirect" --name=
```

I think this command restart CRD host automatically. But in case you can not see the new server in CRD client, try

```shell
sudo service chrome-remote-desktop restart
```

# Misc
## Skip git repository rules
If `ansible-playbook` failed because some git repositories have pending repositories,
pass `--skip-tags git` to `ansible-playbook` to skip `git:` rules.

# Ansible Tips
## inventory format
https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html

## "{{ var }}"

```
# OK
key: another_string {{ var }} # == "another_string {{ var }}"
# Not OK
key: {{ var }} another_string
```

References:
- https://docs.ansible.com/ansible/2.5/user_guide/playbooks_variables.html#hey-wait-a-yaml-gotcha
- https://docs.ansible.com/ansible/2.5/reference_appendices/YAMLSyntax.html#gotchas

## apt_key.id param
To get the value oto set `apt_key.id` param, install a key with `sudo apt-key add` then
find the key info with `apt-key list`.

```
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

Extract the last few sections of `pub` and remove white spaces to create a value of `apt_key.id` param
(e.g. `"8D81803C0EBFCD88"`, `"0EBFCD88"`).
