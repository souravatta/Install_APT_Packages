Install apt Packages
=====================

The playbook aims to install apt packages. The playbook will prompt to enter the packages which the user want to install on the hosts. It will take more than one package name, each separated by comma.

---

Requirements
------------
1) There should be ssh connection between the master node and all other host nodes.
2) Package name to be installed should be valid. Example: tree, npm, zip etc.

---

Role Variables
--------------

| Name               |                                                                         |
|--------------------|-------------------------------------------------------------------------|
| packages           | Store the name of packages to install in form of array, comma seperated |
| list_package       | Store the packages name after spliting                                  |

---

Example 1 Playbook
------------------

```yaml
- name: Install packages on nodes using apt-get
  hosts: all
  vars_prompt:
    - name: packages
      prompt: "Name the packages you want to install, separated by commas"
      private: no
  roles:
    - install-packages
```
---

Example 2 Playbook
------------------

```yaml
- name: Run apt-get update
  apt:
    update_cache: yes
  become: true
  become_method: sudo

- name: Install packages
  apt:
    name: "{{ item }}"
    state: present
  loop: "{{ list_package }}"
  become: true
  become_method: sudo
```
---

Usage
-----
1) After you clone this repo to your desktop, go to its root directory and change the files **ansible.cfg** and **ansiserver**.
2) In file **ansiserver**, add the hosts you want to run the playbook. 
3) In file **ansible.cfg** change the path of inventory file.
4) If the ssh user used to login hosts doesn't have ssh password, then run the playbook using `ansible-playbook deploy.yml`.
5) If the ssh user used to login hosts have ssh password, then run the playbook using `ansible-playbook deploy.yml -k`. It will prompt for `ssh password`.

If you want to check what are the changes playbook will make rather than making them, use `--check` with ansible-playbook to be execute.

Example: `ansible-playbook deploy.yml --check`

>You can check out the full description about `--check` [here](https://docs.ansible.com/ansible/latest/user_guide/playbooks_checkmode.html)
