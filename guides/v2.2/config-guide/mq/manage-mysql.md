---
layout: default
group: config-guide
subgroup: 15_RabbitMQ
title: Manage message queues
menu_title: Manage message queues
menu_order: 3
version: 2.0
github_link: config-guide/mq/manage-mysql.md
---


If you do not want to implement the RabbitMQ solution, you can manage message queues with cron jobs (or an external process manager)and the CLI to ensure that consumers are retrieving messages.

<h2>Process management</h2>

Cron jobs are the default mechanism to restart consumers. Processes started by `cron` consume the specified number of messages, then die after that. Re-running `cron` restarts the consumer.

A magic method, whose name is the same as the consumer name, is used as a callback when declaring new `consumer run` job in `crontab.xml`. Using magic methods allows you to pass the name of the consumer implicitly via method name. Alternatively, virtual types based on abstract consumer runner should be declared with concrete consumer name specified as an argument (this approach is more complex and required extra configuration).

The following shows a `crontab` group entry:

{% highlight xml %}
<group id="default">
    <job name="consumerCustomerCreatedListener" instance="Magento\MessageQueue\Model\ConsumerRunner" method="customerCreatedListener">
        <schedule>0 0 * * *</schedule>
    </job>
</group>
{% endhighlight %}

See <a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-cron.html">Configure and run cron</a> for more information about using cron with Magento.

You can also use a process manager such as <a href="http://supervisord.org/index.html">Supervisor</a> to monitor the status of processes. The manager can use the CLI to restart the processes as needed.

<h2>Command line interface</h2>

<h3>Start consumers</h3>

The CLI can be used to start consumers of the messages from the queue. Multiple consumers can be started at a same time.

`./bin/magento queue:consumers:start [--max-messages=<value>] [--batch-size=<value>] <consumer_name>`

where:

`<consumer_name>` is the consumer to start.

`--max-messages=<value>` defines the maximum number of messages to consume per invocation. If the number of queued messages is less than the specified maximum number of messages, if there are fewer than the specified number of messages in the queue, the consumer will poll for new messages until it has processed that number of messages. The command can be launched again with cron within a configured period of time, or manually.

If `--max-messages` is not specified, the process runs continuously.

`--batch-size=<value>` defines the number of messages to consume per batch. If specified, messages in a queue are consumed in batches of `<value>` each. This option is applicable for the batch consumer only.

If `--batch-size` is not defined, the batch consumer receives all available messages in a queue.

<h3>List consumers</h3>
Use the following command to return a list of message queue consumers:

` ./bin/magento queue:consumers:list`

#### Related Topics
*	<a href="{{page.baseurl}}config-guide/mq/rabbitmq-overview.html">RabbitMQ Overview</a>
*	<a href="{{page.baseurl}}config-guide/mq/config-mq.html">Configure message queues</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands-cron.html">Configure and run cron</a>
*	<a href="{{page.baseurl}}config-guide/cli/config-cli-subcommands.html">Command-line configuration</a>
