---
layout: default
category: Platforms - GCP
title: Secrets & Permissions
order: 2
permalink: /gcp_secrets.html
---

# GCP Secrets & Permissions Management

## Who Needs Permissions?

GCP entities typically fall into two categories:

* A user; a human being who has credentials to access and modify GCP resources
* A service account; an application or VM which needs credentials to access specific resources

Read about [service accounts here](https://cloud.google.com/iam/docs/service-accounts) and [user accounts here](https://cloud.google.com/iam/docs/quickstart).

## Google IAM

GCP Identity and Access Management (IAM) is the tool which manages access permissions between users and GCP resources (such as a database instance or a Cloud Function).

### IAM Roles 

Roles are used to grant entities (users, user groups, or [service accounts](https://cloud.google.com/iam/docs/service-accounts)) access to resources in a GCP project. This access level is associated with a **role**. Roles are fully customizable, but the common, predefined roles are:

* `roles/viewer`: Allows read-only access to the resource
* `roles/editor`: Allows read & write access to the resource
* `roles/owner`: Allows administrative rights to the resource, including administrating access by other entities.

There are additional permissions levels which allow entities access to specific resources, and many of these permissions are configured from GCP templates for specific products and project resources. An entity may have many roles.

Read more about GCP IAM roles in the [GCP documentation page](https://cloud.google.com/iam/docs/understanding-roles).

### IAM Policies

IAM policies are attached to GCP resources, and describe the access permissions that GCP entities have to this particular resource.

Each policy has one or more **bindings**, which describe the association between the entity and the resource.

Bindings consist of the following required fields:

* **Member**, which specifies an entity. A member could be a a user account, service account, Google group, or domain.
* **Role**, which specifies the access level the entity has to the resource.

Bindings can also include a [condition](https://cloud.google.com/iam/docs/conditions-overview), which is the part of the policy that grants granular access to specific products or resources. Conditions support conditional expressions, which become very powerful control mechanisms when combined.

Read more about IAM policies in the [GCP documentation page](https://cloud.google.com/iam/docs/policies).
