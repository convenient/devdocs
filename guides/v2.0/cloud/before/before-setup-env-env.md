---
layout: default
group: cloud
subgroup: 080_setup
title: Step 5, Clone or branch an environment
menu_title: Step 5, Clone or branch an environment
menu_order: 166
menu_node:
level3_menu_node: level3child
level3_subgroup: setupenv
version: 2.0
github_link: cloud/before/before-setup-env-env.md
---

{::options syntax_highlighter="rouge" /}


With your workspace configured, the project cloned, and cron set up, you can create a branch for development work.

You should also configure the {% glossarytooltip 18b930cf-09cc-47c9-a5e5-905f86c43f81 %}Magento Admin{% endglossarytooltip %} variables. For details, see [Add your local authentication keys to the project]({{ page.baseurl }}cloud/admin/admin-snap.html#add-keys-project) The new environment inherits the variable values from the master.

You can branch according to your own branching strategies. For example, you could create a branch to:

* Install and add Magento extensions and modules
* Create custom code and front end themes
* Modify and export configurations

After you create the branch, update project dependencies so you can install the Magento software locally.

1.	Do any of the following:

	*   To create a new environment, enter the following command:

			magento-cloud environment:branch <environment name> <parent environment ID>
	*   To check out an existing environment, enter the following command:

			magento-cloud environment:checkout

	For example, to create a new branch named `sprint1` from master, enter:

			magento-cloud environment:branch sprint1 master

3.	After the command completes, update dependencies:

		composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
4.  Create a [snapshot]({{page.baseurl}}cloud/admin/admin-snap.html) of the environment.

		magento-cloud snapshot:create -e <environment ID>

#### Next step
[Step 6, Install Magento]({{ page.baseurl }}cloud/before/before-setup-env-install.html)
