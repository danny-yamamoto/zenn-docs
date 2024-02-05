---
title: "About Composition as Code"
emoji: "⚙️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["cdktf", "terraform", "go", "googlecloud"]
published: true
---
CaC (Composition as Code) について。

https://www.infoq.com/jp/articles/cloud-computing-post-serverless-trends/?utm_campaign=infoq_content&utm_source=infoq&utm_medium=feed&utm_term=global

> Infrastructure as CodeからComposition as Codeへと移行する大きなトレンドがあり、開発者は使い慣れたプログラミング言語を使用して、より直感的なクラウドサービスの設定ができる。

アプリケーションとインフラの責任の境界が曖昧になる中で CaC は次の paradigm になるかもしれない。

Cloud Development Kit for Terraform (CDKTF)[^1] について unlearn する。

## Environment
- Devcontainer で構築。

https://github.com/danny-yamamoto/cdktf-gke/blob/8a19e51f69835ad3a21351ed1fb5187e8decb974/.devcontainer/devcontainer.json#L6-L10

```bash
vscode ➜ /workspaces/cdktf-gke (main) $ gcloud --version
Google Cloud SDK 462.0.1
alpha 2024.02.01
beta 2024.02.01
bq 2.0.101
core 2024.02.01
gcloud-crc32c 1.0.0
gsutil 5.27
kubectl 1.27.9
vscode ➜ /workspaces/cdktf-gke (main) $ terraform --version
Terraform v1.7.2
on linux_arm64
vscode ➜ /workspaces/cdktf-gke (main) $ cdktf --version
0.20.3
vscode ➜ /workspaces/cdktf-gke (main) $ 
```

## Getting started
- Plan (diff)

:::details cdktf diff

```bash
vscode ➜ /workspaces/cdktf-gke (main) $ cdktf diff
cdktf-gke  Initializing the backend...
cdktf-gke
cdktf-gke  Initializing provider plugins...
           - Reusing previous version of hashicorp/google from the dependency lock file
cdktf-gke  - Using previously-installed hashicorp/google v5.14.0

           Terraform has been successfully initialized!
cdktf-gke
           You may now begin working with Terraform. Try running "terraform plan" to see
           any changes that are required for your infrastructure. All Terraform commands
           should now work.

           If you ever set or change modules or backend configuration for Terraform,
           rerun this command to reinitialize your working directory. If you forget, other
           commands will detect it and remind you to do so if necessary.
cdktf-gke  Terraform used the selected providers to generate the following execution
           plan. Resource actions are indicated with the following symbols:
             + create

           Terraform will perform the following actions:
cdktf-gke    # google_compute_instance.test-vm (test-vm) will be created
             + resource "google_compute_instance" "test-vm" {
                 + allow_stopping_for_update = true
                 + can_ip_forward            = false
                 + cpu_platform              = (known after apply)
                 + current_status            = (known after apply)
                 + deletion_protection       = false
                 + effective_labels          = (known after apply)
                 + guest_accelerator         = (known after apply)
                 + id                        = (known after apply)
                 + instance_id               = (known after apply)
                 + label_fingerprint         = (known after apply)
                 + machine_type              = "e2-micro"
                 + metadata_fingerprint      = (known after apply)
                 + min_cpu_platform          = (known after apply)
                 + name                      = "test-vm"
                 + project                   = "hogehoge"
                 + self_link                 = (known after apply)
                 + tags_fingerprint          = (known after apply)
                 + terraform_labels          = (known after apply)
                 + zone                      = "asia-northeast1-b"

                 + boot_disk {
                     + auto_delete                = true
                     + device_name                = (known after apply)
                     + disk_encryption_key_sha256 = (known after apply)
                     + kms_key_self_link          = (known after apply)
                     + mode                       = "READ_WRITE"
                     + source                     = (known after apply)

                     + initialize_params {
                         + image                  = "projects/debian-cloud/global/images/family/debian-11"
                         + labels                 = (known after apply)
                         + provisioned_iops       = (known after apply)
                         + provisioned_throughput = (known after apply)
                         + size                   = 50
                         + type                   = "pd-standard"
                       }
                   }

                 + network_interface {
                     + internal_ipv6_prefix_length = (known after apply)
                     + ipv6_access_type            = (known after apply)
                     + ipv6_address                = (known after apply)
                     + name                        = (known after apply)
                     + network                     = "default"
                     + network_ip                  = (known after apply)
                     + stack_type                  = (known after apply)
                     + subnetwork                  = (known after apply)
                     + subnetwork_project          = (known after apply)
                   }
               }

           Plan: 1 to add, 0 to change, 0 to destroy.
           
           ─────────────────────────────────────────────────────────────────────────────

           Saved the plan to: plan

           To perform exactly these actions, run the following command to apply:
               terraform apply "plan"

vscode ➜ /workspaces/cdktf-gke (main) $
```

:::

- Apply

:::details cdktf deploy

```bash: log
vscode ➜ /workspaces/cdktf-gke (main) $ cdktf deploy
cdktf-gke  Initializing the backend...
cdktf-gke  Initializing provider plugins...
           - Reusing previous version of hashicorp/google from the dependency lock file
cdktf-gke  - Using previously-installed hashicorp/google v5.14.0

           Terraform has been successfully initialized!
           
           You may now begin working with Terraform. Try running "terraform plan" to see
           any changes that are required for your infrastructure. All Terraform commands
           should now work.

           If you ever set or change modules or backend configuration for Terraform,
           rerun this command to reinitialize your working directory. If you forget, other
           commands will detect it and remind you to do so if necessary.
cdktf-gke  Terraform used the selected providers to generate the following execution plan.
           Resource actions are indicated with the following symbols:
             + create

           Terraform will perform the following actions:
cdktf-gke    # google_compute_instance.test-vm (test-vm) will be created
             + resource "google_compute_instance" "test-vm" {
                 + allow_stopping_for_update = true
                 + can_ip_forward            = false
                 + cpu_platform              = (known after apply)
                 + current_status            = (known after apply)
                 + deletion_protection       = false
                 + effective_labels          = (known after apply)
                 + guest_accelerator         = (known after apply)
                 + id                        = (known after apply)
                 + instance_id               = (known after apply)
                 + label_fingerprint         = (known after apply)
                 + machine_type              = "e2-micro"
                 + metadata_fingerprint      = (known after apply)
                 + min_cpu_platform          = (known after apply)
                 + name                      = "test-vm"
                 + project                   = "hogehoge"
                 + self_link                 = (known after apply)
                 + tags_fingerprint          = (known after apply)
                 + terraform_labels          = (known after apply)
                 + zone                      = "asia-northeast1-b"

                 + boot_disk {
                     + auto_delete                = true
                     + device_name                = (known after apply)
                     + disk_encryption_key_sha256 = (known after apply)
                     + kms_key_self_link          = (known after apply)
                     + mode                       = "READ_WRITE"
                     + source                     = (known after apply)

                     + initialize_params {
                         + image                  = "projects/debian-cloud/global/images/family/debian-11"
                         + labels                 = (known after apply)
                         + provisioned_iops       = (known after apply)
                         + provisioned_throughput = (known after apply)
                         + size                   = 50
                         + type                   = "pd-standard"
                       }
                   }

                 + network_interface {
                     + internal_ipv6_prefix_length = (known after apply)
                     + ipv6_access_type            = (known after apply)
                     + ipv6_address                = (known after apply)
                     + name                        = (known after apply)
                     + network                     = "default"
                     + network_ip                  = (known after apply)
                     + stack_type                  = (known after apply)
                     + subnetwork                  = (known after apply)
                     + subnetwork_project          = (known after apply)
                   }
               }

           Plan: 1 to add, 0 to change, 0 to destroy.
           
           Do you want to perform these actions?
             Terraform will perform the actions described above.
             Only 'yes' will be accepted to approve.
cdktf-gke  Enter a value: yes
cdktf-gke  google_compute_instance.test-vm: Creating...
cdktf-gke  google_compute_instance.test-vm: Still creating... [10s elapsed]
cdktf-gke  google_compute_instance.test-vm: Creation complete after 14s [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm]
cdktf-gke
           Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

No outputs found.
vscode ➜ /workspaces/cdktf-gke (main) $
```

:::

- Apply (modify machine type)

:::details cdktf deploy

```bash
vscode ➜ /workspaces/cdktf-gke (main) $ cdktf deploy
cdktf-gke  Initializing the backend...
cdktf-gke  Initializing provider plugins...
           - Reusing previous version of hashicorp/google from the dependency lock file
cdktf-gke  - Using previously-installed hashicorp/google v5.14.0

           Terraform has been successfully initialized!
           
           You may now begin working with Terraform. Try running "terraform plan" to see
           any changes that are required for your infrastructure. All Terraform commands
           should now work.

           If you ever set or change modules or backend configuration for Terraform,
           rerun this command to reinitialize your working directory. If you forget, other
           commands will detect it and remind you to do so if necessary.
cdktf-gke  google_compute_instance.test-vm: Refreshing state... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm]
cdktf-gke  Terraform used the selected providers to generate the following execution plan.
           Resource actions are indicated with the following symbols:
             ~ update in-place

           Terraform will perform the following actions:
cdktf-gke    # google_compute_instance.test-vm (test-vm) will be updated in-place
             ~ resource "google_compute_instance" "test-vm" {
                   id                        = "projects/hogehoge/zones/asia-northeast1-b/instances/test-vm"
                 ~ machine_type              = "e2-micro" -> "e2-medium"
                   name                      = "test-vm"
                   tags                      = []
                   # (19 unchanged attributes hidden)

                   # (4 unchanged blocks hidden)
               }

           Plan: 0 to add, 1 to change, 0 to destroy.
           
           Do you want to perform these actions?
             Terraform will perform the actions described above.
             Only 'yes' will be accepted to approve.
cdktf-gke  Enter a value: yes
cdktf-gke
cdktf-gke  google_compute_instance.test-vm: Modifying... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm]
cdktf-gke  google_compute_instance.test-vm: Still modifying... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm, 10s elapsed]
cdktf-gke  google_compute_instance.test-vm: Still modifying... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm, 20s elapsed]
cdktf-gke  google_compute_instance.test-vm: Still modifying... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm, 30s elapsed]
cdktf-gke  google_compute_instance.test-vm: Still modifying... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm, 40s elapsed]
cdktf-gke  google_compute_instance.test-vm: Still modifying... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm, 50s elapsed]
cdktf-gke  google_compute_instance.test-vm: Still modifying... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm, 1m0s elapsed]
cdktf-gke  google_compute_instance.test-vm: Still modifying... [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm, 1m10s elapsed]
cdktf-gke  google_compute_instance.test-vm: Modifications complete after 1m18s [id=projects/hogehoge/zones/asia-northeast1-b/instances/test-vm]
cdktf-gke  
           Apply complete! Resources: 0 added, 1 changed, 0 destroyed.

No outputs found.
vscode ➜ /workspaces/cdktf-gke (main) $ 
```

:::

## Cost Calculation
- Infracost[^3] の無料 TRIAL が終了したのでコストは見れない。
- 無料 TRIAL は VS Code Extension のみ使える。
- `cdktf.out/stacks/xxx/` を `path` option に指定する。

:::details Infracost breakdown

```bash
vscode ➜ /workspaces/cdktf-gke (main) $ infracost breakdown --path cdktf.out/stacks/cdktf-gke/
Evaluating Terraform directory at cdktf.out/stacks/cdktf-gke/
  ✔ Downloading Terraform modules 
  ✔ Evaluating Terraform directory 
  ✔ Retrieving cloud prices to calculate costs 

Project: danny-yamamoto/cdktf-gke/cdktf.out/stacks/cdktf-gke

 Name                                         Monthly Qty  Unit   Monthly Cost 
                                                                               
 google_compute_instance.test-vm                                               
 └─ Instance usage (Linux/UNIX, on-demand, )          730  hours         $0.00 
                                                                               
 OVERALL TOTAL                                                           $0.00 
──────────────────────────────────
1 cloud resource was detected:
∙ 1 was estimated, it includes usage-based costs, see https://infracost.io/usage-file

┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━┓
┃ Project                                             ┃ Monthly cost ┃
┣━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━╋━━━━━━━━━━━━━━┫
┃ danny-yamamoto/cdktf-gke/cdktf.out/stacks/cdktf-gke ┃ $0.00        ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┻━━━━━━━━━━━━━━┛
vscode ➜ /workspaces/cdktf-gke (main) $
```

:::

## Composition Visualization[^4]
- `pluralith graph` command を `cdktf.out/stacks/xxx/` で実行する。

## Star History
- CDKTF は Pulumi[^2] に比べると認知度は落ちる。
- Pulumi に取り組む時が来ている。

[![Star History Chart](https://api.star-history.com/svg?repos=hashicorp/terraform-cdk,pulumi/pulumi&type=Date)](https://star-history.com/#hashicorp/terraform-cdk&pulumi/pulumi&Date)

## BTW
reviewer を担当していて感じることは実践的な知識が欠けていくこと。例えば Terraform の option など。

code を書きながら manage していく。

[^1]: https://developer.hashicorp.com/terraform/cdktf
[^2]: https://www.pulumi.com/
[^3]: https://www.infracost.io/
[^4]: https://www.pluralith.com/
