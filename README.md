AWS Solr Cloud Cluster
------------

This is an Ansible playbook and also guide on how to setup Solr Cloud on AWS in a clustered mode.

Requirements
------------
* Ansible v2.1
* Boto
* AWS IAM Profile with S3 Access
* AWS AutoScalingGroups
* AWS Route53 internal DNS
* External Zookeeper

This playbook assumes you have setup your AWS account and deployed the external zookeeper. For a guide on how to deploy Zookeeper please see the following link.

Playbook/Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ---
    - hosts: localhost
    connection: local
    become: true
    user: root
    gather_facts: yes
    vars:
      # DNS
      route_53_internal: true/false
      route_53_external: true/false
      int_dns_zone: { domain name }
      int_dns_record: { host name }
      int_dns_vpc: { specify vpc }
      int_dns_type: A
      int_dns_ttl: 60
      domain_name: "{{ int_dns_record }}.{{ int_dns_zone }}"
      # Solr_cloud
      config_set_download_external: true/false
      config_set_download_internal: true/false
      solr_config_update: true
      solr_upload: true
      solr_create: false        # Create the collections in the Solr gui to start with
      solr_create_cores: false  # Only use this if you have to create cores explictly
      zookeeper_dns_name_1: zookeeper-1.local
      zookeeper_dns_name_2: zookeeper-2.local
      zookeeper_dns_name_3: zookeeper-3.local
      # Common
      install_dev_tools: false
      update_hostname: true
      update_time_config: true
      hostname: "{{ int_dns_record }}.local"
      timezone: Europe/London
      restart_server: true # Server has to be restarted after changing the timezone config file
    roles:
      - dns
      - solr
      - solr_cloud
      - common

*Please note, we have included both examples of the master and the worker nodes playbooks in this repository.*