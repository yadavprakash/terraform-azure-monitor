# Terraform-azure-monitor

# Terraform Azure Cloud MONITOR Module

## Table of Contents
- [Introduction](#introduction)
- [Usage](#usage)
- [Examples](#examples)
- [Author](#author)
- [License](#license)
- [Inputs](#inputs)
- [Outputs](#outputs)

## Introduction
This module provides a Terraform configuration for deploying various Azure resources as part of your infrastructure. The configuration includes the deployment of resource groups, monitor.
## Usage
To use this module, you should have Terraform installed and configured for AZURE. This module provides the necessary Terraform configuration
for creating AZURE resources, and you can customize the inputs as needed. Below is an example of how to use this module:

# Examples

# Example: Default

```hcl
module "ampls" {
  source      = "git::https://github.com/yadavprakash/terraform-azure-monitor.git?ref=v1.0.0"
  name        = "appgyt"
  environment = "test"
  label_order = ["name", "environment"]

  resource_group_name = module.resource_group.resource_group_name
  location            = module.resource_group.resource_group_location
  linked_resource_ids = [module.log-analytics.workspace_id]
  subnet_id           = module.subnet.subnet_id[0]
  private_dns_zones_names = [
    "privatelink.agentsvc.azure-automation.net",
    "privatelink.blob.core.windows.net",
    "privatelink.monitor.azure.com",
    "privatelink.ods.opinsights.azure.com",
    "privatelink.oms.opinsights.azure.com",
  ]
}
```

# Example: Diff_subs

```hcl
module "ampls_diff_subs" {
  source      = "git::https://github.com/yadavprakash/terraform-azure-monitor.git?ref=v1.0.0"
  name        = "app-1"
  environment = "test-1"
  label_order = ["name", "environment"]

  diff_sub_resource_group_name = data.azurerm_resource_group.other_rg.name
  diff_sub_location            = module.resource_group.resource_group_location
  subnet_id                    = "****"

  azurerm_monitor_private_link_scope_id = "**"
  diff_sub                              = true
  alias_sub                             = "***"
  private_dns_zones_names = [
    "privatelink.agentsvc.azure-automation.net",
    "privatelink.blob.core.windows.net",
    "privatelink.monitor.azure.com",
    "privatelink.ods.opinsights.azure.com",
    "privatelink.oms.opinsights.azure.com",
  ]
}
```
This example demonstrates how to create various AZURE resources using the provided modules. Adjust the input values to suit your specific requirements.

## Examples
For detailed examples on how to use this module, please refer to the [examples](https://github.com/yadavprakash/terraform-azure-virtual-machine/blob/master/_example) directory within this repository.

## License
This Terraform module is provided under the **MIT** License. Please see the [LICENSE](https://github.com/yadavprakash/terraform-azure-virtual-machine/blob/master/LICENSE) file for more details.

## Author
Your Name
Replace **MIT** and **Cypik** with the appropriate license and your information. Feel free to expand this README with additional details or usage instructions as needed for your specific use case.

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.3.6 |
| <a name="requirement_azurerm"></a> [azurerm](#requirement\_azurerm) | >=3.0.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_azurerm"></a> [azurerm](#provider\_azurerm) | >=3.0.0 |
| <a name="provider_azurerm.peer"></a> [azurerm.peer](#provider\_azurerm.peer) | >=3.0.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_labels"></a> [labels](#module\_labels) | git::https://github.com/yadavprakash/terraform-azure-labels.git | v1.0.0 |

## Resources

| Name | Type |
|------|------|
| [azurerm_monitor_private_link_scope.main](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_private_link_scope) | resource |
| [azurerm_monitor_private_link_scoped_service.main](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_private_link_scoped_service) | resource |
| [azurerm_private_dns_zone.diff_sub](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_dns_zone) | resource |
| [azurerm_private_dns_zone.main](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_dns_zone) | resource |
| [azurerm_private_endpoint.diff_sub_pe](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint) | resource |
| [azurerm_private_endpoint.this](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_alias_sub"></a> [alias\_sub](#input\_alias\_sub) | Different subscription id for local provider(id of diff sub in which DNS zone is present). | `string` | `null` | no |
| <a name="input_ampls_enabled"></a> [ampls\_enabled](#input\_ampls\_enabled) | Set to false to prevent the module from creating ampls resource. | `bool` | `true` | no |
| <a name="input_azurerm_monitor_private_link_scope_id"></a> [azurerm\_monitor\_private\_link\_scope\_id](#input\_azurerm\_monitor\_private\_link\_scope\_id) | The id  of the monitor private link scope from which private dns will be created for it | `string` | `null` | no |
| <a name="input_diff_sub"></a> [diff\_sub](#input\_diff\_sub) | Flag to tell whether dns zone is in different sub or not. | `bool` | `false` | no |
| <a name="input_diff_sub_location"></a> [diff\_sub\_location](#input\_diff\_sub\_location) | Location where different subscription resource should be created. | `string` | `""` | no |
| <a name="input_diff_sub_resource_group_name"></a> [diff\_sub\_resource\_group\_name](#input\_diff\_sub\_resource\_group\_name) | A container that holds related different subscription resources for an Azure solution | `string` | `""` | no |
| <a name="input_enable_private_endpoint"></a> [enable\_private\_endpoint](#input\_enable\_private\_endpoint) | enable or disable private endpoint to storage account | `bool` | `true` | no |
| <a name="input_enabled"></a> [enabled](#input\_enabled) | Set to false to prevent the module from creating any resources. | `bool` | `true` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Environment (e.g. `prod`, `dev`, `staging`). | `string` | `""` | no |
| <a name="input_label_order"></a> [label\_order](#input\_label\_order) | Label order, e.g. `name`,`application`. | `list(any)` | `[]` | no |
| <a name="input_linked_resource_ids"></a> [linked\_resource\_ids](#input\_linked\_resource\_ids) | (Required) The name of the Azure Monitor Private Link Scoped Service. Changing this forces a new resource to be created. | `list(string)` | <pre>[<br>  ""<br>]</pre> | no |
| <a name="input_location"></a> [location](#input\_location) | Location where resource should be created. | `string` | `""` | no |
| <a name="input_managedby"></a> [managedby](#input\_managedby) | ManagedBy, eg 'yadavprakash'. | `string` | `"hello@yadavprakash.com"` | no |
| <a name="input_name"></a> [name](#input\_name) | Name  (e.g. `app` or `cluster`). | `string` | `""` | no |
| <a name="input_private_dns_zones_names"></a> [private\_dns\_zones\_names](#input\_private\_dns\_zones\_names) | The name of the private dns zones from which private dns will be created for AMPLS | `list(string)` | <pre>[<br>  ""<br>]</pre> | no |
| <a name="input_repository"></a> [repository](#input\_repository) | Terraform current module repo | `string` | `"https://github.com/yadavprakash/terraform-azure-subnet.git"` | no |
| <a name="input_resource_group_name"></a> [resource\_group\_name](#input\_resource\_group\_name) | A container that holds related resources for an Azure solution | `string` | `""` | no |
| <a name="input_subnet_id"></a> [subnet\_id](#input\_subnet\_id) | (Required) The ID of the Subnet from which Private IP Addresses will be allocated for this Private Endpoint. Changing this forces a new resource to be created. | `string` | `""` | no |

## Outputs

No outputs.
<!-- END_TF_DOCS -->