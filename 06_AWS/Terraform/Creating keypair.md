---
created: 2026-06-23 00:17
updated:
tags:
  - aws
status: active
service-category:
source:
related:
---


> [!abstract]
> AWS services should be understood through architecture positioning and trade-offs, not memorization.

## Creating a keypair

```
resource "tls_private_key" "nautilus_kp" {
  algorithm = "RSA"
  rsa_bits  = 2048
}

resource "aws_key_pair" "nautilus_kp" {
  key_name   = "nautilus-kp"
  public_key = tls_private_key.nautilus_kp.public_key_openssh
}

resource "local_file" "private_key" {
  content         = tls_private_key.nautilus_kp.private_key_pem
  filename        = "/home/bob/nautilus-kp.pem"
  file_permission = "0400"
}
```

Private key will be stored in local machine, public key will be used for key pair. 

---

## To Run

```
cd /home/bob/terraform

terraform init

terraform plan

terraform apply -auto-approve
```