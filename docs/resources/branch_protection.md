---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "gitlab_branch_protection Resource - terraform-provider-gitlab"
subcategory: ""
description: |-
  This resource allows you to protect a specific branch by an access level so that the user with less access level cannot Merge/Push to the branch.
  -> The allowed_to_push, allowed_to_merge and code_owner_approval_required arguments require a GitLab Premium account or above.  Please refer to Gitlab API documentation https://docs.gitlab.com/ee/api/protected_branches.html for further information.
---

# gitlab_branch_protection (Resource)

This resource allows you to protect a specific branch by an access level so that the user with less access level cannot Merge/Push to the branch.

-> The `allowed_to_push`, `allowed_to_merge` and `code_owner_approval_required` arguments require a GitLab Premium account or above.  Please refer to [Gitlab API documentation](https://docs.gitlab.com/ee/api/protected_branches.html) for further information.

## Example Usage

```terraform
resource "gitlab_branch_protection" "BranchProtect" {
  project                      = "12345"
  branch                       = "BranchProtected"
  push_access_level            = "developer"
  merge_access_level           = "developer"
  code_owner_approval_required = true
  allowed_to_push {
    user_id = 5
  }
  allowed_to_push {
    user_id = 521
  }
  allowed_to_merge {
    user_id = 15
  }
  allowed_to_merge {
    user_id = 37
  }
}

# Example using dynamic block
resource "gitlab_branch_protection" "main" {
  project            = "12345"
  branch             = "main"
  push_access_level  = "maintainer"
  merge_access_level = "maintainer"

  dynamic "allowed_to_push" {
    for_each = [50, 55, 60]
    content {
      user_id = allowed_to_push.value
    }
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- **branch** (String) Name of the branch.
- **merge_access_level** (String) Access levels allowed to merge. Valid values are: `no one`, `developer`, `maintainer`.
- **project** (String) The id of the project.
- **push_access_level** (String) Access levels allowed to push. Valid values are: `no one`, `developer`, `maintainer`.

### Optional

- **allowed_to_merge** (Block Set) Defines permissions for action. (see [below for nested schema](#nestedblock--allowed_to_merge))
- **allowed_to_push** (Block Set) Defines permissions for action. (see [below for nested schema](#nestedblock--allowed_to_push))
- **code_owner_approval_required** (Boolean) Can be set to true to require code owner approval before merging.
- **id** (String) The ID of this resource.

### Read-Only

- **branch_protection_id** (Number) The ID of the branch protection (not the branch name).

<a id="nestedblock--allowed_to_merge"></a>
### Nested Schema for `allowed_to_merge`

Optional:

- **group_id** (Number) The ID of a GitLab group allowed to perform the relevant action. Mutually exclusive with `user_id`.
- **user_id** (Number) The ID of a GitLab user allowed to perform the relevant action. Mutually exclusive with `group_id`.

Read-Only:

- **access_level** (String) Level of access.
- **access_level_description** (String) Readable description of level of access.


<a id="nestedblock--allowed_to_push"></a>
### Nested Schema for `allowed_to_push`

Optional:

- **group_id** (Number) The ID of a GitLab group allowed to perform the relevant action. Mutually exclusive with `user_id`.
- **user_id** (Number) The ID of a GitLab user allowed to perform the relevant action. Mutually exclusive with `group_id`.

Read-Only:

- **access_level** (String) Level of access.
- **access_level_description** (String) Readable description of level of access.

## Import

Import is supported using the following syntax:

```shell
# Gitlab protected branches can be imported with a key composed of `<project_id>:<branch>`, e.g.
terraform import gitlab_branch_protection.BranchProtect "12345:main"
```
