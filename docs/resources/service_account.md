---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "temporalcloud_service_account Resource - terraform-provider-temporalcloud"
subcategory: ""
description: |-
  Provisions a Temporal Cloud Service Account.
---

# temporalcloud_service_account (Resource)

Provisions a Temporal Cloud Service Account.

## Example Usage

```terraform
terraform {
  required_providers {
    temporalcloud = {
      source = "temporalio/temporalcloud"
    }
  }
}

provider "temporalcloud" {

}

resource "temporalcloud_namespace" "namespace" {
  name               = "terraform"
  regions            = ["aws-us-east-1"]
  accepted_client_ca = base64encode(file("${path.module}/ca.pem"))
  retention_days     = 14
}

resource "temporalcloud_service_account" "global_service_account" {
  name           = "admin"
  account_access = "Admin"
}

resource "temporalcloud_service_account" "namespace_admin" {
  name           = "developer"
  account_access = "Developer"
  namespace_accesses = [
    {
      namespace_id = temporalcloud_namespace.namespace.id
      permission   = "admin"
    }
  ]
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `account_access` (String) The role on the account. Must be one of [admin, developer, read] (case-insensitive)
- `name` (String) The name associated with the service account.

### Optional

- `namespace_accesses` (Attributes List) The list of namespace accesses. (see [below for nested schema](#nestedatt--namespace_accesses))
- `timeouts` (Block, Optional) (see [below for nested schema](#nestedblock--timeouts))

### Read-Only

- `id` (String) The unique identifier of the Service Account.
- `state` (String) The current state of the Service Account.

<a id="nestedatt--namespace_accesses"></a>
### Nested Schema for `namespace_accesses`

Required:

- `namespace_id` (String) The namespace to assign permissions to.
- `permission` (String) The permission to assign. Must be one of [admin, write, read] (case-insensitive)


<a id="nestedblock--timeouts"></a>
### Nested Schema for `timeouts`

Optional:

- `create` (String) A string that can be [parsed as a duration](https://pkg.go.dev/time#ParseDuration) consisting of numbers and unit suffixes, such as "30s" or "2h45m". Valid time units are "s" (seconds), "m" (minutes), "h" (hours).
- `delete` (String) A string that can be [parsed as a duration](https://pkg.go.dev/time#ParseDuration) consisting of numbers and unit suffixes, such as "30s" or "2h45m". Valid time units are "s" (seconds), "m" (minutes), "h" (hours). Setting a timeout for a Delete operation is only applicable if changes are saved into state before the destroy operation occurs.

## Import

Import is supported using the following syntax:

```shell
terraform import temporalcloud_service_account.terraform terraform.badf00d
```