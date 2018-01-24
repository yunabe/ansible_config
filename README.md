# ansible_config

# Chromebook (crouton)
- `ansible-playbook -i localhost playbook.yml -K -c local`

# Misc
## Skip git repository rules
If `ansible-playbook` failed because some git repositories have pending repositories,
pass `--skip-tags git` to `ansible-playbook` to skip `git:` rules.
