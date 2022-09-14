## Terraform Cheatsheet
| Syntax | Description |
| ----------- | ----------- |
| `terraform init` | initialize directory, pull down providers |
| `terraform apply` | apply changes (need to type yes to apply) |
| `terraform apply --auto-approve` | apply changes without being prompted to enter “yes” |
| `terraform destroy` | destroy/cleanup deployment |
| `terraform plan` | shows the end result of the file |
| `terraform refresh` | reconcile the state in Terraform state file with real-world resources |
| `terraform providers` | get information about providers used in current configuration |
| `terraform workspace list` | list out all workspaces |
| `terraform workspace new <workspace name>` | create a new workspace |
| `terraform workspace select <workspace name>` | change to the selected workspace |


