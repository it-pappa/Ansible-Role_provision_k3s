---

- hosts: k3s_cluster
  gather_facts: yes
  become: yes


# There is an reset task if you need to reset. Build new playbook for reset..