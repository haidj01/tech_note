### Terraform Quick Start


1. download Terraform 
```
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

2. Terraform Installation Confirmation
```
terraform -v
```

3. In your browser, download the macOS pkg 
file: https://awscli.amazonaws.com/AWSCLIV2.pkg

4. AWS Cli Installation Confirmation 
```
aws --version
```

5. Create new User with Programatic Accesss

6. AWS CLI Confiugration 
```
aws configure
```

7. Create provider.tf

```
terraform {
 required_providers {
   aws = {
     source  = "hashicorp/aws"
     version = "~> 4.19.0"
   }
 }
}
```

8. init terraform 

```
terraform init
```

9. create main.tf
```
resource "aws_instance" "my_vm" {
 ami                       = "ami-065deacbcaac64cf2" //Ubuntu AMI
 instance_type             = "t2.micro"

 tags = {
   Name = "My EC2 instance",
 }
}
```
** Find latest Amazon Linux 2 AMI
```
aws ec2 describe-images \
    --owners amazon \
    --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2" \
              "Name=state,Values=available" \
    --query 'Images | sort_by(@, &CreationDate) | [-1].{ImageId:ImageId,Name:Name}' \
    --region us-east-1

```

10. terraform format
```
terraform fmt
```
11. intialize
```
terraform init
```

12. Plan
```
terraform plan
```
13. apply
```
terraform apply
```
14. destrory
```
terraform destroy
```

** [Quick Start reference](https://spacelift.io/blog/terraform-tutorial)
