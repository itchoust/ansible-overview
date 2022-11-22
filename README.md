# Purpose
This is just for an Ansible overview.

# Credential
User: test

Password: test

# Steps

```bash
# Build the VM nodes image
docker build -f Dockerfile_node -t ansible-node .

# Run two nodes
docker run -d --name ansible-node1 -p 2222:22 ansible-node
docker run -d --name ansible-node2 -p 2223:22 ansible-node

# Get IP of each nodes
docker container inspect -f '{{ .NetworkSettings.IPAddress }}' ansible-node1
docker container inspect -f '{{ .NetworkSettings.IPAddress }}' ansible-node2

# Test SSH connection
ssh test@<node1-ip>
ssh test@<node2-ip>

# Edit host file to set node IPs
vi /etc/ansible/hosts

# Check if the controller can ping nodes
ansible --inventory hosts all -m ping -u test --ask-pass

# Execute simple playbook
ansible-playbook --inventory hosts -u test --ask-pass playbooks/my_first_playbook.yaml

# Execute playbook with template and handler
ansible-playbook --inventory hosts -u test --ask-pass --ask-become-pass playbooks/install-nginx.yaml

# Test with curl or in browser
curl http://172.17.0.2
curl http://172.17.0.3
```

