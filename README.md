# Ansible Control Node

*One control node to rule them all, One control node to find them, One control node to bring them all, and in the runtime bind them.*

## How to:

```
git clone git@bitbucket.org:marionete/ansible-controller.git
vagrant up
vagrant ssh controller
ansible-playbook /vagrant/playbooks/host_setup.yml -i /vagrant/.inventory
ansible-playbook --ask-vault-pass /vagrant/playbooks/services_setup.yml -i /vagrant/.inventory
```

Be happy.

Note:

After set up the control node, you might want to ssh into the controller machine and execute the setup playbook to manage the other nodes right away.
Note that for your first run; you need to manually login into the nodes to add the generated controller SSH key to 'known_hosts' on the managed nodes.

**Disabling the host key checking is not the way to go.**

More at: [host-key-checking](http://docs.ansible.com/ansible/intro_getting_started.html#host-key-checking)

If a new node is included. It must be included in the `inventory` file then run `ansible-playbook /vagrant/playbooks/host_setup.yml -i /vagrant/.inventory` to add the changes to `/etc/hosts`.
