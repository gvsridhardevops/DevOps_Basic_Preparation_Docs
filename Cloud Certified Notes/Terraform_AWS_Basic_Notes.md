### Terraform :

        Terraform helps in IaC (Infrastructure as Code)
        From Terraform, Infrastructure can be created in almost all the virtual environments 
        (AWS, Azure, VmWare, VirtualBox ,Google Cloud etc)

		*****Terraform is not for physical servers.

		Terraform is developed in GO Language. 
		Like Packer, Terraform is also a single executable. (Download and set path)
		Terraform templates are written in Custom DSL (Domain Specific Language). 
		This DSL mostly looks like JSON.
		Terraform templates end with .tf extension

### Terraform in CD Pipelines : How Terraform is Actually Used

		1.Developers Commit code (write and save the code) to git .
		2.What ever code commit by developers to git will picked by Jenkins
				Hooks    : If Git is triggering jenkins  .
				Poll SCM : If Jenkins is asking from git .
		3.Build the Code to Package (Creating war/jar/tar/dll file) (Using Maven , MSBUILD , GULP , GRADDLE) .
		4.Basically will execute the unit tests on Package.(Basic Test Executions)
		5.Take the package and we need to build the application.
		We need to have Infrastructure for your application to work.
		If Infrastructure is not there , we create using Terraform 
		If Infrastructure is there (Present) , we go with Ansibile or Chef
		6. From Terraform , we do Provisioning using Chef or Ansibile / Shell scprit / power shell 
		7. And Application Deployed on Servers (Tomcat..etc) 
		
		Terraform is used to create an Environment , can be used where ever we want . 
		This Servers which we are creating can be used on any Virtual Platform or Cloud 
		
		******Terraform definetely used for every night build
		
### Installation of Terraform :

		### Windows:
		
				choco install terraform
				
				Open power shell and enter above command
				close power shell and reopen power shell to check version
				
				terraform --version
		
		### Linux:
		
				Download the terraform from here : https://www.terraform.io/downloads.html
			
				unzip the file to some folder. 
				sudo apt-get install unzip

				Copy Link address of Linux 64 bit .
				wget https://releases.hashicorp.com/terraform/0.12.24/terraform_0.12.24_linux_amd64.zip

				Add this folder to PATH environment variable (use export command and add this statement to /etc/environment or .bashrc)
				terraform version
				
				
### Concepts :

Terraform has list of providers supported. 
Each Provider provides resources and data sources.

### Provider: 

		Provider is used to create infrastructure in particular virtual environments. 
		Refer Here for the complete list. 

		https://www.terraform.io/docs/providers/index.html

		Some of the popular providers are
				AWS
				Azure
				OpenStack
				VmWare
	
### Resource: 

		Resource is part of the infrastructure created on the provider. 
		Every Provider provides resources.

		Ex : Network , Subnet , Virtual Machine

### DataSource: <will be defining soon. not now>

### Variables: 

		To give options for the user to enter different values to resources.

### Provisioners: 

		Terraform supports various provisioners like shell, Chef, Powershell

#######################################################################################################################

### Terraform Example for creating simple AWS Resource 

### Setup Terraform DEV Environment

		*** Install Terraform
		*** Install Visual Studio Code
		*** Install Terraform Extension into Visual Studio Code 

		In Packer file is the Input  
		In Terraform : Folder is the Input 

### Manual Approach :  Create a VPC (Virtual Private Cloud) in Oregon Region
		
### when we are working on cloud

		Service: What cloud provider is offering you (EC2)
		Resource: Anything created using Service     

		Login to AWS
		Click on Services : These are the services amazon is selling 
		Create a Network
		Search VPCS
		Your VPCS
		Create VPC
		
			Name Tag : Manual
			IPV4 CIDR Block : 10.0.0.0/16
					(10.10.X.X , Where X value from 0 to 255) 
			
			Click Create
		
		Now you can see newly created VPC Under your VPCS

		
### Using Terraform:  Create a VPC in oregon Region
			
			Create a new directory hello-tf as folder is input to terraform
			Create a new file called as main.tf
		
			mkdir hello-tf
			cd hello-tf

			touch main.tf
			vi main.tf
			
### To create resources in aws, use aws provider. 
			Provider syntax is
								provider 'provider type' 
								{
									arg1  'value1'
									...
									...
									argn  'valuen'
								}
								
			Using The above syntax and the documentation over here, lets fill the aws provider
								provider "aws" 
								{
									access_key = "<ACCESS-KEY>"
									secret_key = "<SECRET-KEY>"
									region = "us-west-2"
								}

			Looks like :

							provider "aws" 
								{
									access_key = "AKIAYXJA6GJ4APYEB37I"
									secret_key = "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"
									region     = "us-west-2"
								}

#### HOW TO GET NEW ACCESS KEY AND SECRET KEY
					IAM
					USERS
					CREATE USER
							USER NAME   : 
							ACCESS TYPE : Programmatic access
							ATTACH EXISTING POLICY DIRECTLY : ADMINISTRATION ACCESS

					FOR ACCESS KEY AND SECRET KEY 
							CLICK ON CREATED USER
							SECURITY CREDENTAILS
							DELETE ACCESS KEY AFTER WORK COMPLETED
							AND CLICK CREATE ACCESS KEY WHEN EVER KEY REQUIRED

#### Note: Inputs in Terraform are called as Arguments 
####       Outputs are called as Attributes. 
### Now we need to create a VPC Resource. 
			Search the Resources in Documentation page or just google ‘terraform resource <cloud> <resource>’
			Ex:  terraform resource aws vpc

			Resource Syntax is

							resource "type" "name" 
							{
								arg1  'value1'
								...
								...
								argn  'valuen'
							}
			
			Lets add the VPC resource details to existing main.tf file
			
							provider "aws" 
							{
								access_key = "<ACCESS-KEY>"
								secret_key = "<SECRET-KEY>"
								region = "us-west-2"
							}

							resource "aws_vpc" "myvpc" 
							{
								cidr_block = "192.168.0.0/16"

								tags = 
								{
									"Name" = "from-tf"
								}
							}
			
Now to execute this terraform tempalte,
 
Launch Powershell in the directory where terraform template is written
			**** terraform init . (.means current directory)
			**** terraform validate .
			**** terraform apply .
			
			Once you are done resource can be removed using
			**** terraform destroy .

### Explanation :

			1.  Terraform providers are not installed as part of terraform installations
				To get providers we need to execute init 

			2.  Validate means compiliation 

			Errors like :

			A block definition must have block content delimited by "{" and "}", 
			starting on the same line as the block header.

			3. Apply .

			    An execution plan has been generated and is shown below.
				Resource actions are indicated with the following symbols:
				+ create

				Terraform will perform the following actions:

				# aws_vpc.myvpc will be created
				+ resource "aws_vpc" "myvpc" {
					+ arn                              = (known after apply)
					+ assign_generated_ipv6_cidr_block = false
					+ cidr_block                       = "192.168.0.0/16"
					+ default_network_acl_id           = (known after apply)
					+ default_route_table_id           = (known after apply)
					+ default_security_group_id        = (known after apply)
					+ dhcp_options_id                  = (known after apply)
					+ enable_classiclink               = (known after apply)
					+ enable_classiclink_dns_support   = (known after apply)
					+ enable_dns_hostnames             = (known after apply)
					+ enable_dns_support               = true
					+ id                               = (known after apply)
					+ instance_tenancy                 = "default"
					+ ipv6_association_id              = (known after apply)
					+ ipv6_cidr_block                  = (known after apply)
					+ main_route_table_id              = (known after apply)
					+ owner_id                         = (known after apply)
					+ tags                             = {
						+ "name" = "from-tf"
						}
					}

				Plan: 1 to add, 0 to change, 0 to destroy.

				Do you want to perform these actions?
				Terraform will perform the actions described above.
				Only 'yes' will be accepted to approve.

				Enter a value: yes

				aws_vpc.myvpc: Creating...
				aws_vpc.myvpc: Still creating... [10s elapsed]
				aws_vpc.myvpc: Creation complete after 19s [id=vpc-039640741201b1775]

				Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

#### Made a mistake by giving Name as name 
Name not displayed so done the changes in tf-file and re-executing
				terraform validate .
				terraform apply 
				aws_vpc.myvpc: Refreshing state... [id=vpc-039640741201b1775]

				An execution plan has been generated and is shown below.
				Resource actions are indicated with the following symbols:
				~ update in-place

				Terraform will perform the following actions:

				# aws_vpc.myvpc will be updated in-place
				~ resource "aws_vpc" "myvpc" {
						arn                              = "arn:aws:ec2:us-west-2:599753896568:vpc/vpc-039640741201b1775"
						assign_generated_ipv6_cidr_block = false
						cidr_block                       = "192.168.0.0/16"
						default_network_acl_id           = "acl-05d778b295d4e169f"
						default_route_table_id           = "rtb-0d60d4eda16446ea8"
						default_security_group_id        = "sg-086a47d2631b9341b"
						dhcp_options_id                  = "dopt-52f57c2a"
						enable_classiclink               = false
						enable_classiclink_dns_support   = false
						enable_dns_hostnames             = false
						enable_dns_support               = true
						id                               = "vpc-039640741201b1775"
						instance_tenancy                 = "default"
						main_route_table_id              = "rtb-0d60d4eda16446ea8"
						owner_id                         = "599753896568"
					~ tags                             = {
						+ "Name" = "from-tf"
						- "name" = "from-tf" -> null
						}
					}

				Plan: 0 to add, 1 to change, 0 to destroy.

				Do you want to perform these actions?
				Terraform will perform the actions described above.
				Only 'yes' will be accepted to approve.

				Enter a value: yes

				aws_vpc.myvpc: Modifying... [id=vpc-039640741201b1775]
				aws_vpc.myvpc: Still modifying... [id=vpc-039640741201b1775, 10s elapsed]
				aws_vpc.myvpc: Modifications complete after 17s [id=vpc-039640741201b1775]

				Apply complete! Resources: 0 added, 1 changed, 0 destroyed.

#### Available Commands in Terraform 
			PS C:\Devops\Terraform\TerraformZone\terraform-tf> terraform --help
			Usage: terraform [-version] [-help] <command> [args]

			The available commands for execution are listed below.
			The most common, useful commands are shown first, followed by
			less common or more advanced commands. If you're just getting
			started with Terraform, stick with the common commands. For the
			other commands, please read the help and docs before usage.

			Common commands:
				apply              Builds or changes infrastructure
				console            Interactive console for Terraform interpolations
				destroy            Destroy Terraform-managed infrastructure
				env                Workspace management
				fmt                Rewrites config files to canonical format
				get                Download and install modules for the configuration
				graph              Create a visual graph of Terraform resources
				import             Import existing infrastructure into Terraform
				init               Initialize a Terraform working directory
				output             Read an output from a state file
				plan               Generate and show an execution plan
				providers          Prints a tree of the providers used in the configuration
				refresh            Update local state file against real resources
				show               Inspect Terraform state or plan
				taint              Manually mark a resource for recreation
				untaint            Manually unmark a resource as tainted
				validate           Validates the Terraform files
				version            Prints the Terraform version
				workspace          Workspace management

			All other commands:
				0.12upgrade        Rewrites pre-0.12 module source code for v0.12
				debug              Debug output management (experimental)
				force-unlock       Manually unlock the terraform state
				push               Obsolete command for Terraform Enterprise legacy (v1)
				state              Advanced state management



##############################################################################################################
#### Resource Dependencies :

When Resource A requires Resource B to be present (or already existing) this is called as Dependency
In Terraform terms you have to create resource B before resource A

To Demonstrate this lets add subnets to VPC. 
To create subnet resource vpc id is required (vpc has to be existing)

		First Create VPC 
		Second Create Subnet

### Resource Dependencies in terraform are used to tell in which order it need to executed
### By default , terraform create resources in the order which they are written

To create resource dependencies use the following expression
"${<resource-type>.<resource-name>.<attribute-name>}"
Ex : vpc_id = "${aws_vpc.myvpc.id}"

### Create Manual Subnet :

	Required Fields for creating Subnets are :
												cidr_block
												vpc_id
												availability_zone
												tags

If we apply the above expressions to create two subnets in the vpc

					provider "aws" {
						access_key = "<accesskey>"
						secret_key = "<secret-key>"
						region = "us-west-2"
					}
					
					resource "aws_vpc" "myvpc"{
						cidr_block = "192.168.0.0/16"
						tags = {
							"Name" = "from-tf"}
					}
					resource "aws_subnet" "subnet1" {
						vpc_id = "${aws_vpc.myvpc.id}"
						cidr_block = "192.168.0.0/24"
						availability_zone = "us-west-2a"

						tags = {
							"Name" = "subnet-1"
								}			
						}

						resource "aws_subnet" "subnet2" {
							vpc_id = "${aws_vpc.myvpc.id}"
							cidr_block = "192.168.1.0/24"
							availability_zone = "us-west-2c"
							tags = {
								"Name" = "subnet2"
								}
							
						}

### vpc_id = "aws_vpc.myvpc.id" in updated version like this it will work


Now Execute terraform apply to create infra and also observe the folder where terraform.tfstate is created 
where the information about created resources are stored.

#### Now Lets add one more subnet to existing template
					provider "aws" {
						access_key = "<accesskey>"
						secret_key = "<secret-key>"
						region = "us-west-2"
					}

					resource "aws_vpc" "myvpc"{
						cidr_block = "192.168.0.0/16"
						tags = {
							"Name" = "from-tf"}
					}
					resource "aws_subnet" "subnet1" {
						vpc_id = "${aws_vpc.myvpc.id}"
						cidr_block = "192.168.0.0/24"
						availability_zone = "us-west-2a"

						tags = {
							"Name" = "subnet-1"
								}			
						}

						resource "aws_subnet" "subnet2" {
							vpc_id = "${aws_vpc.myvpc.id}"
							cidr_block = "192.168.1.0/24"
							availability_zone = "us-west-2c"
							tags = {
								"Name" = "subnet2"
								}
							
						}

						resource "aws_subnet" "subnet3"{
							vpc_id = "${aws_vpc.myvpc.id}"
							cidr_block = "192.168.2.0/24"
							availability_zone = "us-west-2c"
							tags = {
								"Name" = "subnet3"
								}
						}

Now if you execute _terraform_apply_ command

Terraform creates two files tfstate and backup files
Terraform memory 
### .terraform folder where we will have the provider and will give the current status
### tfstate  : current state (what was created)
### backup   : There will be last successful configuration
### plan     : is a representation what has to be created


* Terraform will get the current status from provider (aws)  o/p : x
* It compares the status of provider with tfstate file,      o/p : x , Then no Issues
* if any differences are found they will be added for execution.  o/p : x ,  o/p : y

(Deleting from aws and executing terraform apply)
* during apply , plan will be created and compared to current status, 
* if plan is not matching the current state, then the changes will be added to terraforms execution.


#### Experiment Terraform

* By deleting one subnet from template

		output : Plan: 1 to add, 0 to change, 0 to destroy.
		Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

* change the tag value for one subnet

		output : Plan: 0 to add, 1 to change, 0 to destroy.
		Apply complete! Resources: 0 added, 1 changed, 0 destroyed.

change the avaliability zone for one subnet

		output : Plan: 1 to add, 0 to change, 1 to destroy.
		aws_subnet.subnet2: Destroying... [id=subnet-0c901f3e86e5648de]
		aws_subnet.subnet2: Destruction complete after 3s
		aws_subnet.subnet2: Creating...
		aws_subnet.subnet2: Creation complete after 7s [id=subnet-0554e5a572380755d
		Apply complete! Resources: 1 added, 0 changed, 1 destroyed.

##############################################################################################################
#### Terraform file organization :

		First terrafrom init creates .terraform folder , in which there is a provider which is installed
		when  terraform apply , if we doing it for the first time there is no state file 
		plan will created and check for the state , if no state exist , it will execute whole plan
		plan is what we what to create and state is what is present in the cloud right now
		now it create a .tfstate file in the local directory . (what terraform has created)

		terraform apply , it will refresh the state and it will get state what ever there in the cloud 
		and compare the cloud with .tfstate file .

		if plan and state are same , nothing happens
		if plan and state are not same , it will try to make both as same

		state : Reality
		plan : Expectation

Since writing every thing in one file looks complex and unreadable, 
Terraform allows you to write resources, providers etc in multiple .tf files

Remove already created main.tf file using rm -r main.tf

Lets organize the work into multiple files
touch provider.tf
touch network.tf

In Provider.tf add the following

				provider "aws" {
					access_key = "<ACCESS-KEY>"
					secret_key = "<SECRET-KEY>"
					region = "us-west-2"
				}


In network.tf add the following

				resource "aws_vpc" "myvpc" {
					cidr_block = "192.168.0.0/16"

					tags = {
						"Name" = "from-terraform"
					}
				}

				resource "aws_subnet" "subnet1" {
					cidr_block = "192.168.0.0/24"
					vpc_id = "${aws_vpc.myvpc.id}"
					availability_zone = "us-west-2a"
					tags = {
						"Name" = "subnet-1"
					}
				
				}

				resource "aws_subnet" "subnet2" {
					cidr_block = "192.168.1.0/24"
					availability_zone = "us-west-2b"
					tags = {
						"Name" = "subnet2"
					}
					vpc_id = "${aws_vpc.myvpc.id}"
				}

				resource "aws_subnet" "subnet3" {
				cidr_block = "192.168.2.0/24"
				availability_zone = "us-west-2c"
				tags = {
					"Name" = "subnet3"
				}
				vpc_id = "${aws_vpc.myvpc.id}"
				}


Now execute terraform apply .


### Adding some more stuff to existing network

### Manual Creation of Internet Gateway :

	Login to AWS
	VPC
	Internet -Gateways
	Create Internet -Gateway 
	Enter name-tag : Manual
	Click on create 

	Select newly created internet-gateway --> select Actions --> select Attach to vpc
	and select the vpc to attach --click ok

	Same way to detach the vpc and delete the Internet -Gateway

### Add internet gateway to vpc and a public route table. 

Create a new extendnetwork.tf file and add the following content to existing folder
			resource "aws_internet_gateway" "igw" {
				vpc_id = "${aws_vpc.myvpc.id}"

				tags = {
					"Name" = "from-terraform"
				}
			
			}

			resource "aws_route_table" "publicrt" {
				vpc_id = "${aws_vpc.myvpc.id}"

				route {
					cidr_block = "0.0.0.0/0"
					gateway_id = "${aws_internet_gateway.igw.id}"
				}

				tags = {
					"Name" = "public rt-tf"
				}
			}


### Lets create a plan by executing

			terraform plan -out="aws.plan" .
			Now execute the plan by using terraform apply "aws.plan" 

###########################################################################################################

#### Variable support to Terraform

Create a new file input.tf and add variables as described over here

				variable "accesskey" {
					type = "string"
				}

				variable "secretkey" {
				type = "string"
				}

				variable "region" {
					type = "string"
					default = "us-west-2"
				}
    
Now lets use these varaibles in the provider section

				provider "aws" {
					access_key = var.accesskey
					secret_key = var.secretkey
					region = var.region
				}

				for newer versions : 

				provider "aws" {
					access_key = "${var.accesskey}"
					secret_key = "${var.secretkey}"
					region     = "${var.region}"
				}


### Now Create a plan and extecute using
terraform plan -var 'accesskey=<your-access-key>' -var 'secretkey=<your-secret-key>' -out='aws.plan' .
terraform plan -var 'accesskey=AKIATBNUIHVQEZD7VGFU' -var 'secretkey=3ga8t9YR1eXzW6BwYWms26uN8goFdslVMlV8D076' -out='aws.plan' .
terraform apply aws.plan

Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

### Add variables for vpc cidr, subnet cidr

		variable "vpccidr" {
			type = "string"
			default = "192.168.0.0/16"
		}

		variable "subnet1cidr" {
			type = "string"
			default = "192.168.0.0/24"
		}


		variable "subnet2cidr" {
			type = "string"
			default = "192.168.1.0/24"
		}


		variable "subnet3cidr" {
			type = "string"
			default = "192.168.2.0/24"
		}

### Resources will be looking like
			resource "aws_vpc" "myvpc"{
				cidr_block = "${var.vpccidr}"
				tags = {
					"Name" = "from-terraform"}
			}

			resource "aws_subnet" "subnet1" {
				vpc_id = "${aws_vpc.myvpc.id}"
				cidr_block = "${var.subnet1cidr}"
				availability_zone = "us-west-2a"

				tags = {
					"Name" = "subnet-1"
						}			
				}

			resource "aws_subnet" "subnet2" {
				vpc_id = "${aws_vpc.myvpc.id}"
				cidr_block = "${var.subnet2cidr}"
				availability_zone = "us-west-2b"
				tags = {
					"Name" = "subnet-2"
					}
				
			}

			resource "aws_subnet" "subnet3"{
				vpc_id = "${aws_vpc.myvpc.id}"
				cidr_block = "${var.subnet3cidr}"
				availability_zone = "us-west-2c"
				tags = {
					"Name" = "subnet-3"
						}
			}

Note : Create -var -file  variable file 

##############################################################################################################

### terraform cli  --- command line application

### refresh: is used to update the local state (*.tfstate) with resources created.
terraform refresh -var 'accesskey=<youraccesskey>' -var 'secretkey=<your-secret-key>' .

		terraform refresh -var 'accesskey=AKIATBNUIHVQEZD7VGFU' -var 'secretkey=3ga8t9YR1eXzW6BwYWms26uN8goFdslVMlV8D076' .
		aws_vpc.myvpc: Refreshing state... (ID: vpc-06702850aaa9d4717)
		aws_subnet.subnet1: Refreshing state... (ID: subnet-043c9472f337f5d2b)
		aws_internet_gateway.igw: Refreshing state... (ID: igw-09f1de392dfe10fa0)
		aws_subnet.subnet3: Refreshing state... (ID: subnet-08e09f1d8d44d1dc6)
		aws_subnet.subnet2: Refreshing state... (ID: subnet-0ce00e3090558361f)
		aws_route_table.publicrt: Refreshing state... (ID: rtb-0007792beb926f0b1)

### taint: is used to mark resources for recreation during next apply
terraform taint <resource-type>.<resource-name>
terraform apply .

		# example

		#### terraform taint aws_subnet.subnet3
		#### terraform apply -var 'accesskey=<youraccesskey>' -var 'secretkey=<your-secret-key>' . 
		
		The resource aws_subnet.subnet3 in the module root has been marked as tainted!
		ubuntu@ip-172-31-27-250:~/hello-tf$ terraform apply -var 'accesskey=AKIATBNUIHVQEZD7VGFU' -var 'secretkey=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' .
		aws_vpc.myvpc: Refreshing state... (ID: vpc-06702850aaa9d4717)
		aws_subnet.subnet3: Refreshing state... (ID: subnet-08e09f1d8d44d1dc6)
		aws_subnet.subnet1: Refreshing state... (ID: subnet-043c9472f337f5d2b)
		aws_subnet.subnet2: Refreshing state... (ID: subnet-0ce00e3090558361f)
		aws_internet_gateway.igw: Refreshing state... (ID: igw-09f1de392dfe10fa0)
		aws_route_table.publicrt: Refreshing state... (ID: rtb-0007792beb926f0b1)

		An execution plan has been generated and is shown below.
		Resource actions are indicated with the following symbols:
		-/+ destroy and then create replacement

		Terraform will perform the following actions:

		-/+ aws_subnet.subnet3 (tainted) (new resource required)
			id:                              "subnet-08e09f1d8d44d1dc6" => <computed> (forces new resource)
			arn:                             "arn:aws:ec2:us-west-2:209220812128:subnet/subnet-08e09f1d8d44d1dc6" => <computed>
			assign_ipv6_address_on_creation: "false" => "false"
			availability_zone:               "us-west-2c" => "us-west-2c"
			availability_zone_id:            "usw2-az3" => <computed>
			cidr_block:                      "192.168.2.0/24" => "192.168.2.0/24"
			ipv6_cidr_block:                 "" => <computed>
			ipv6_cidr_block_association_id:  "" => <computed>
			map_public_ip_on_launch:         "false" => "false"
			owner_id:                        "209220812128" => <computed>
			tags.%:                          "1" => "1"
			tags.Name:                       "subnet-3" => "subnet-3"
			vpc_id:                          "vpc-06702850aaa9d4717" => "vpc-06702850aaa9d4717"


		Plan: 1 to add, 0 to change, 1 to destroy.

		Do you want to perform these actions?
		Terraform will perform the actions described above.
		Only 'yes' will be accepted to approve.

		Enter a value: yes

		aws_subnet.subnet3: Destroying... (ID: subnet-08e09f1d8d44d1dc6)
		aws_subnet.subnet3: Destruction complete after 1s
		aws_subnet.subnet3: Creating...
		arn:                             "" => "<computed>"
		assign_ipv6_address_on_creation: "" => "false"
		availability_zone:               "" => "us-west-2c"
		availability_zone_id:            "" => "<computed>"
		cidr_block:                      "" => "192.168.2.0/24"
		ipv6_cidr_block:                 "" => "<computed>"
		ipv6_cidr_block_association_id:  "" => "<computed>"
		map_public_ip_on_launch:         "" => "false"
		owner_id:                        "" => "<computed>"
		tags.%:                          "" => "1"
		tags.Name:                       "" => "subnet-3"
		vpc_id:                          "" => "vpc-06702850aaa9d4717"
		aws_subnet.subnet3: Creation complete after 0s (ID: subnet-0f899044a6fec8597)

		Apply complete! Resources: 1 added, 0 changed, 1 destroyed.


*** when ever we done apply , three things happened everytime : refresh , plan and apply

How do i mark individual resources for delete : for that we have to change the template
if we want individual resources for recreate , we use taint

### untaint: if you have tainted any resource by mistake/for any other reason and if you want it to be removed from taint then use untaint.

		ubuntu@ip-172-31-27-250:~/hello-tf$ terraform untaint aws_subnet.subnet3
		The resource aws_subnet.subnet3 in the module root has been successfully untainted!

## Graph: Generate a graph from your template. 
			Ensure you install dot from graphviz. Refer Here
			choco install graphviz -y

			In Linux :
			sudo apt-get update
			sudo apt-get install graphviz

			terraform graph | dot -Tsvg > graph.svg


### Terraform outputs: Output is result of the infra provisioning which can be shared

To Create outputs, create a new file called as outputs.tf with following content
				output "vpc-id" {
				value = "${aws_vpc.myvpc.id}"
				}

				output "subnet1-id" {
				value = "${aws_subnet.subnet1.id}"
				}

Now execute terraform apply and observe the output

			ubuntu@ip-172-31-27-250:~/hello-tf$ terraform apply .
			var.accesskey
			Enter a value: AKIATBNUIHVQEZD7VGFU

			var.secretkey
			Enter a value: 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

			aws_vpc.myvpc: Refreshing state... (ID: vpc-06702850aaa9d4717)
			aws_subnet.subnet1: Refreshing state... (ID: subnet-043c9472f337f5d2b)
			aws_subnet.subnet3: Refreshing state... (ID: subnet-0f899044a6fec8597)
			aws_subnet.subnet2: Refreshing state... (ID: subnet-0ce00e3090558361f)
			aws_internet_gateway.igw: Refreshing state... (ID: igw-09f1de392dfe10fa0)
			aws_route_table.publicrt: Refreshing state... (ID: rtb-0007792beb926f0b1)

			Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

			Outputs:

			subnet1-id = subnet-043c9472f337f5d2b
			vpc-id = vpc-06702850aaa9d4717

### Terraform modules: Module is reusable terraform configuration.
Terraform community shares many modules for reuse in Terraform Registry : https://registry.terraform.io/
Terraform templates written by us can also be reused as modules.
Using Terraform module from Local folder

Create a new directory called as moduledemo

In the module demo create one file main.tf with following content
			module "<name>" {
			source  = "../hello-tf"
			accesskey = ""
			secretkey = ""
			}

				module "moduledemo" {
				source  = "../hello-tf"
				accesskey = "AKIATBNUIHVQEZD7VGFU"
				secretkey = "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"
				}
	
Now execute
	terraform init
	terraform apply .

In hello-tf all the variables will be arguments to module and all the outputs will be attributes of module.
			Any varaible without default is required argument 
			Any variable with default is optional argument.

###################################################################################################################
Next Sections
Terraform Registry example
Terraform remote-state
terraform env and workspaces
Terraform import
Terraform example with Azure.

#################################################################################################################

### Data-Sources :

For creating Ec2 machine, we need to provide

			subnet-id
			security group
			key value pair

One approach is : create every thing and use the attributes.

If we want to use existing subnet-id, security group and key-value pair, we need to know ids, 
For this : terraform has data sources which can query the information from providers. (Ex : Select Query)

### Lets create a simple data source to pull the information of default subnet and create a new subnet

Create a new directory called as datasourcedemo
Create a file main.tf

In main.tf

				provider "aws" {
					access_key = "${var.accesskey}"
					secret_key = "${var.secretkey}"
					region     = "${var.region}"
				}


				data "aws_vpc" "default" {
					default = true
				}

				resource "aws_subnet" "extra" {
					cidr_block = "172.31.48.0/20"
					vpc_id = "${data.aws_vpc.default.id}"
				}

	To go folder path

			terraform init .
			terraform validate .
			terraform apply .

Every provider gives various data sources much like resources.


### Backends

Two terraform developers have same terraform script and they have applied terraform, it creates two different resources, 
as the state file is stored on individual developers laptop 

Now, if we want to restrict these two developers in such a way, 
whenever they execute terraform it should not create two different but one resource.

Terraform supports backends, to store state remotely 
and terraform also supports locking feature to avoid simultaneous access to terraform state. 

To use Azurerm Backend refer here

To use Aws Backend Refer Here

For Backend , Create a new stuff

		terraform destroy

		Delete terraform.tf state.tf backup.tf files 

		terraform init

		terraform validate .

		terraform apply .

		

#####################################################################################################################

#### Terraform provisioning :

Provisioning in Terraform is majorly used to do Configuration Management
Execution of Shell or PowerShell scripts/Ansible/Chef after creation of Virtual Machines is supported by terraform provisioners. 

Terraform supports many provisioners, 
some of them are
				* remote-exec
				* local-exec
				* Chef
				* For Ansible based CM use remote-exec to install ansible and execute ansible-playbook with localhost in inventory


To use provisioners, you should also use connection

Provisioners are written in terraform resources
			resource '<resource-type>' '<resource-name>' {
				connection {
					...
				}

				provisioner 'p-type' {

				}
			}


### Lets create a EC2 machine and install git & apache

First create Ec2 machine

Create a ProvisonDemo Folder
Create a main.tf File

			resource "aws_instance" "apache" {
				ami = "ami-04590e7389a6e577c"
				instance_type = "t2.micro"
				key_name = "packer"
				security_groups = ["sshonly"]
			}

			
			Hard core is not a good idea , Always use data source to get ami id 
			copy the pem file in the current location

			To go folder path
			terraform init .
			terraform validate .



### Now lets add the connection section for terraform to connect to ec2 machine

			    resource "aws_instance" "apache" {
				ami = "ami-04590e7389a6e577c"
				instance_type = "t2.micro"
				key_name = "packer"
				security_groups = ["sshonly"]

				connection {
					type = "ssh"
					user = "ec2-user"
					host = "${aws_instance.apache.public_ip}"
					private_key = "${file("./packer.pem")}"
				}
			}      

### Now lets provision using remote-exec to install git & apache

			    resource "aws_instance" "apache" {
				ami = "ami-04590e7389a6e577c"
				instance_type = "t2.micro"
				key_name = "packer"
				security_groups = ["sshonly"]

				connection {
					type = "ssh"
					user = "ec2-user"
					host = "${aws_instance.apache.public_ip}"
					private_key = "${file("./packer.pem")}"
				}

				provisioner "remote-exec" {
					inline = ["sudo yum install git -y", "sudo yum install httpd -y"]
				}
				}

				terraform validate .
				terraform apply .

				open git bash from same folder 
				$ ssh -i packer.pem ec2-user@ipaddress

				git --version
				sudo service httpd status

Exercise
Write a terraform template to create two vms (ec2 or azure vms) and install tomcat in one vm and mongo db (any database) in other vm


#########################################################################################################################################



mkdir foldername
cd foldername
terraform init .
terraform providers
terraform validate .
terraform apply .
terraform plan --out="aws.plan"
terraform apply aws.plan

terraform state
Subcommands:
    list    List resources in the state
    mv      Move an item in the state
    pull    Pull current state and output to stdout
    push    Update remote state from a local state file
    rm      Remove an item from the state
    show    Show a resource in the state

 
