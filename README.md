# Minecraft Server using Infastruce as Code Setup Guide 

## Pre-requistes
  
  Install Terraform -> https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli
  
  Install AWSCLI -> https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
  
  Install Ansible -> sudo dnf install -y ansible
  
  ## Step 1 - Key Pair Setup
  
  Set up key pair in main minecraft-server directory
  
  Run `ssh-keygen -f "minecraft_key"`
  Hit enter twice to skip the passkey options.

## Step 2 - Terraform

  Before moving to the terraform directory
  
  Run `mv minecraft_key.pub terraform/minecraft.pub`
  
  This will put the public key in your terraform directory, which will make it accessible by the EC2 Instance created.
  
  Next use 'cd terraform' to enter the terraform directory
  
  Run 
  `
  terraform init
  ` and `terraform apply`
  then type `yes` when prompted
  
  Take note of the ip that terraform outputs
  
  Use `cd ..` to return to the main minecraft-server directory

## Step 3 - Ansible

Before moving into the ansible directory

Run `mv minecraft_key ansible/minecraft.pem`

This will put the private key in your ansible directory, which will make it accessible by the ansible when using ssh

Next, use `cd ansible` to enter the ansible directory

The first thing you will want to do is use `vim hosts` 

Here you will add a new line, using the template: 
`SERVER_IP ansible_user=ec2-user ansible_ssh_private_key_file=minecraft_key.pem`
Tip: Hit i to enter insert mode and when you're done, hit escape then type :wq to save the file

This will link ansible to the EC2 instance you just created using terraform.

Finally, run `ansible-playbook -i hosts playbook.yml`

then type `yes` when prompted

Wait for ansible to run through the tasks

## Step 4 - Connect!

Launch your minecraft client

Now, connect using the IP from terraform and a port of 25565, like this `x.x.x.x:25565`

Congrats! You did it! :)

# References

Terraform Documentation: https://developer.hashicorp.com/terraform/docs

Ansible Documentation: https://docs.ansible.com/

AWSCLI Documentation: https://docs.aws.amazon.com/cli/

Help from CS312 Discord Server & CS312 Lab Content 

Help/Ideas from ChatGPT on how to generate private keys and using ansible tasks. 



