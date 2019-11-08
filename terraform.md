# Terraform

#### Parameter

```text
resource "azurerm_template_deployment" "customers_api" {
  name                = "api_template"
  resource_group_name = "${var.api_management_rg_name}"

  template_body = <<DEPLOY
  ${file("${path.module}/api.json")}
DEPLOY

  parameters = {
    apimInstanceName = "${var.api_management_name}"
    backendName      = "${var.backend_name}"
  }

  deployment_mode = "Incremental"
}
```

