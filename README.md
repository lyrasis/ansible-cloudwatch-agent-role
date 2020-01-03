Ansible Role - CloudWatch Agent
=========

Ansible Role that installs and configures the AWS CloudWatch Agent on a server.
Currently only tested against EC2 instances running Ubuntu Xenial (16.x).


Requirements
------------

None


Role Variables
--------------

    # will be constructed, if not provided
    cloudwatch_agent_url: ""

    # if not provided, a default template will be used, based on the detail level
    cloudwatch_agent_config_template: ""

    # basic | standard | advanced
    cloudwatch_agent_detail_level: "basic"

    cloudwatch_agent_logs: []
    # - file_path: "/var/log/fail2ban.log"
    #   group_name: "fail2ban.log"
    #   stream_name: "{hostname}"


Dependencies
------------

None


Example Playbook
----------------

See [tests](./tests/test.yml) for an example playbook


License
-------

CC0
