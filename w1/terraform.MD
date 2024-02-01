Commands
- init: get me the providers I need
- plan: what am I about to do?
- apply: do what is in the tf files         
- destroy: remove everything defined in the tf files


Terraform Google provider: https://registry.terraform.io/providers/hashicorp/google/latest/docs

## Setting up creds

### Manually

```bash
terraform fmt
```

### Using gcloud

```bash
export GOOGLE_CREDENTIALS=${HOME}'/work/src/github.com/prantoran/DEZoomcamp2024/w1/terraform/keys'
```

```bash
gcloud default auth-login
gcloud auth login
```


## init to get the provider
```bash
terraform init
```

```
Initializing the backend...

Initializing provider plugins...
- Finding hashicorp/google versions matching "5.14.0"...
- Installing hashicorp/google v5.14.0...
j- Installed hashicorp/google v5.14.0 (signed by HashiCorp)

Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

```bash
terraform plan
```

```
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # google_storage_bucket.auto-expire will be created
  + resource "google_storage_bucket" "auto-expire" {
      + effective_labels            = (known after apply)
      + force_destroy               = true
      + id                          = (known after apply)
      + location                    = "US"
      + name                        = "terraform-demo-412815-terra-bucket"
      + project                     = (known after apply)
      + public_access_prevention    = (known after apply)
      + rpo                         = (known after apply)
      + self_link                   = (known after apply)
      + storage_class               = "STANDARD"
      + terraform_labels            = (known after apply)
      + uniform_bucket_level_access = (known after apply)
      + url                         = (known after apply)

      + lifecycle_rule {
          + action {
              + type = "AbortIncompleteMultipartUpload"
            }
          + condition {
              + age                   = 1
              + matches_prefix        = []
              + matches_storage_class = []
              + matches_suffix        = []
              + with_state            = (known after apply)
            }
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform
apply" now.
```

```bash
terraform apply
```

```
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # google_storage_bucket.auto-expire will be created
  + resource "google_storage_bucket" "auto-expire" {
      + effective_labels            = (known after apply)
      + force_destroy               = true
      + id                          = (known after apply)
      + location                    = "US"
      + name                        = "terraform-demo-412815-terra-bucket"
      + project                     = (known after apply)
      + public_access_prevention    = (known after apply)
      + rpo                         = (known after apply)
      + self_link                   = (known after apply)
      + storage_class               = "STANDARD"
      + terraform_labels            = (known after apply)
      + uniform_bucket_level_access = (known after apply)
      + url                         = (known after apply)

      + lifecycle_rule {
          + action {
              + type = "AbortIncompleteMultipartUpload"
            }
          + condition {
              + age                   = 1
              + matches_prefix        = []
              + matches_storage_class = []
              + matches_suffix        = []
              + with_state            = (known after apply)
            }
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

google_storage_bucket.auto-expire: Creating...
google_storage_bucket.auto-expire: Creation complete after 3s [id=terraform-demo-412815-terra-bucket]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```


```bash
terraform destroy
```

```
google_storage_bucket.auto-expire: Refreshing state... [id=terraform-demo-412815-terra-bucket]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # google_storage_bucket.auto-expire will be destroyed
  - resource "google_storage_bucket" "auto-expire" {
      - default_event_based_hold    = false -> null
      - effective_labels            = {} -> null
      - enable_object_retention     = false -> null
      - force_destroy               = true -> null
      - id                          = "terraform-demo-412815-terra-bucket" -> null
      - labels                      = {} -> null
      - location                    = "US" -> null
      - name                        = "terraform-demo-412815-terra-bucket" -> null
      - project                     = "terraform-demo-412815" -> null
      - public_access_prevention    = "inherited" -> null
      - requester_pays              = false -> null
      - rpo                         = "DEFAULT" -> null
      - self_link                   = "https://www.googleapis.com/storage/v1/b/terraform-demo-412815-terra-bucket" -> null
      - storage_class               = "STANDARD" -> null
      - terraform_labels            = {} -> null
      - uniform_bucket_level_access = false -> null
      - url                         = "gs://terraform-demo-412815-terra-bucket" -> null

      - lifecycle_rule {
          - action {
              - type = "AbortIncompleteMultipartUpload" -> null
            }
          - condition {
              - age                        = 1 -> null
              - days_since_custom_time     = 0 -> null
              - days_since_noncurrent_time = 0 -> null
              - matches_prefix             = [] -> null
              - matches_storage_class      = [] -> null
              - matches_suffix             = [] -> null
              - no_age                     = false -> null
              - num_newer_versions         = 0 -> null
              - with_state                 = "ANY" -> null
            }
        }
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

google_storage_bucket.auto-expire: Destroying... [id=terraform-demo-412815-terra-bucket]
google_storage_bucket.auto-expire: Destruction complete after 2s

Destroy complete! Resources: 1 destroyed.
```
## Variables

### Terraform BigQuery dataset
https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/bigquery_dataset


### terraform apply

```bash
terraform apply
```
```
var.bq
  Enter a value: yes

google_storage_bucket.auto-expire: Refreshing state... [id=terraform-demo-412815-terra-bucket]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # google_bigquery_dataset.demo_dataset will be created
  + resource "google_bigquery_dataset" "demo_dataset" {
      + creation_time              = (known after apply)
      + dataset_id                 = "demo_dataset"
      + default_collation          = (known after apply)
      + delete_contents_on_destroy = false
      + effective_labels           = (known after apply)
      + etag                       = (known after apply)
      + id                         = (known after apply)
      + is_case_insensitive        = (known after apply)
      + last_modified_time         = (known after apply)
      + location                   = "US"
      + max_time_travel_hours      = (known after apply)
      + project                    = "terraform-demo-412815"
      + self_link                  = (known after apply)
      + storage_billing_model      = (known after apply)
      + terraform_labels           = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

google_bigquery_dataset.demo_dataset: Creating...
google_bigquery_dataset.demo_dataset: Creation complete after 2s [id=projects/terraform-demo-412815/datasets/demo_dataset]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```