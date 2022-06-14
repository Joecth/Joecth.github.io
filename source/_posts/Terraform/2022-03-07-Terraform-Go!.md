---
layout: post
categories: 
date: 2019-12-01
tag: [] 

---





- terraform plan:
  not going to apply the changes, but going to show what changes it would make
- terraform apply
  to make those changes
- terraform plan -out changes.terraform --> terraform apply changes.terraform --> rm changes.terraform
- terraform destroy (CAREFUL in PRODUCTION)



### Var types:

- string, list, set, map, object(like map, but can have diff type), tuple(like list, but can have diff type)

- can let terraform decide on the type

  ```
  variable "a-string"{
    default = "I'm a string"
  }
  variable "a-list"{
    default = ["list of", "strings"]
  }
  ```

> Terraform is not fit to do configuration management on the software on your machines. Ansible/Chef/Puppet/Salt are better alternatives to do that. Terraform can work together with these tools to provide you CI on your machines. Terraform provides Configuration Management on an infrastructure level, not on the level of software of your machines.
>
> A tool to 
>
> 1. automate the provisioning of infrastructure
> 2. can keep a history of all infrastructure changes
> 3. can beused to collaborate in a team on infrastructure automation
> 4. keeps the remote state of my infrastructure

## Go! Runing Script

```bash
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:06:41]
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Finding latest version of hashicorp/aws...
- Installing hashicorp/aws v4.4.0...
- Installed hashicorp/aws v4.4.0 (signed by HashiCorp)

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
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:09:49]
$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions
are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                                  = "ami-0b0ea68c435eb488d"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + subnet_id                            = (known after apply)
      + tags_all                             = (known after apply)
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + vpc_security_group_ids               = (known after apply)

      + capacity_reservation_specification {
          + capacity_reservation_preference = (known after apply)

          + capacity_reservation_target {
              + capacity_reservation_id = (known after apply)
            }
        }

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + enclave_options {
          + enabled = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + metadata_options {
          + http_endpoint               = (known after apply)
          + http_put_response_hop_limit = (known after apply)
          + http_tokens                 = (known after apply)
          + instance_metadata_tags      = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.example: Creating...
aws_instance.example: Still creating... [10s elapsed]
aws_instance.example: Still creating... [20s elapsed]
aws_instance.example: Still creating... [30s elapsed]
aws_instance.example: Creation complete after 36s [id=i-0148e3795c780d26f]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:11:20]
$ terraform show
# aws_instance.example:
resource "aws_instance" "example" {
    ami                                  = "ami-0b0ea68c435eb488d"
    arn                                  = "arn:aws:ec2:us-east-1:713832322211:instance/i-0148e3795c780d26f"
    associate_public_ip_address          = true
    availability_zone                    = "us-east-1a"
    cpu_core_count                       = 1
    cpu_threads_per_core                 = 1
    disable_api_termination              = false
    ebs_optimized                        = false
    get_password_data                    = false
    hibernation                          = false
    id                                   = "i-0148e3795c780d26f"
    instance_initiated_shutdown_behavior = "stop"
    instance_state                       = "running"
    instance_type                        = "t2.micro"
    ipv6_address_count                   = 0
    ipv6_addresses                       = []
    monitoring                           = false
    primary_network_interface_id         = "eni-00262f65b89b3b9d6"
    private_dns                          = "ip-172-31-0-57.ec2.internal"
    private_ip                           = "172.31.0.57"
    public_dns                           = "ec2-3-80-98-101.compute-1.amazonaws.com"
    public_ip                            = "3.80.98.101"
    secondary_private_ips                = []
    security_groups                      = [
        "default",
    ]
    source_dest_check                    = true
    subnet_id                            = "subnet-9f6c0cd6"
    tags_all                             = {}
    tenancy                              = "default"
    vpc_security_group_ids               = [
        "sg-aa46f8d7",
    ]

    capacity_reservation_specification {
        capacity_reservation_preference = "open"
    }

    credit_specification {
        cpu_credits = "standard"
    }

    enclave_options {
        enabled = false
    }

    metadata_options {
        http_endpoint               = "enabled"
        http_put_response_hop_limit = 1
        http_tokens                 = "optional"
        instance_metadata_tags      = "disabled"
    }

    root_block_device {
        delete_on_termination = true
        device_name           = "/dev/sda1"
        encrypted             = false
        iops                  = 100
        tags                  = {}
        throughput            = 0
        volume_id             = "vol-0060a0c25d6420d9c"
        volume_size           = 8
        volume_type           = "gp2"
    }
}
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:12:45]
$ terraform destroy
aws_instance.example: Refreshing state... [id=i-0148e3795c780d26f]

Terraform used the selected providers to generate the following execution plan. Resource actions
are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # aws_instance.example will be destroyed
  - resource "aws_instance" "example" {
      - ami                                  = "ami-0b0ea68c435eb488d" -> null
      - arn                                  = "arn:aws:ec2:us-east-1:713832322211:instance/i-0148e3795c780d26f" -> null
      - associate_public_ip_address          = true -> null
      - availability_zone                    = "us-east-1a" -> null
      - cpu_core_count                       = 1 -> null
      - cpu_threads_per_core                 = 1 -> null
      - disable_api_termination              = false -> null
      - ebs_optimized                        = false -> null
      - get_password_data                    = false -> null
      - hibernation                          = false -> null
      - id                                   = "i-0148e3795c780d26f" -> null
      - instance_initiated_shutdown_behavior = "stop" -> null
      - instance_state                       = "running" -> null
      - instance_type                        = "t2.micro" -> null
      - ipv6_address_count                   = 0 -> null
      - ipv6_addresses                       = [] -> null
      - monitoring                           = false -> null
      - primary_network_interface_id         = "eni-00262f65b89b3b9d6" -> null
      - private_dns                          = "ip-172-31-0-57.ec2.internal" -> null
      - private_ip                           = "172.31.0.57" -> null
      - public_dns                           = "ec2-3-80-98-101.compute-1.amazonaws.com" -> null
      - public_ip                            = "3.80.98.101" -> null
      - secondary_private_ips                = [] -> null
      - security_groups                      = [
          - "default",
        ] -> null
      - source_dest_check                    = true -> null
      - subnet_id                            = "subnet-9f6c0cd6" -> null
      - tags                                 = {} -> null
      - tags_all                             = {} -> null
      - tenancy                              = "default" -> null
      - vpc_security_group_ids               = [
          - "sg-aa46f8d7",
        ] -> null

      - capacity_reservation_specification {
          - capacity_reservation_preference = "open" -> null
        }

      - credit_specification {
          - cpu_credits = "standard" -> null
        }

      - enclave_options {
          - enabled = false -> null
        }

      - metadata_options {
          - http_endpoint               = "enabled" -> null
          - http_put_response_hop_limit = 1 -> null
          - http_tokens                 = "optional" -> null
          - instance_metadata_tags      = "disabled" -> null
        }

      - root_block_device {
          - delete_on_termination = true -> null
          - device_name           = "/dev/sda1" -> null
          - encrypted             = false -> null
          - iops                  = 100 -> null
          - tags                  = {} -> null
          - throughput            = 0 -> null
          - volume_id             = "vol-0060a0c25d6420d9c" -> null
          - volume_size           = 8 -> null
          - volume_type           = "gp2" -> null
        }
    }

Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

aws_instance.example: Destroying... [id=i-0148e3795c780d26f]
aws_instance.example: Still destroying... [id=i-0148e3795c780d26f, 10s elapsed]
aws_instance.example: Still destroying... [id=i-0148e3795c780d26f, 20s elapsed]
aws_instance.example: Still destroying... [id=i-0148e3795c780d26f, 30s elapsed]
aws_instance.example: Destruction complete after 31s

Destroy complete! Resources: 1 destroyed.
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:16:35]
$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                                  = "ami-0b0ea68c435eb488d"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + subnet_id                            = (known after apply)
      + tags_all                             = (known after apply)
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + vpc_security_group_ids               = (known after apply)

      + capacity_reservation_specification {
          + capacity_reservation_preference = (known after apply)

          + capacity_reservation_target {
              + capacity_reservation_id = (known after apply)
            }
        }

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + enclave_options {
          + enabled = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + metadata_options {
          + http_endpoint               = (known after apply)
          + http_put_response_hop_limit = (known after apply)
          + http_tokens                 = (known after apply)
          + instance_metadata_tags      = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly
these actions if you run "terraform apply" now.
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:17:01]
$ ll
total 32
-rw-r--r--  1 joe  staff   253B Mar  7 18:06 instance.tf
-rw-r--r--  1 joe  staff   155B Mar  7 18:16 terraform.tfstate
-rw-r--r--  1 joe  staff   3.7K Mar  7 18:16 terraform.tfstate.backup
-rw-r--r--  1 joe  staff    46B Mar  7 16:29 versions.tf
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:17:52]
$ cat instance.tf
provider "aws" {
  access_key = "AKIA2MM5Y2SRYHHTGNLD"
  secret_key = "SBwhmGofEBJ8W90VsDWeW7wYXAcgtZrY9hQu8dAh"
  region     = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0b0ea68c435eb488d"
  instance_type = "t2.micro"
}

(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:18:05]
$ terraform plan -out out.terraform

Terraform used the selected providers to generate the following execution plan. Resource actions are
indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "example" {
      + ami                                  = "ami-0b0ea68c435eb488d"
      + arn                                  = (known after apply)
      + associate_public_ip_address          = (known after apply)
      + availability_zone                    = (known after apply)
      + cpu_core_count                       = (known after apply)
      + cpu_threads_per_core                 = (known after apply)
      + disable_api_termination              = (known after apply)
      + ebs_optimized                        = (known after apply)
      + get_password_data                    = false
      + host_id                              = (known after apply)
      + id                                   = (known after apply)
      + instance_initiated_shutdown_behavior = (known after apply)
      + instance_state                       = (known after apply)
      + instance_type                        = "t2.micro"
      + ipv6_address_count                   = (known after apply)
      + ipv6_addresses                       = (known after apply)
      + key_name                             = (known after apply)
      + monitoring                           = (known after apply)
      + outpost_arn                          = (known after apply)
      + password_data                        = (known after apply)
      + placement_group                      = (known after apply)
      + placement_partition_number           = (known after apply)
      + primary_network_interface_id         = (known after apply)
      + private_dns                          = (known after apply)
      + private_ip                           = (known after apply)
      + public_dns                           = (known after apply)
      + public_ip                            = (known after apply)
      + secondary_private_ips                = (known after apply)
      + security_groups                      = (known after apply)
      + source_dest_check                    = true
      + subnet_id                            = (known after apply)
      + tags_all                             = (known after apply)
      + tenancy                              = (known after apply)
      + user_data                            = (known after apply)
      + user_data_base64                     = (known after apply)
      + vpc_security_group_ids               = (known after apply)

      + capacity_reservation_specification {
          + capacity_reservation_preference = (known after apply)

          + capacity_reservation_target {
              + capacity_reservation_id = (known after apply)
            }
        }

      + ebs_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + snapshot_id           = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }

      + enclave_options {
          + enabled = (known after apply)
        }

      + ephemeral_block_device {
          + device_name  = (known after apply)
          + no_device    = (known after apply)
          + virtual_name = (known after apply)
        }

      + metadata_options {
          + http_endpoint               = (known after apply)
          + http_put_response_hop_limit = (known after apply)
          + http_tokens                 = (known after apply)
          + instance_metadata_tags      = (known after apply)
        }

      + network_interface {
          + delete_on_termination = (known after apply)
          + device_index          = (known after apply)
          + network_interface_id  = (known after apply)
        }

      + root_block_device {
          + delete_on_termination = (known after apply)
          + device_name           = (known after apply)
          + encrypted             = (known after apply)
          + iops                  = (known after apply)
          + kms_key_id            = (known after apply)
          + tags                  = (known after apply)
          + throughput            = (known after apply)
          + volume_id             = (known after apply)
          + volume_size           = (known after apply)
          + volume_type           = (known after apply)
        }
    }

Plan: 1 to add, 0 to change, 0 to destroy.

─────────────────────────────────────────────────────────────────────────────────────────────────────

Saved the plan to: out.terraform

To perform exactly these actions, run the following command to apply:
    terraform apply "out.terraform"
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:18:17]
$ cat out.terraform
H�gT	tfplanUT��%bdRMk[W}��^�RJ���J,K�*
                                         /���R.���M|��;�)��&� �
                                                               M v�c�$)�-��/h�MK�O�'�̻g�9w����?�F-�K�%�&8��<����n�έ��lm����{
�^���~�GO�h�=V���<��
                    R
tFrŦ��E�0��*���Y�Ȓ,ԣ�qU�K
   B��QӔtW��ⅎ�ҁQ��xYū�H&���u;�ѐ�dx��E�� ;�WԗX%U٠���t&9�X�B��Y�zm��
                                                                  >�y��F�
                                                                         ���/g(*BJ��FxwVS����|^�$�2�+��'�QES�����v� O�}5*Ս�0��Z
���|�����"���iPl�W�8��c���C\(.S]Xh������\:Ȣ>��w<I`�ߣ�*�<�Δ��H̺�Y9��O�-ht�E�84qUo�,�%�7�¬�����ikk�"S��h�+^��j�u�t׼=� ���uB���y�Ӌ����n�.�)4�QL�t������T?���,-O�kWE�\Ѓ׋l����HZ�6	�ʝ�M�*H8e�m��C�ѽi�V7wQd�:?=�uT*�s��A��\ypt|�ĽjZ��;�v�L����p4+��G�_��˃q��w�4h1����g?�
��ޖ�)4V��7�����P7L��H�gT	tfstateUT��%bD�M�� F�]��G�h��l��x1$��
                                                                     8i�w�s�|����rL&�$r!���+ F1*�=#E��es�5n��F�ܗ��%��S\�m��i1��1B~?R-�Z2X�]�T�cS��p
                                             ���P�=wM��H�gT
                                                           	tfstate-prevUT��%b��RPP*K-*���S�R0��KR����r�2J�z�z�J`��Ԣ��%+07'3/51="�_ZRPZR�d�P]
                                           (J-�/-JN	E�r�r��P�(w3awH�gT	tfconfig/m-/instance.tfUT��%bD��N�0�~��ރBh�=䐪��HP��u��Ǒ�!T�wG���gw����4��?@iM���!,�n�l��V��Z����
                                                                           L�S8R���ڥ{����r����j�^
                                                                                                 �
                                                                                                  o�����l�6^z��A\8pB�Cr��O���S����4! }+���5pZ��I�mJ�R��Ŕ�)��:~�þ�H���������P���p��H�gT	tfconfig/m-/versions.tfUT��%b�*I-*JL�/�U��RP(J-,�,JM�/K-*���S�UP��U0�34R����P[k2�4.H�gT	tfconfig/modules.jsonUT��%b��RP��RPPPP�N�T�RPRҁp]2�@\=%.�Z�X@��P��k))H�gT	.terraform.lock.hclUT��%bT�ߊ�E���\E�=|_���U]�
                                                                                                     s��"���.����7�f�1yf�(޻�����}(�����rIs9DZ.�A���1<��zz�uz8\�]�v��Y����nw�W�+=>�!�/���,��tY�rL�i}:Gzzt]��lϧ��㜶��\��u�/�[N��z�/�t~|��.���&��8_��1ݥ-�h�MJ/gqIw��MJ)m���:?�����~~���{~�����|�O�5������>��yw��������~߿�&�"[�qtg�ܲjkSx:���Ջ
                      @��� ��˜7�(:2M�*���͸�s +��(f�L�b�Yk��rK�9�9�
                                                                  �C��AA��"�4A8��j�1�{8����
                                                                                           +!j#�Ő�[�1z!�L����و��lrC����C+H�����@��0�q��ֺL�YC!g��
                                         �C��i5
���3�����o�\!#K�>F�����R+S�sBtu,&�Y����aeRE��mR�֪hL�0:�Vz�ݭ�����[�H��f�"#��[i�;LlU+F�d�Ф��YVV����!����j��t�b8k@��Â�պJ��+4�"H9��3�a�톚�0jiޫ����K�j}Be���V8�e4t��b^p�w�S?m�����P��⢛4H�gT7L��	tfplanUT��%H�gT�=wM��	$tfstateUT��%bH�gT�(w3aw
                                	�tfstate-prevUT��%bH�gT���p��	�tfconfig/m-/instance.tfUT��%bH�gT[k2�4.	�tfconfig/m-/versions.tfUT��%bH�gT��k))	%tfconfig/modules.jsonUT��%bH�gT��⢛4	�.terraform.lock.hclUT��%bPK�	%
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/first-steps on git:master x [18:19:27]
$ :q
```





## Basics

- <img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h02ill9uf8j213y0gsabh.jpg" alt="image-20220308153914158" style="zoom:67%;" />

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h02iomti6fj21440j6762.jpg" alt="image-20220308154212606" style="zoom: 50%;" />

```shell
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course on git:master x [15:43:18]
$ cd demo-1
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/demo-1 on git:master x [15:43:26]
$ cat instance.tf
resource "aws_instance" "example" {
  ami           = lookup(var.AMIS, var.AWS_REGION, "") # last parameter is the default value
  instance_type = "t2.micro"
}

(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/demo-1 on git:master x [15:43:49]
$ cat vars.tf
variable "AWS_ACCESS_KEY" {
}

variable "AWS_SECRET_KEY" {
}

variable "AWS_REGION" {
  default = "eu-west-1"
}

variable "AMIS" {
  type = map(string)
  default = {
    us-east-1 = "ami-13be557e"
    us-west-2 = "ami-06b94666"
    eu-west-1 = "ami-0d729a60"
  }
}

(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/demo-1 on git:master x [15:44:05]
$ cat provider.tf
provider "aws" {
  access_key = var.AWS_ACCESS_KEY
  secret_key = var.AWS_SECRET_KEY
  region     = var.AWS_REGION
}

(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/demo-1 on git:master x [15:44:51]
$ cat ../.gitignore
*/terraform.tfvars
*/terraform.tfstate
*/terraform.tfstate.backup
*/.terraform
*/mykey*
(base)
# joe @ J-M1-Pro-16 in ~/Codes_MO/terraform-course/demo-1 on git:master x [15:45:30]
$
```

#### Testing

`terraforom plan` is OK enough to test, no really need `terraform apply`

<img src="https://tva1.sinaimg.cn/large/e6c9d24egy1h02iu7akixj20x00c40um.jpg" alt="image-20220308154733683" style="zoom:33%;" />



## Software Provisioning

- Packer

5 mins for init

#### File uploads

