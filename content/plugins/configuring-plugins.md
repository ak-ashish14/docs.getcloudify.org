---
layout: bt_wiki
title: Configuring Plugins
category: Docs
draft: true
weight: 51

---

Your plugins must either be configured or be part of a blueprint in order for them to be processed during a workflow execution.

The recommended way of preparing your plugin is to save all the IaaS information in the Cloudify secret storage so that, when you deploy plugins, the information in secret storage is consumed by the plugin. Each out-of-box plugin provides examples of the IaaS data it requires and how to save that data in [secret storage]({{< relref "manager_architecture/security/#additional-security-information" >}}).

Alternatively, you can use a plugin's config file to save the IaaS configuration data. If you use this option, values such as passwords are saved as clear text in the configuration file. The properties for which you must specify values include such things as 
`server`, `image`, `flavor`, `management_network_name`, `use_password`, `use_external_resource`, `resource_id` and `openstack_config`. These specific properties are for the OpenStack plugin. You can find the required properties for your plugin in the relevant plugin documentation.