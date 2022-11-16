# EC2

Type: Essential

### EC2 Pricing Options:

- on-demand:

pay by the hr or the second, depending on the type of the instance you run 

- reserved:

reserved capacity for 1 or 3 years.

Up to 72% discount on the hourly charge

- spot

Purchase unsued capacity at a discount up to 90%

- dedicated

A phy EC2 server dedicated for ur use ( the most expensive option ) 

Feaatures of each one of them:

|  | on-demand | reserved | spot | dedicated |
| --- | --- | --- | --- | --- |
|  | Flexible ( low-cost w/o any payments in advance | apps have predictable usage  ( u can commit to use a specific amount of compute power for 1-year or 3-year period ) + pay up front ( save up to 72% )  | * apps that hv flexible strt and end times -  * apps that r only feasible at very low compute prices - * users with urgent need for large amounts of additional compute capacity |  |
|  | apps with short-term | apps that require reserved capacity |  |  |
|  |  | operate at regional level |  |  |

### EBS volumes:

EBS is a way to add a new disk to your instance

In the screenshot below, the block ‘ xvdf ‘ is actually an EBS volume that I created it and attached it to my instance

![Untitled](EC2%20b4f97316f4f941d9a3ba313b16a25bbf/Untitled.png)

PS: EC2 instance and EBS volume must belong to the same region ( Both in us-east-1a for example  )

### How to create an EBS volume ?

![Untitled](EC2%20b4f97316f4f941d9a3ba313b16a25bbf/Untitled%201.png)

Then specify whether u want it to be encrypted or not / from a snapshot or not … 

- From a snapshot :

there are encrypted an unencrypted snapshots 

If u create an EBS volume from an encrypted snapsho ,then the EBS volume will be encrypted and vica versa 

- Not from a snapshot

EBS Snapshots:

They’re good because you can :

- make a backup of your EBS volume
- can copy snapshot across AZ or region ( copy your data )

### AMI ( Amazon Machine Image ) :

- used to customize EC2 instances ( adding your own software, configuration, OS, monitoring … .)

⇒ Faster boot & configuration time 

- They’re built for a specific region ( can be copied across regions )
- There’s a public AMI provided by AWS or we can create our own AMI

To create a custome AMI 

1. right click on EC2 instance ID ⇒ Image ⇒ create Image 
2. Now, we can create an EC2 instance from the created AMI