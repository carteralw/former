# former
main.tf
variable "environment" {
  type    = string
  default = "dev"   # used if nothing is set in tfvars
}

variable "app_name" {
  type    = string
  default = "myapp"
}

locals {
  prefix = "${var.app_name}-${var.environment}"
}

resource "local_file" "example" {
  filename = "${path.module}/output.txt"
  content  = "Hello from ${local.prefix}"
}

tf.vars
environment = "prod"
app_name    = "greetingapp"
```

That's it. When you run `terraform apply`, Terraform automatically reads `terraform.tfvars` and the `output.txt` it creates will contain:
```
Hello from greetingapp-prod





main.tf
locals {
  # toset() converts the list to a set, which is what for_each requires
  files = toset(["1", "2", "3", "4", "5"])
}

resource "local_file" "hello" {
  for_each = local.files

  filename = "${path.module}/output-${each.key}.txt"
  content  = "Hello ${each.key}"
}
```

That creates:
```
output-1.txt  →  "Hello 1"
output-2.txt  →  "Hello 2"
output-3.txt  →  "Hello 3"
output-4.txt  →  "Hello 4"
output-5.txt  →  "Hello 5"






main.tf

locals {
  files = {
    "1" = "Hello One"
    "2" = "Hello Two"
    "3" = "Hello Three"
  }
}

resource "local_file" "hello" {
  for_each = local.files

  filename = "${path.module}/output-${each.key}.txt"
  content  = each.value   # now each file gets its own content
}
