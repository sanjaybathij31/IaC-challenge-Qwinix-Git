# Create/Upgrade a VM Scale Set Running IIS Configured For Autoscale

The following template deploys a Windows VM Scale Set (VMSS) running an IIS .NET MVC application integrated with Azure autoscale. This template can be used to demonstrate initial rollout and confiuguration with the VMSS PowerShell DSC extension, as well as the process to upgrade an application already running on a VMSS.

VMSS Initial Deployment
The template deploys a Windows VMSS with a 2 count of VMs in the scale set. Once the VMSS is deployed, the VMSS PowerShell DSC extension installs IIS and a default web app from a WebDeploy package. the website is accessible through LoadBalancer’s public DNS name and it shows
message “IaC challenge completed: Sanjay Bathija".
The application URL is an output on ARM template. It's http://qwinxiac.centralus.cloudapp.azure.com/MyApp
VMSS Application Upgrade
This template can also be used to demonstrate application upgrades for VMSS leveraging ARM template deployments and the VMSS PowerShell DSC extension.
.
Autoscale Rules
The Autoscale rules are configured as follows
•	Sample for Percentage CPU in each VM every 1 Minute
•	If the Percentage CPU is greater than 80% for 5 Minutes, then the scale out action (add more VM instances, one at a time) is triggered and scale down 1 web server at a time when cpu usage is less than 20%
•	Once the scale out action is completed, the cool down period is 1 Minute

Tasks Completed
  •	Create 1 VN (Virtual Network) and setup any other network resource required to enable the expected functionality of this challenge (e.g. route tables, internet gateway, nat gateway, subnets, etc.).
  •	Create 1 load balancer.
  •	Should be internet-facing with “crosszone" balancing activated.
  •	Should route http traffic to web servers
  •	Create two VM’s in private subnets located in different availability zones. 
  These VM’s should be created within a Virtual machine scale set that comply with the following:
    o	VM’s have Linux or Windows (choose only one)
    o	maintain at least 2 VM’s running at any given time.
    o	install and configure IIS on port 80.
    o	and configure IIS to serve that file by default.
    o	ensure IIS is always running even after rebooting the VM.
    o	create VM scaling policies to scale up 1 web server at a time based on a cpu alarm trigger of 80% and scale down 1 web server at a time when cpu usage is less than 20%.
    •	web servers should have a policy that allows putting and getting objects from that bucket.
     - set up an Azure Content Delivery Network distribution to speed up the distribution of
  static content stored in the storage bucket you created before.
  
Items Included
    o  VMSS -> Easy to Create, Provides high Availability, allows application to auto scaledown as demand changes
    o  Blob storage - public read
    o  load balancer: allow http traffic from anywhere
    o  Webservers : 2 VM’s in private subnets located in different availability zones allows redudancy and HA.
    o  good balance between cost and performance. The monthly cost of the stack configured by the template is below $300/mo
    o  automation of IIS / ASP.Net MVC application using MS powershell DSC from the ARM template
    o  network security groups : network security group (NSG) contains a list of security rules that allow or deny network traffic to resources connected to 
       Azure Virtual Networks (VNet). NSGs can be associated to subnets, individual VMs (classic), or individual network interfaces (NIC) attached to VMs (Resource Manager)
    o  The ARM templates could have be used as linked templates for provisioning individual resources, that way managing ARM templates will become more easier 
       and better reusability.



Tasks InComplete – timebound & Azure service I have is paid subscription

  •	Bastion host : 
  •	Initiate an Azure Database for PostgreSQL in a private subnet. Automate the installation of PostgreSQL client in web servers.

  •	Network traffic:
  •	Web servers: allow ssh traffic from only the bastion host, allow http traffic from the load balancer.
  2•	bastion: allow ssh connections from anywhere.
  •	load balancer: allow http traffic from anywhere.
  •	database: allow postgres traffic from web servers.

  Minimum template outputs:
  ● Bastion host’s static IP.
  ● For any spec that wasn’t mentioned, choose a proper value keeping in mind a
  good balance between cost and performance. The monthly cost of the stack
  configured by the template shouldn’t exceed $300/mo.

  Bonus Features-
  ● You can add these additional features to your template. Though they
  are not necessary to complete the challenge, they would certainly impress us:
 
  - set up notification to notify through email whenever a web server is launched and
  terminated. The email address should be received by an Azure resource manager
  parameter.


