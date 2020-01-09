Ansible Role - CloudWatch Agent
=========

Ansible Role that installs and configures the AWS CloudWatch Agent on a server.
Currently only tested against EC2 instances running Ubuntu Xenial (16.x).

Notes
------

The cloudwatch agent configuration wizard offers to use collectd by default. In our experience, however, more configuration would be needed to get collectd working (at least on Ubuntu Xenial). Therefore we have departed from defaults in our provided configs by removing it. If you want to use collectd, you'll probably need to install and configure it with another role or playbook. You can then use the `cloudwatch_agent_config_template` var to provide a custom cloudwatch agent config file that includes a collectd block like the following:

    "metrics_collected": {
      "collectd": {
        "metrics_aggregation_interval": 60
      },
      ...

https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-custom-metrics-collectd.html

Likewise StatsD:

    "metrics_collected": {
    ...
      "statsd": {
        "metrics_aggregation_interval": 60,
        "metrics_collection_interval": 10,
        "service_address": ":8125"
      },
    ...

https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-custom-metrics-statsd.html

Also, in order to avoid issues with Nitro (NVME) drive naming, we've added  `"drop_device": true` to our default disk metrics configs. Your cloudwatch alarms should therefore exclude the device dimension, unless you provide alternate configuration as described above.

https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-Configuration-File-Details.html#CloudWatch-Agent-Configuration-File-Metricssection

Finally, we are ignoring some common temporary file system types in the disk metrics as well.


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
    # only used if a custom config template is not provided as above
    cloudwatch_agent_detail_level: "basic"

    cloudwatch_agent_logs: []
    # - file_path: "/var/log/fail2ban.log"
    #   group_name: "fail2ban.log"
    #   stream_name: "{hostname}"

    # the cloudwatch agent installer creates a "cwagent" user under which we run
    # the agent by default. "root" or a user you have created and permissioned
    # elsewhere would also be an option.
    cloudwatch_agent_run_as_user: "cwagent"


Dependencies
------------

None


Example Playbook
----------------

See [tests](./tests/test.yml) for an example playbook


License
-------

CC0
