Role Name
=========

Run dovetail tests on undercloud.

Requirements
------------

This Ansible role allows to run dovetail tests from installed undercloud against overcloud.
Dovetail intends to define and provide a set of OPNFV related validation criteria that will provide input for the evaluation of the use of OPNFV trademarks, refer to https://wiki.opnfv.org/display/dovetail for details.

Role Variables
--------------

* `dovetail_url`: https://github.com/opnfv/dovetail.git
* `enable_debug`: true/false - whether to enable debug log
* `sut_type`: type of sut(system under test), i.e. "apex"
* `testsuite`: test suite name in dovetail, i.e. "compliance_set"
* `working_dir`: where to clone dovetail code
* `install_dovetail`: true/false - whether to run install-dovetail.yml in main task
* `pre_dovetail`: true/false - whether to run pre-dovetail.yml in main task
* `run_dovetail`: true/false - whether to run run-dovetail.yml in main task
* `post_dovetail`: true/flase - whether to run post-dovetail.yml in main task

Dependencies
------------

This Ansible role is designed to run from tripleo undercloud, make sure you have a undercloud ready for testing.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ---
    - hosts: localhost
      tasks:
       - name: Copy public key
         command: cat ~/.ssh/id_rsa.pub
         register: undercloud_ssh_key

       - name: Append public key to authorized_key
         lineinfile:
           dest: ~/.ssh/authorized_keys
           line: '{{ undercloud_ssh_key.stdout }}'
           create: yes

    - name: Run dovetail
      hosts: undercloud
      gather_facts: no
      roles:
       - ansible-role-tripleo-dovetail

Example Ansible host config
----------------

    [stack@undercloud ansible]$ cat hosts

    localhost   ansible_connection=local
    [undercloud]
    `your_undercloud_ip_address`
    [undercloud:vars]
    ansible_ssh_common_args='-o StrictHostKeyChecking=no'

License
-------

Apache 2.0

Author Information
------------------

