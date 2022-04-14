# terraform-create-ec2
Sample Terraform for creating AWS EC2 for development work

This Terraform code is used to create a simple EC2 server on AWS, which will be useful in case you need to create a server quickly to install your test environment; and then you can easily delete that server easily.

## 1. Tested version works
- Terraform: 1.1.8
- AWS-CLI: 1.18.x

## 2. How it works
- Terraform automatically creates a network on AWS with a VPC and a public subnet.
- Terraform automatically creates a 64-bit EC2 Ubuntu 20.04 LTS server in the public subnet created above.
- Terraform will add the SSH public key available in the `modules/ec2/keyfiles/id_rsa.pub` directory to the newly created EC2 server.
- Terraform will print to the terminal screen output information including: EC2 ID, EC2 public IP, VPC ID, subnet ID.

## 3. Clone repository and change necessary information

1. First, you need to generate an IAM key on AWS, install AWS-CLI on your computer. Then, please set up the IAM key above into AWS-CLI according to the [following link](https://peter-whyte.com/how-to-install-aws-cli-on-ubuntu-20-04/).
2. Clone this repository to your computer and open a terminal on this source terraform folder.
3. You change the values ​​according to your wishes in the `terraform.tfvars` file. In there:
    - `env`: the name of the environment you want to set, it will be used to attach the tags of resources created on AWS.
    - `region`: the region where you want to create EC2, you can select the region from [this list](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html).
    - `vpc_cidr`: network address for VPC.
    - `subet-cidrs`: network address for public subnet.
    - `instance_type`: ec2 type, you can choose the style from [this list](https://aws.amazon.com/ec2/instance-types/).
    - `volume_size`: size of the EBS volume that will be created for EC2.
4. Change the SSH public key file inside the `modules/ec2/keyfiles/id_rsa.pub` folder to yours.
5. Change the `shared_credentials_file` in the `main.tf` file with the value pointing to the path to the AWS-CLI setup credentials file on your computer.

## 4. Execute the terraform command

At the terraform repository directory, open the terminal:

- Type this command on the terminal so that it downloads the necessary modules and providers API to your computer.

    ```sh
    terraform init
    ```

- Type this command to terraform check the code for any problems, and will print to the screen the changes related to resources on AWS (such as creating networks and ec2).
    ```sh
    terraform plan
    ```

- Type this command to create EC2 instance on AWS as desired.
    After this command is done executing, the screen will show the newly created EC2 public IP. 
    ```sh
    terraform apply
    ```

    You can use the following command to ssh into the server:
    ```sh
    ssh -i id_rsa ubuntu@xxx.xxx.xxx.xxx
    ```
    With:
    - `id_rsa` is the SSH private key corresponding to the public key you changed before.
    - `xxx.xxx.xxx.xxx` is the EC's public IP printed to the screen, for example: 192.168.10.10.
- Type this command to delete EC2 and related resources after use.
    ```sh
    terraform destroy
    ```