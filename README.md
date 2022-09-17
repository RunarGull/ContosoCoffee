# ContosoCoffee
This is a GitHub repository for a case study 0 tasked by our instructor for Microsoft University 2022. We will use Microsoft Azure cloud to provide a solution for the case study. The repository will be used for collaboration between three students in the class. Do feel free to fork this repository if wished.

A shop named "Contoso Coffee" will be opened in London and New York. They are looking for a cloud-hosting solution that will be used to host their website, and as well keeping the cost at a minimum.

Please see in the "Case Study" directory to read the original assignment that contains the detailed information about the task

These are following key-point requirements for implementing the hosting-solution:

* Assign admin access for three user accounts
    * Bob is the owner of Contoso Coffee, and he needs full admin access to track billing of resources.
    * Dave needs admin access to resources that is associated to the website
    * Mark requires read-only access to the resources.

* Website hosting:
    * Use Azure Container Registry (ACR) to store the Docker Images (Level 3 Difficutly)
    * Use Azure Container Instances (ACI) to deploy the website (Level 3 Difficutly)
    * Load-Balance the traffic and keep the website rendundant across separate datacenter regions, so it is highly available.

* Storage Account:
    * Geo-redundant (GRS)
    * Store images and preveiw them on the website by using Shared-Access-Signature (SAS) token.

* Azure Resource Manager (ARM) Template must be created (exported) is it is possible to re-deploy the website solution.

_____________________________________________

# Azure Active Directory Architectural Design
When the case study stated that we must assign the user accounts admin access. We took this task a step further by designing an Active Directory (AD) in regards to best-practices in order to make future administration easier and strengthen overall security in the tenant.

![](Active%20Directory%20Design/AD_diagram.png)

Note that the "Project Managers" group is the student collaborators for the project. Notice that their groups is placed inside the "Container Registry Developer". Instead of assigning each users individually to a group, you can just assign a whole group. This method is known as a "group nesting". Which is a big part of administration, and makes it so much easier. The mehtod is also formerly known as a "A-G-DL-P" practice.

Let´s say Bob (the owner) wishes to hire app developers to create a new image with use of Container Instances. He can then just initiate a Business-To-Business collaboration and assign their external group into the "Container Registry Developer" security group. Which allowes them to push new images into the registry.

The same thing applies to the other security groups as well. An administator can just place new users, or existing groups within those groups to easily administer user accounts. Instead of individually assigning roles and permissions to every users.

All of the resources that is associated with the website - Is placed into a common Resource Group named "ContosoCoffeeWebApp". There is a nice feature to apply custom policy to the group. Like in this situation; We want to enforce every resources to inherit a tag from the group. So that it is possible for the CCWA-CostMgMt group to track the billing of the resources with a specific common tag.

The Global Admin account is the only account that has the global role in the tenant. 
It is separated from the user accounts due to security reasons. Like, if an active user account were to be assigned the global role. Many things could go wrong with the tenant with careless usage of the role. Which is why we decided to create a standalone account with a global admin role, and secure it with additional security features like Multi-Factor Authentication, Conditional Access Policy and such.

_____________________________________________
# Traffic Manger
Traffic Manager is a global Azure service that works in the layer 7 (application layer). It will be used to load-balance traffic and keep the website rendundant.

As shown in the diagram. Users that is closes to the Uk South external endpoint - Will be redirected to the UK South website region, and same goes with users closest to the East US 2 region.

![](/Traffic%20Manager/Traffic%20Manager%20Diagram.png)

_____________________________________________
# Storage Account
The storage account will be used as a common storage for both the deployed website.
It is a Genereal Purpose V2 account type and is Geo-Redundant. The data is replicated to a paired region, in order to provide high availability to the images. If a region goes down - The website will still be able to display the images from the paired region.

![](/Storage%20Account/Storage%20Account%20Type.png)

The Lifecycle Management is a feature within the storage account that can automatize a certain tasks to files. In this scenario - The customer wishes to keep cost at a minimum. So the custom rule is set to change the access tier of the blob files within the container. When they haven´t been accesssed for 7 days (this can be adjusted), the access tier is then changed from hot to cool tier. This will then minimize the storage cost of those files. In this case, it is only a few pictures, but imagine if there were thousands. Then this kind of feature would be very helpful to dynamically change the tiers in order to save cost.

![](/Storage%20Account/Lifecycle%20Management.png)

_____________________________________________
# The Contoso Coffee Website

This is the how the deployed website looks. It the HTML code is originally borrowed from the kramit/MSU2022 repo. The index.html file is edited to add in the SAS token and add in the Contoso Coffee titles and descriptions.

![](/ContosoCoffeeWebsite/ContosoCoffee%20Website.png)

_____________________________________________
# Azure Resource Manager (ARM) Template
The ARM template is available on this repo. If you wishes to re-deploy our solution - Please feel free to use our template.