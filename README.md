# Server Key Management

## Prerequisites
- Controller-node registered as GitHub self-hosted runner
- Controller-node has SSH access to all nodes
- Ansible installed on the controller-node

## Adding a Key
Add to `group_vars/all/ssh_keys.yml`:
```yaml
ssh_public_keys:
  - name: "dev-john"
    key: "ssh-rsa AAAA..."
    state: present
```

## Removing a Key

Running the pipeline once with `state:absent` will remove the key from the nodes, next we can remove `ssh-keys.yml` file on the next commit.
```yaml
  - name: "dev-john"
    key: "ssh-rsa AAAA..."
    state: absent
```

## Running Manually
```bash
# dry run
ansible-playbook -i inventory/hosts.yaml playbook.yml --check --diff

# apply
ansible-playbook -i inventory/hosts.yaml playbook.yml -v
```

## Pipeline
- Auto triggers on push to `main` when `ssh_keys.yml` changes
- Runs on self-hosted runner