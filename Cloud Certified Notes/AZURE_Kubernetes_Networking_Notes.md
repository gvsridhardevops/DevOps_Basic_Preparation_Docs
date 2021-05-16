## Networking with Kubernetes & Azure 


This document describes the difference between Basic and Advance Networking for Azure Kubernetes Service (AKS) whilst also demonstrating how to deploy and configure other networking resources such as LB, App Gateway as Ingress Controller and Azure Firewall.

AKS allows you to bring the rich set of Azure network capabilities to containers, by utilising the same software defined networking stack that powers virtual machines. This is made possible by using the container network interface (CNI) plug-in, which is installed on the Azure Virtual Machine. The plug-in assigns IP addresses from a virtual network to containers, attaching them to the virtual network and connecting them directly to other containers and virtual network resources. The plug-in doesn’t rely on overlay networks, or routes for connectivity. 

At a high level, the plug-in provides the following capabilities:
*   A virtual network IP address is assigned to every Pod, which could consist of one or more containers.
*   Pods can connect to peered virtual networks and to on-premises over ExpressRoute or a site-to-site VPN. Pods are also reachable from peered and on-premises networks.
*   Pods can access services such as Azure Storage and Azure SQL Database, that are protected by virtual network service endpoints.
*   Network security groups and routes can be applied directly to Pods.
*   Pods can be placed directly behind a internal or Azure public Load Balancer, just like virtual machines
*   Pods can be assigned a public IP address, which makes them directly accessible from the internet. Pods can also access the internet themselves.
*   It works seamlessly with Kubernetes resources such as Services, Ingress controllers, and Kube DNS. A Kubernetes Service can also be exposed internally or externally through the Azure Load Balancer.

![alt text](https://github.com/jgmitter/images/blob/master/s1.png)
 
# Connecting Pods to a virtual network
Pods are hosted within a virtual machine that is part of a virtual network. A pool of IP addresses for the Pods is configured as secondary addresses on a virtual machine's network interface. Azure CNI sets up the basic Network connectivity for Pods and manages the utilisation of the IP addresses in the pool. When a Pod is spun up in the virtual machine, Azure CNI assigns an available IP address from the pool and connects the Pod to a software bridge in the virtual machine. When the Pod is terminated, the IP address is returned back to the pool of addresses. The following picture shows how Pods connect to a virtual network:

 ![alt text](https://github.com/jgmitter/images/blob/master/s2.png)

# Internet access
To enable Pods to access the internet, the plug-in configures iptables rules to network address translate (NAT) the internet bound traffic from Pods. The source IP address of the packet is translated to the primary IP address on the virtual machine's network interface. Windows virtual machines automatically source NAT (SNAT) traffic destined to IP addresses outside the subnet the virtual machine is in. Typically, all traffic destined to an IP address outside of the IP range of the virtual network is translated.

# Limits
The plug-in supports up to 250 Pods per virtual machine and up to 16,000 Pods in a virtual network. These limits are different for Azure Kubernetes Service.

# Using the plug-in
The plug-in can be used in the following ways, to provide basic virtual network attach for Pods or Docker containers:

*   Azure Kubernetes Service: The plug-in is integrated into the Azure Kubernetes Service (AKS), and can be used by choosing the Advanced Networking option. Advanced Networking lets you deploy a Kubernetes cluster in an existing, or a new, virtual network. To learn more about Advanced Networking and the steps to set it up, see Network configuration in AKS.
*   AKS-Engine: AKS-Engine is a tool that generates an Azure Resource Manager template for the deployment of a Kubernetes cluster in Azure. For detailed instructions, see Deploy the plug-in for AKS-Engine Kubernetes clusters.
*   Creating your own Kubernetes cluster in Azure: The plug-in can be used to provide basic networking for Pods in Kubernetes clusters that you deploy yourself, without relying on AKS, or tools like the AKS-Engine. In this case, the plug-in is installed and enabled on every virtual machine in a cluster. For detailed instructions, see Deploy the plug-in for a Kubernetes cluster that you deploy yourself.
*   Virtual network attach for Docker containers in Azure: The plug-in can be used in cases where you don’t want to create a Kubernetes cluster, and would like to create Docker containers with virtual network attach, in virtual machines. For detailed instructions, see Deploy the plug-in for Docker.


# Let’s have a look in details the two options

# Basic Networking on K8
Uses kubenet network plugin and has the following features:
*   Nodes and Pods are placed on different IP subnets
*   User Defined Routing and IP Forwarding is for connectivity between Pods across Nodes.

![alt text](https://github.com/jgmitter/images/blob/master/s3.png)

This approach has some drawbacks, including degraded performance, the need to manage two IP CIDRs and complexity with Peering and On-Premise connectivity. For these reasons, you may want to consider the Advanced Networking option. 

# Advance Networking 
 
![alt text](https://github.com/jgmitter/images/blob/master/s4.png)
 
Unlike the basic networking approach, using the Advance Networking enabled by the CNI plug-in allows us to only manage a single IP CIDR. Offers better performance and supports both peering and on-premise connectivity out of the box. 

# Prerequisites on creating an Advance Networking on AKS
*   The virtual network for the AKS cluster must allow outbound internet connectivity.
*   Don't create more than one AKS cluster in the same subnet.
*   AKS clusters may not use 169.254.0.0/16, 172.30.0.0/16, or 172.31.0.0/16 for the Kubernetes service address range.
*   The service principal used by the AKS cluster must have at least Network Contributor permissions on the subnet within your virtual network. If you wish to define a custom role instead of using the built-in Network Contributor role, the following permissions are required:

Microsoft.Network/virtualNetworks/subnets/join/action
Microsoft.Network/virtualNetworks/subnets/read

# Planning IP addressing for your cluster

![alt text](https://github.com/jgmitter/images/blob/master/s5.png)
 
# Additional subnets
*   Kubernetes service address range
*   Non-overlapping with other subnets in your network (does not need to be routable) and not 169.254.0.0/16, 172.30.0.0/16 and 172.31.0.0/16.
*   Smaller than /12
*   Docker bridge address
*   Non-overlapping with AKS Subnet

# What is a Service?
Logical set of Pods and a policy by which to access them. 

The set of Pods targeted by a Service is (usually) determined by a Label Selector.

# Accessing services from inside the cluster
Kubernetes supports several types of Services. The default type is Cluster IP
A ClusterIP Services has a stable IP address and port that is only accessible from inside the cluster. We call this IP its ClusterIP and it’s programmed into the network fabric and guaranteed to be stable for the life of the Services.
The ClusterIP gets registered against the name of the Service on the Cluster’s native DNS service. All Pods in the cluster are pre-programmed to know the cluster’s DNS service, meaning all Pods are able to resolve Service names.

Let’s look at this simple example

Creating a new service called “cat-svc” will trigger the following: Kubernetes will register the name “cat-svc”, along the ClusterIP and port, with the cluster’s DNS services. The name, ClusterIP, and port are guaranteed to be long-lived and stable, and all Pods in the cluster will be able to resolve “cat-svc” to the ClusterIP. IPTABLES or IPVS rules are distributed across the cluster that ensure traffic sent to the ClusterIP gets routed to Pods on the backend.
As long as the Pod (application microservices) knows the name of Services, it can resolve that to its ClusterIP address and connect to the desire Pods.

This only works for Pods and other object on the cluster, as it requires access to the cluster’s DNS services. It doesn’t work outside the cluster.


# Accessing services from outside the cluster
Kubernetes has another type of Services called a NodePort Service. This builds on top of ClusterIP and enables access from outside the cluster.

We already know that the default Service Type is ClusterIP, and that it registers a DNS name, virtual IP, and port with the cluster DNS. A different type of Service, called a NodePort Services builds on this by adding another port that can be used to reach the Service from outside the cluster. This additional port is called the NodePort.

There are other type of Services, such as LoadBalancer services. They build on top of NodePort Services (which in turn build on top of ClusterIP Services) and allow clients on the internet to reach your Pods via Load Balancer.

Service provides important features that are standardized across the cluster: load-balancing (L4), service discovery between applications, and features to support zero-downtime application deployments.


# Internal and External Load Balancer

![alt text](https://github.com/jgmitter/images/blob/master/s6.png)

To provide access to your applications in Azure Kubernetes Service (AKS), you can create and use an Azure Load Balancer. A load balancer running on AKS can be used as an internal or an external load balancer. An internal load balancer makes a Kubernetes service accessible only to applications running in the same virtual network as the AKS cluster. An external load balancer receives one or more public IPs for ingress and makes a Kubernetes service accessible externally using the public IPs.

This session shows you how to create and use an internal load balancer with Azure Kubernetes Service (AKS).

[!NOTE] Azure Load Balancer is available in two SKUs - Basic and Standard. By default, the Basic SKU is used when a service manifest is used to create a load balancer on AKS. Using a Standard SKU load balancer provides additional features and functionality, such as larger backend pool size and Availability Zones. It's important that you understand the differences between Standard and Basic load balancers before choosing which to use. Once you create an AKS cluster, you cannot change the load balancer SKU for that cluster. For more information on the Basic and Standard SKUs, see Azure load balancer SKU comparison.

One more thing to remember is that Basic Load Balancer is not accessible across Global VNET Peering.

# Create an internal load balancer
To create an internal load balancer, create a service manifest named internal-lb.yaml with the service type LoadBalancer and the azure-load-balancer-internal annotation as shown in the following example:

![alt text](https://github.com/jgmitter/images/blob/master/s7.png)

![alt text](https://github.com/jgmitter/images/blob/master/s8.png)

An Azure load balancer is created in the node resource group and connected to the same virtual network as the AKS cluster.

When you view the service details, the IP address of the internal load balancer is shown in the EXTERNAL-IP column. In this context, External is in relation to the external interface of the load balancer, not that it receives a public, external IP address. It may take a minute or two for the IP address to change from <pending> to an actual internal IP address, as shown in the following example:
 
![alt text](https://github.com/jgmitter/images/blob/master/s9.png)
 

# Specify an IP address
If you would like to use a specific IP address with the internal load balancer, add the loadBalancerIP property to the load balancer YAML manifest. The specified IP address must reside in the same subnet as the AKS cluster and must not already be assigned to a resource.

![alt text](https://github.com/jgmitter/images/blob/master/s10.png)

When deployed and you view the service details, the IP address in the EXTERNAL-IP column reflects your specified IP address

![alt text](https://github.com/jgmitter/images/blob/master/s11.png)
 
![alt text](https://github.com/jgmitter/images/blob/master/s12.png)

# Specify a different subnet
To specify a subnet for your load balancer, add the azure-load-balancer-internal-subnet annotation to your service. The subnet specified must be in the same virtual network as your AKS cluster. When deployed, the load balancer EXTERNAL-IP address is part of the specified subnet.

![alt text](https://github.com/jgmitter/images/blob/master/s13.png)

 
# Delete the load balancer
When all services that use the internal load balancer are deleted, the load balancer itself is also deleted.
You can also directly delete a service as with any Kubernetes resource, such as kubectl delete service internal-app, which also then deletes the underlying Azure load balancer.


# Securing Kubernetes Services with Firewall
By default, AKS clusters have unrestricted outbound (egress) internet access. This level of network access allows nodes and services you run to access external resources as needed. If you wish to restrict egress traffic, a limited number of ports and addresses must be accessible to maintain healthy cluster maintenance tasks. Your cluster is then configured to only use base system container images from Microsoft Container Registry (MCR) or Azure Container Registry (ACR), not external public repositories. You must configure your preferred firewall and security rules to allow these required ports and addresses.

# Egress traffic overview
For management and operational purposes, nodes in an AKS cluster need to access certain ports and fully qualified domain names (FQDNs). These actions could be to communicate with the API server, or to download and then install core Kubernetes cluster components and node security updates. By default, egress (outbound) internet traffic is not restricted for nodes in an AKS cluster. The cluster may pull base system container images from external repositories.

To increase the security of your AKS cluster, you may wish to restrict egress traffic. The cluster is configured to pull base system container images from MCR or ACR. If you lock down the egress traffic in this manner, you must define specific ports and FQDNs to allow the AKS nodes to correctly communicate with required external services. Without these authorized ports and FQDNs, your AKS nodes can't communicate with the API server or install core components.
*   Target FQDN
 *    *.azmk8s.io – http, https (Eg. *eastus.azmk8s.io)
 *    k8s.gcr.io – http, https
 *    storage.googleapis.com - http, https
 *    *auth.docker.io – http, https
 *    *cloudflare.docker.io – http, https
 *    *registry-1.docker.io – http, https
*   NAT Rules
*    TCP Port 22 to AKS Destination Addresses for the Region

 ![alt text](https://github.com/jgmitter/images/blob/master/s14.png)

You can use Azure Firewall or a 3rd-party firewall appliance to secure your egress traffic and define these required ports and addresses. AKS does not automatically create these rules for you. The following ports and addresses are for reference as you create the appropriate rules in your network firewall.

Basic networking requires you to modify the routetable that will be created by the AKS deployment, add another route and point it towards the internal ip of the azure firewall. If you want to use advanced networking skip this section and continue in the Advanced Networking section. One common question is  “How can I expose a Kubernetes service through the azure firewall?” If you expose a service through the normal LoadBalancer with a public ip, it will not be accessible because the traffic that has not been routed through the azure firewall will be dropped on the way out. Therefore, you need to create your service with a fixed internal ip, internal LoadBalancer and route the traffic through the azure firewall both for outgoing and incoming traffic. If you only want to use internal load balancers, then you do not need to do this. You can read more about this here. 

# Create a Kubernetes cluster with Application Gateway ingress controller 

AKS manages your hosted Kubernetes environment. AKS makes it quick and easy to deploy and manage containerised applications without container orchestration expertise. It also eliminates the burden of ongoing operations and maintenance by provisioning, upgrading, and scaling resources on demand, without taking your applications offline.

An ingress controller is a piece of software that provides reverse proxy, configurable traffic routing, and TLS termination for Kubernetes services. Kubernetes ingress resources are used to configure the ingress rules and routes for individual Kubernetes services. Using an ingress controller and ingress rules, a single IP address can be used to route traffic to multiple services in a Kubernetes cluster. All the above functionalities are provided by Azure Application Gateway, which makes it an ideal Ingress controller for Kubernetes on Azure.

![alt text](https://github.com/jgmitter/images/blob/master/s15.png)

https://github.com/Azure/application-gateway-kubernetes-ingress

*   Attach Application Gateways to AKS Clusters
*   Load Balance from the Internet to pods
*   Supports features of k8s ingress resource – TLS, multi-site and path-based routing
*   Pod-AAD for ARM authentication

# Need for Network Policies for Containers

*   NSGs secure the infrastructure
 *   Corporate network policies placed on the subnet is applied to container workloads
*   Microservices need fine grain access control
 *   Containers are dynamic and their IP address changes frequently
 *   Traffic filtering required between containers in the same VM

*   A Kubernetes access policy Specification to restrict communication between Pods
*   Labels used to select Pods to which policies apply
*   Ingress/Egress policy rules defined for a Pod Label, Namespace or IP Block
 *   Policies are defined as a yaml manifest

# Networking policy
A Policy solution is an implementation of this specification that listen to the Kubernetes API and enforces the defined policies. Usually runs as a daemonset   which make sure that an instance of a specific pod is running on all (or a selection of) nodes in a cluster. It creates pods on each added node and garbage collects pods when nodes are removed from the cluster.
 
![alt text](https://github.com/jgmitter/images/blob/master/s16.png)
 
*   Azure Policy plugin is Open Source on GitHub, 
*   Uses Linux IPTables to enforce the policies
*   Rules are translated to allowed/disallowed IP filtering rules  
*   Works with Azure CNI that connects Pods to an L2 Bridge for VNet connectivity
*   Default policy option in AKS
*   Supported in aks-engine
*   Set networkPolicy setting to azure in cluster definition file
*   Supported for Kubernetes DIY clusters in Azure
*   Supports Linux hosts
 
![alt text](https://github.com/jgmitter/images/blob/master/s17.png)

# Structure of a networking policy

Remember that all Pods are non-isolated by defaults, they are running on a flat network, in other words, all pods can talk to other pods.
By default, they accept traffic from anyone and if customer has a multi stage/zone project this could expose security risks. You can think of 3 tier webapp but also Front end could technically talk to DB on other pods.
Networking Policy create a micro segmentation for containers, like Network Security Group provides for VM’s in the subnet of a Virtual Network in Azure.

![alt text](https://github.com/jgmitter/images/blob/master/s18.png)

If no ingress or egress rules are specified, then traffic to/from the selected Pods is disallowed, Let's say you likely want to block traffic directly to back-end applications, you’ll need to apply a policy on the ingress rule to allow traffic to the Pod selected matching one or more specified sources on the specific port, with IP from CIDR range, Pod Namespace and or Pod Label
You can apply multiple policy yaml files
For Azure Policies, the recommendation is to combine policies for a Pod into one file
Policy can be removed by deleting the policy yaml file
kubectl delete –f policy.yaml

About Calico on Azure

![alt text](https://github.com/jgmitter/images/blob/master/s19.png)

For more information please check our Microsoft Website:
https://docs.microsoft.com/en-us/azure/aks/



















# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# Legal Notices

Microsoft and any contributors grant you a license to the Microsoft documentation and other content
in this repository under the [Creative Commons Attribution 4.0 International Public License](https://creativecommons.org/licenses/by/4.0/legalcode),
see the [LICENSE](LICENSE) file, and grant you a license to any code in the repository under the [MIT License](https://opensource.org/licenses/MIT), see the
[LICENSE-CODE](LICENSE-CODE) file.

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all other rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.