# ansible-control-server -> bastion-server -> private-ec2-server

## Work
- preliminarily check available ssh connection "bastion-server -> private-ec2-server" (security-group...etc)
- vi /etc/ansible/ansible.cfg following [ssh_connection] parts
~~~
[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=60s -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o ProxyCommand="ssh -W %h:%p -i <踏み台サーバの鍵ファイル> <踏み台サーバのユーザ>@<踏み台サーバのパブリックipアドレス> 22 -o 'StrictHostKeyChecking=no'" -i <対象サーバの鍵ファイル>
~~~
- vi inventory file
~~~
[test_server_ec2]
<対象サーバのプライベートipアドレス> ansible_ssh_user=ec2-user
~~~
- vi playbook file
~~~
---
- name: setup test
  hosts: all
  gather_facts: no
  tasks:
  - name: test task
    ansible.builtin.shell: |
      touch /tmp/test231016
~~~
- ex) \# ansible-playbook -i inventory.ini -vv playbook.yml
