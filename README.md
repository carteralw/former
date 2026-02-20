# former
main.tf

```hcl
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
```

```hcl
tf.vars
environment = "prod"
app_name    = "greetingapp"
```






main.tf
```hcl
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







main.tf
```hcl
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
```
