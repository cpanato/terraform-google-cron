# Cloud Run-based cron jobs.

This repository contains a terraform module for deploying cron jobs that run on a defined schedule.

## Defining a custom cron job.

A cron job can be defined as a simple Go program, with as little code as:

```go
import "log"

func main() {
    log.Println("hello")
}
```

> See our [example](./example/).

## Deploying a custom cron job

With the terraform module provided here, a probe can be deployed with a little
configuration as:

```terraform
module "prober" {
  source  = "chainguard-dev/cron/google"
  version = "v0.1.2"

  name       = "example"
  project_id = var.project_id

  importpath  = "github.com/chainguard-dev/terraform-google-cron/example"
  working_dir = path.module
}
```

> See our [example](./example/).

## Passing additional configuration

You can pass additional configuration to your custom cron jobs via environment
variables passed to the application. These can be specified in the module:

```terraform
  env = {
    "FOO" : "bar"
  }
```

> See our [example](./example/).

<!-- BEGIN_TF_DOCS -->
## Requirements

No requirements.

## Providers

| Name | Version |
|------|---------|
| <a name="provider_google"></a> [google](#provider\_google) | n/a |
| <a name="provider_ko"></a> [ko](#provider\_ko) | n/a |
| <a name="provider_random"></a> [random](#provider\_random) | n/a |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [google_cloud_run_service.probers](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/cloud_run_service) | resource |
| [google_cloud_run_service_iam_policy.noauths](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/cloud_run_service_iam_policy) | resource |
| [google_compute_backend_service.probers](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_backend_service) | resource |
| [google_compute_global_address.static_ip](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_global_address) | resource |
| [google_compute_global_forwarding_rule.forwarding_rule](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_global_forwarding_rule) | resource |
| [google_compute_managed_ssl_certificate.prober_cert](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_managed_ssl_certificate) | resource |
| [google_compute_region_network_endpoint_group.neg](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_region_network_endpoint_group) | resource |
| [google_compute_target_https_proxy.prober](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_target_https_proxy) | resource |
| [google_compute_url_map.probers](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_url_map) | resource |
| [google_dns_record_set.prober_dns](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/dns_record_set) | resource |
| [google_monitoring_uptime_check_config.global_uptime_check](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/monitoring_uptime_check_config) | resource |
| [google_monitoring_uptime_check_config.regional_uptime_check](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/monitoring_uptime_check_config) | resource |
| [google_service_account.prober](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/service_account) | resource |
| [ko_image.image](https://registry.terraform.io/providers/ko-build/ko/latest/docs/resources/image) | resource |
| [random_password.secret](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/password) | resource |
| [google_iam_policy.noauth](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/iam_policy) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_base_image"></a> [base\_image](#input\_base\_image) | The base image that will be used to build the container image. | `string` | `"cgr.dev/chainguard/static:latest-glibc"` | no |
| <a name="input_dns_zone"></a> [dns\_zone](#input\_dns\_zone) | The managed DNS zone in which to create prober record sets (required for multiple locations). | `string` | `""` | no |
| <a name="input_domain"></a> [domain](#input\_domain) | The domain of the environment to probe (required for multiple locations). | `string` | `""` | no |
| <a name="input_env"></a> [env](#input\_env) | A map of custom environment variables (e.g. key=value) | `map` | `{}` | no |
| <a name="input_importpath"></a> [importpath](#input\_importpath) | The import path that contains the prober application. | `string` | n/a | yes |
| <a name="input_locations"></a> [locations](#input\_locations) | Where to run the Cloud Run services. | `list(string)` | <pre>[<br>  "us-central1"<br>]</pre> | no |
| <a name="input_name"></a> [name](#input\_name) | Name to prefix to created resources. | `any` | n/a | yes |
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | The project that will host the prober. | `string` | n/a | yes |
| <a name="input_repository"></a> [repository](#input\_repository) | Container repository to publish images to. | `string` | `""` | no |
| <a name="input_working_dir"></a> [working\_dir](#input\_working\_dir) | The working directory that contains the importpath. | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_uptime_check"></a> [uptime\_check](#output\_uptime\_check) | n/a |
| <a name="output_uptime_check_name"></a> [uptime\_check\_name](#output\_uptime\_check\_name) | n/a |
<!-- END_TF_DOCS -->
