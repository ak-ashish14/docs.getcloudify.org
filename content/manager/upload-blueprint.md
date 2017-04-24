---
layout: bt_wiki
title: Uploading a Blueprint
category: Manager Intro
draft: false
weight: 400
---

Before Cloudify Manager can deploy your blueprint, you must upload it. You can upload a blueprint using the CLI or (for Premium users) the Cloudify Web interface.

Either use a blueprint that you have written or download an [example blueprint](https://github.com/cloudify-cosmo/cloudify-nodecellar-example) to upload.

## Uploading via the CLI

The Cloudify command-line interface provides two methods for uploading your blueprint to Cloudify Manager.

 * `publish-archive` enables you to upload a pre-packaged archive, for example *.tar, *.tar.gz, *.tar.bz, or *.zip.
 * `upload` enables you to specify a path to a blueprint file. Cloudify will manage compressing the folder and its contents.

The following is an example of `publish_archive`:
{{< gsHighlight  bash >}}
$ cfy blueprints publish-archive -l ARCHIVE_LOCATION -b BLUEPRINT_ID -n BLUEPRINT_FILENAME
...

...
{{< /gsHighlight >}}

The following is an example of `upload`:
{{< gsHighlight  bash >}}
$ cfy blueprints upload -b BLUEPRINT_ID -p BLUEPRINT_FILE_LOCATION
...

...
{{< /gsHighlight >}}


## Uploading a Blueprint via the Cloudify Web Interface

If you are a Premium user, you can upload a pre-packaged Blueprint archive, such as *.tar, *.tar.gz, *.tar.bz, *.zip., using the Cloudify Manager UI.

The **upload blueprint** button is located on the **Blueprints** tab of the UI.

![The blueprint upload button]({{< img "ui/ui_upload_blueprint_button.png" >}})

When you click **Upload Blueprint**, the upload dialog appears.

You can either specify the path to the blueprint archive, or select it from the filesystem by clicking the Add `+` button:

![The blueprint upload dialog]({{< img "ui/ui-upload-blueprint.png" >}})

The `Blueprint ID` field is mandatory.

The `Blueprint filename` field is optional and refers to the *.yaml file that contains the application topology. If left blank, the default `blueprint.yaml` file will be used. To override, specify the name of the YAML file to be used.

After all the mandatory fields are completed, the `Save` button becomes active.

![The user can enter a custom blueprint name]({{< img "ui/ui-upload-blueprint-with-input.png" >}})

After you click `Save`, the dialog box to be grayed-out until the blueprint file is fully uploaded to Cloudify. After the upload is complete, you are redirected to the blueprint's page.

## Uploading a Blueprint via the Command Line

The following scenario describes how to use the CLI to upload the Nodecellar blueprint.

If you have downloaded cloudify-nodecellar-example from github and want to use that specific blueprint for your IaaS, you can run one of the these:

  {{% gsInitTab %}}

  {{% gsTabContent "OpenStack" %}}
  {{< gsHighlight  bash >}}
  cfy blueprints upload -b nodecellar -p openstack-blueprint.yaml
  {{< /gsHighlight >}}
  {{% /gsTabContent %}}

  {{% gsTabContent "SoftLayer" %}}
  {{< gsHighlight  bash >}}
  cfy blueprints upload -b nodecellar -p softlayer-blueprint.yaml
  {{< /gsHighlight >}}
  {{% /gsTabContent %}}

  {{% gsTabContent "AWS EC2" %}}
  {{< gsHighlight  bash >}}
  cfy blueprints upload -b nodecellar -p aws-ec2-blueprint.yaml
  {{< /gsHighlight >}}
  {{% /gsTabContent %}}

  {{% gsTabContent "vCloud " %}}
  {{< gsHighlight  bash >}}
  cfy blueprints upload -b nodecellar -p vcloud-blueprint.yaml
  {{< /gsHighlight >}}
  {{% /gsTabContent %}}

  {{% /gsInitTab %}}


<br/>
The `-b` flag assigns a unique name to the blueprint on Cloudify Manager. Before creating a deployment, review this blueprint.

Navigate to the Cloudify Manager URL and refresh the screen. The nodecellar blueprint is displayed in the list.

  ![Blueprints table]({{< img "guide/quickstart/blueprints_table.png" >}})

Click the blueprint to view its topology.<br>
A topology consists of elements called _nodes_.

In this case, the following nodes exist:

  * Two VM's (one for mongo and one for nodejs)
  * A nodejs server
  * A MongoDB database
  * A nodejs application called nodecellar (which is a sample nodejs application backed by mongodb).

  ![Nodecellar Blueprint]({{< img "guide/quickstart-openstack/nodecellar_openstack_topology.png" >}})


# What's Next

You can now [deploy]({{< relref "manager/create-deployment.md" >}}) your blueprint.
