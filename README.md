<h1>Use Terraform Deploy an AWS EC2 Instance that you can SSH into</h1>

<img src="https://i.imgur.com/ygj8Nru.png" height="30%" width="85%" text-align="center" alt="Reference Architecture"/>

<h2>Overview</h2>
This lab demonstrates knowledge of the foundations of <b>Terraform and AWS Infrastructure</b>. In this lab, you will setup an <b>AWS IAM user</b> and assign permissions. You will learn <b>Terraform commands</b> like init, plan, apply, destory, and more and some flags. You will create providers for AWS. You will create a <b>VPC, Public Subnet, Internet Gateway, Routing Table with associations, Security Group, AMI, Key Pair, EC2 Instance, .tpl files, SSH configurations, Provisioners, Variables, Conditional Expressions, and Outputs</b>. 

<br>

<h2>main.tf</h2>
The main.tf is the main file where all the AWS resources are created. Each resource that is used is created using a resource block with the name of the resource in "" followed by the name you give this specific resource. In the {} are whatever values you need or want to specific when creating that resource. 
<br>
<br>
In this main.tf we list all the resources we want to create starting with the AWS VPC, then AWS Subnet, then AWS Internet Gateway, then AWS Routing Table, then create a default route to the Internet. Then the route table association between the route table and the subnet. Then the AWS Security Group with ingress and egress, then create a key pair called mtckey and store it in a folders called ".ssh" anywhere on your computer. Then create an AWS EC2 Instance using the free tier type "t2.micro". In the EC2 Instance we are going to include AMI id information that we will store in a block called data in the datasources.tf. Then in the EC2 Instance we will install Docker software using the script from the userdata.tpl file. Also inside the EC2 Instance block, we are adding a local-exec Provisioner which will run windows-ssh-config.tpl file (I am using Windows, Mac or Linux has a slightly different .tpl file) and configure the SSH host (Provisioner block works for Mac or Linux as well).

<h2>providers.tf</h2>
The providers.tf is the file that tells Terraform what service we are going to use. Terraform supports hundreds of programs from AWS, GCP, Kubernetes, etc. so this file specifies what we are using.
<br>
<br>
In this providers.tf we start with terraform block can put everything inside. Required_providers block tells Terraform its aws. The next block specifies more about AWS, it tells the region we will be in and the credentials for the account we will use using a file  called credentials that has the access keys that we will store in a folder called .aws anywhere on the computer. 

<h2>datasources.tf</h2>
The datasources.tf is where data about resources are stored. Data blocks are used. 
<br>
<br>
In this file data block we will store information about the AMI and call the block of information server_ami. Here we need to specify which AMI we are using by including the owners id and an AMI name (all information we can find from the AWS Console in EC2 instance AMIs).

<h2>variables.tf & terraform.tf.vars.tf</h2>
The variables.tf is where variables that will be used throughout the main.tf can be stored. Refer to the "Variable Defintion Precedence" section on the Input Variables page on the Terraform website. You will see that terraform.tfvars will take precendence over variables.tf.
<br>
<br>
In this lab the only reason there are two variables files is to test precendence in case of situations where multiple sets of variables need to be available for different users to add a flag to the command and use a different set of variables automatically. In the variables.tf there is a variables block for the variable "host_os". In the terraform.tfvars file change the host_os = "linux" or "mac" to see the precendence take effect. In the command line type "terraform console" then "var.host_os" and it will display either "linux" or "mac" instead of the default windows from the variables.tf. To override that while keeping both variable files follow up any command with "-var-file="variables.tf" then "var.host_os" and it will display the default from the variables.tf. 

<h2>outputs.tf</h2>
The outputs.tf file stores any information that can be displayed out to the console for the user to read. The file consists of an output block with the name of the block in "". Inside the {} is where the values are stored.
<br>
<br>
In this case, the only thing that I want to output to the console is the public ip address of the aws instance. Because I would not know the IP off the top of my head and cannot hardcode it, I put "the value of the dev_ip output is the public IP address of the aws instance called dev_node. 

<h2>.tpl</h2>
The .tpl extension is used to create template files. 
<h4>userdata.tpl</h4>
In this template file this is code to install docker.
<h4>windows-ssh-config.tpl</h4>
In this template file this is code to configure the windows SSH with identifying values.

<h2>Resources Used</h2>

- <b>YouTube:</b> [Learn Terraform (and AWS) by Building a Dev Environment – Full Course for Beginners](https://www.youtube.com/watch?v=iRaai1IBlB0&ab_channel=freeCodeCamp.org) <br>
- <b>YouTube:</b> [Terraform explained in 15 mins | Terraform Tutorial for Beginners](https://www.youtube.com/watch?v=l5k1ai_GBDE&t=3s&ab_channel=TechWorldwithNana) <br>
- <b>Terraform:</b> [Terraform Registry](https://developer.hashicorp.com/terraform/language/values/variables)
