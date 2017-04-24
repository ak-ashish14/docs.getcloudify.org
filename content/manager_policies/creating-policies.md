---
layout: bt_wiki
title: Creating Policies
category: Policies
draft: false
weight: 1000
---

{{% gsSummary %}}{{% /gsSummary %}}


{{% gsNote title="Note" %}}
This section is intended for advanced users. At the very least make sure you understand the [policies mechanism]({{< relref "manager_policies/overview.md" >}}) in general.
{{% /gsNote %}}


# Policy Types Blueprint Definition
Policy types are specified in the blueprint, together with a reference to their implementation and properties that configure instances of them.

Consider the following example. It is followed by an explanation.

{{< gsHighlight  yaml  >}}
policy_types:
  my_policy_type:
    source: my_policy/my_policy_type.clj
    properties:
      prop1:
        description: >
          prop1 is probably the most important property
          of my_policy_type
      prop2:
        description: >
          prop2 is a little less important, so we have a default
          for it
        default: 15.0
{{< /gsHighlight >}}

Policy types are defined under the `policy_types` section of the blueprint.

* The `source` attribute of a policy type can be either a relative path to a policy implementation within the blueprint directory or a URL to a policy hosted somewhere.
* The `properties` attribute of a policy type defines the policy's properties schema. These properties are configured when instantiating policies within the `groups` section. The next section describes how these properties can be used by a policy implementation.

    The following properties are built-in and common for all policies:

    * `policy_operates_on_group`:
        Specifies whether the policy must maintain its state for the entire group of nodes instances,
        or each node instance individually (Default: `false`).
    * `is_node_started_before_workflow`:
        Before triggering a workflow, checka if the node instance's state is started. This is a very important check that prevents the `heal` workflow from being executed after the built-in `uninstall` workflow takes place (Default: `true`).
    * `interval_between_workflows`:
        Triggers a workflow only if the last workflow was triggered earlier than `interval-between-workflows` seconds ago.
        If the specified value is less than 0, workflows can run concurrently. (Default: `300`)

# Policy Type Implementation
The implementation of policy types is written in [Clojure](http://clojure.org/), more specifically, using [Riemann's](http://riemann.io/) API with a thin layer provided by Cloudify.

An example: `my_policy/my_policy_type.clj`
{{< gsHighlight >}}
(where (metric {{prop2}})
  (with :state "{{prop1}}")
    process-policy-triggers)
{{< /gsHighlight >}}

Notice how `prop2` and `prop1` are referenced, using double curly braces. The implementation is actually a [`Jinja2`](http://jinja.pocoo.org/docs/dev/) template that is used to generate the actual implementation when the policy engine is started.

For each event with a metric value equal to the value of `prop2`, the event's `state` field is set to the value of `prop1`, and this event is delegated to the `process-policy-triggers` stream. In terms of Riemann, this is a very simple stream definition.

The `process-policy-triggers` stream executes all triggers that are specified for the instantiated policy in a deployment-dedicated thread pool. Executing the triggers in a separate thread pool is important, because the policies themselves must be non-blocking. If policies performed blocking operations, the entire deployment event stream would be blocked by each policy that performs them.

A related function is also provided, `check-restraints-and-process`, which checks the list of restraints before it processes triggers. There are two built-in restraints that can be deactivated in the blueprint if required (using policy properties `is_node_started_before_workflow` and `interval_between_workflows`). If both restraints return `true`, this function proceeds with `process-policy-triggers`.
