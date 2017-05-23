Role Name
=========

Run dovetail tests in undercloud.

Requirements
------------

This Ansible role allows to run dovetail tests from installed undercloud against overcloud.
Dovetail intends to define and provide a set of OPNFV related validation criteria that will provide input for the evaluation of the use of OPNFV trademarks, refer to https://wiki.opnfv.org/display/dovetail for details.

Role Variables
--------------

* `dovetail_url`: https://github.com/opnfv/dovetail.git
* `enable_debug`: true/false - whether to enable debug log
* `sut_type`: type of sut(system under test), i.e. 'apex', could be '' when testing third-party SUT.
* `testsuite`: test suite name in dovetail, i.e. "compliance_set", available testsuites are : debug, compliance_set, proposed_tests.
* `working_dir`: where to clone dovetail code and store dovetail test results
* `install_dovetail`: true/false - whether to run install-dovetail.yml in main task
* `pre_dovetail`: true/false - whether to run pre-dovetail.yml in main task
* `run_dovetail`: true/false - whether to run run-dovetail.yml in main task
* `post_dovetail`: true/flase - whether to run post-dovetail.yml in main task

Dependencies
------------

* This Ansible role is designed to run from tripleo undercloud against overcloud, make sure you have both undercloud & overcloud ready for testing.
* External network is needed to run dovetail cases, to create an external network in overcloud as below if not yet:

    ---
    - neutron net-create management --router:external
    - neutron subnet-create management x.x.x.x/y --name management_subnet



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
      hosts: localhost
      gather_facts: no
      roles:
       - ansible-role-tripleo-dovetail

Example Ansible host config
----------------

    [stack@undercloud ansible]$ cat hosts

    localhost   ansible_connection=local    ansible_ssh_common_args='-o StrictHostKeyChecking=no'

License
-------

Apache 2.0

Author Information
------------------

