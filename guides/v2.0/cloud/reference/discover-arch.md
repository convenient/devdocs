---
layout: default
group: cloud
subgroup: 065_reference
title: Architecture
menu_title: Architecture
menu_order: 1
menu_node: parent
version: 2.0
github_link: cloud/reference/discover-arch.md
---

Magento Enterprise Cloud Edition enables you to use the following three types of systems.

### Integration environment {#cloud-arch-int}
Developers use the Integration environment to develop, deploy, and test the Magento application, custom code, extensions, and services. This environment does not support all Magento ECE services, for example, Fastly is not accessible in Integration.

An Integration system runs in a Linux container (LXC). You can have up to eight active *environments* (Git branches) at a time to development and test code. You can have more than eight branches, but not all will be active to access, view, and test in Integration.

The process for developing in Integration requires the following process:

* Clone the master branch of Magento code from a Git repository
* Branch and develop in a new Git Git branch on your local workspace
* Push code to Git and the Integration environment for testing

Additional sections in this guide provide instructions and walk-throughs for setting up your local, working with Git branches, and deploying code.

### Staging environment {#cloud-arch-stage}
The Staging environment includes a system running on hardware similar to production. This environment includes all services and options used in Production, for example, Fastly, to provide a production-like environment for all testing. All code in Staging is read-only, requiring deploys of Git repositories.

Additional sections in this guide provide instructions and walk-throughs for final code deployments and testing production level interactions in a safe Staging environment.

We highly recommend fully testing every merchant and customer interaction in Staging prior to pushing to Production.

### Production environment {#cloud-arch-prod}
The Production environment runs your public-facing Magento single and multisite storefronts. This system include triple-redundant High Availability hardware for continuous access for your customers. This system is read-only, requiring deployment across the architecture from Integration to Staging and finally Production.

Additional sections in this guide provide walk-throughs for deploying to Production and Go Live requirements and processes.

We highly recommend fully testing in Staging prior to pushing to Production.

#### Advantage of redundant hardware
Rather than running a traditional active-passive master or master-slave setup, Magento Enterprise Cloud Edition runs a triple-redundant multimaster where all three instances accept reads and writes. This architecture offers zero downtime when scaling, and also provides guaranteed transactional integrity.

Because of our unique triple-redundant hardware, we can provide you with a set of three gateway servers. Most external services enable you to {% glossarytooltip 34f8f61d-2b48-4628-be06-aaa6e32ddc1f %}whitelist{% endglossarytooltip %} multiple IPs, so having more than one fixed IP isn't usually a problem.

These three gateways map to the three servers in your Magento Enterprise Cloud Edition cluster and retain permanent addresses.

Furthermore, Magento Enterprise Cloud Edition is fully redundant and highly available at every level:

*	DNS
*	{% glossarytooltip f83f1fa7-7a64-467b-b629-c2d0c25d2e7f %}Content Delivery Network{% endglossarytooltip %} (CDN)
*	Elastic load balancer (ELB)
*	Three-server cluster comprising all Magento services, including the database and web server.

#### Backup and disaster recovery
We automatically back up your production system every six hours. Each production system cluster is designed to withstand the loss of an entire server and all of the services running on it.

The coordinating agent that monitors your production system detects failures at the service level (for example, MySQL), and for all cases where an automated recovery is possible, fully automates and coordinates that recovery.

## Magento Enterprise Cloud Edition production technology stack
The following figure shows the technology used in a Magento Enterprise Cloud Edition production system

![Magento Enterprise Cloud Edition technology stack]({{ site.baseurl }}common/images/cloud_stack-diagram.png)

Magento Enterprise Cloud Edition seamlessly scales from the smallest six CPU cluster with 11.25GB of RAM to the largest 96 CPU cluster with 180GB of RAM. Our triple-redundant architecture means that upscaling can be conducted swiftly and without downtime: each of the three instances in the cluster is taken out of rotation in turn, upgraded to the new size and returned to rotation.

In addition, extra web servers can be added to an existing cluster should the constriction be at the {% glossarytooltip bf703ab1-ca4b-48f9-b2b7-16a81fd46e02 %}PHP{% endglossarytooltip %} level rather than the database level. This provides [*horizontal scaling*](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling){:target="_blank"} to complement the vertical scaling provided by extra CPUs on the database level.

## Projects {#cloud-arch-projects}
The container for your Magento application is a *project*. The project is your Magento store. Each project has one or more *environments*, which are Git branches that enable developers to work on new features or perform testing. Each environment is comprised of *services*, which are deployed inside highly restricted containers on a grid of servers.

Monitoring and failover happen automatically, behind the scenes.

<div class="bs-callout bs-callout-info" id="info">
  <p>Magento Enterprise Cloud Edition currently supports the following services: PHP, MySQL (MariaDB), Solr, Elasticsearch, Redis and RabbitMQ.</p>
</div>

## Services {#cloud-arch-services}
Each service runs in its own secure container; containers are managed together in the project.
Some services are built-in, such as the following:

*	HTTP router (handling incoming requests, but also caching and redirects)
*	PHP application server
*	Git
*	Secure Shell (SSH)

You can even have multiple applications running in the same project. Building
a microservice oriented architecture with Magento Enterprise Cloud Edition is
as easy as managing a monolithic application.

## Software versions used {#cloud-arch-software}
Magento Enterprise Cloud Edition uses:

*	Operating system: Debian GNU/Linux 8 (jessie)
*	Web server: {% glossarytooltip b14ef3d8-51fd-48fe-94df-ed069afb2cdc %}nginx{% endglossarytooltip %} 1.8

The preceding software is *not* upgradable but versions of [PHP]({{page.baseurl}}cloud/project/project-conf-files_magento-app.html), [MySQL]({{page.baseurl}}cloud/project/project-conf-files_services-mysql.html), [Solr]({{page.baseurl}}cloud/project/project-conf-files_services-solr.html), [Redis]({{page.baseurl}}cloud/project/project-conf-files_services-redis.html), [RabbitMQ]({{page.baseurl}}cloud/project/project-conf-files_services-rabbit.html), and [Elasticsearch]({{page.baseurl}}cloud/project/project-conf-files_services-elastic.html) are configurable.

#### Related topics
*	[Workflow]({{page.baseurl}}cloud/welcome/discover-workflow.html)
*	[Deployment process]({{page.baseurl}}cloud/reference/discover-deploy.html)
*	[Magento Enterprise Cloud Edition requirements]({{page.baseurl}}cloud/requirements/cloud-requirements.html)
