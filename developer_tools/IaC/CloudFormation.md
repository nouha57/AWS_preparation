### What is it?

It is an IaC service that encourages collaboration and utilizes version control and automation. 

It can use a single template or multiple templates to create an environment and it can interact with other tools such as Puppet 

⇒ for exp, we can create instances with CloudFormation and puppet configures them 

The elements/sections that make up a CloudFormation template:

- Metadata
- Parameters: passed at runtime ( they’re defined here and then used in the ‘Resources’ section as a variable )
- Rules: evaluate the parameters
- Mappings: map an instance type to an AMI ID
- Conditions: to dictate the size of servers to be created for exp
- Transform: to integrate lambda function
- Resources: REQUIRED ( here we create our resources )
- Outputs: output the url to the server created for exp

Here’s an exp of a CF template 

```json
{
"AWSTemplateFormatVersion": "2010-09-09",
    "Description": "The IAM Resources for External DNS",
    "Parameters": {
        "DeployRole": {
            "Description": "Decide whether deploy or not the IAM Role",
            "Type": "String",
            "Default": "no",
            "AllowedValues": [
                "yes",
                "no"
            ]
        }
    },
    "Conditions": {
        "CreateIamRole": { "Fn::Equals": [ { "Ref": "DeployRole" }, "yes"] }
    },
    "Resources": {
        
        "ExternalDnsIamPolicy": {
            "Type": "AWS::IAM::ManagedPolicy",
            "Properties": {
                "PolicyDocument" : {
                    "Version": "2012-10-17",
                    "Statement": [
                        {
                            "Effect": "Allow",
                            "Action": [
                                "route53:ChangeResourceRecordSets"
                            ],
                            "Resource": [
                                "arn:aws:route53:::hostedzone/*"
                            ]
                        },
                        {
                            "Effect": "Allow",
                            "Action": [
                                "route53:ListHostedZones",
                                "route53:ListResourceRecordSets"
                            ],
                            "Resource": [
                                "*"
                            ]
                        }
                    ]
                },
                "ManagedPolicyName" : "ExternalDNSPolicy"
            }
        }
        
    }
}
```

### CloudFormation essentials

developer creates code template  ⇒ stored in S3 ⇒ CloudFormation creates a stack from the template ⇒ Stack 

As a result , we will get all of the AWS services up and running that we defined in the CF template

### CloudFormation intrinsic functions

built-in cloudFormation functions that allow you to assign dynamic values to properties ( fr exp assigning a public IP address at runtime ) 

**There are 7 key intrinsic functions**

1. Ref
2. **Fn:GetAtt**

In AWS, each resource has a set of attributes ( for exp we can have a load balacer with the given name ‘myELB’ that has attributes such as its DNSname, its SG … ) 

```yaml
"Fn::GetAtt" : [ "myELB", "DNSName" ]

# we call it like so:
!GetAtt myELB.DNSName # and this will give us the DNS name of the load balancer 
```

1. **Fn:FindInMap** : returns the value corresponding to the key in a map 

In this example, the FindInMap function maps the EC2 instance architecture to an AMI ID  

```yaml
us-east-1: 
      HVM64: "ami-0ff8a91507f77f867"
      HVMG2: "ami-0a584ac55a7631c0c"

# key = "HVM644"  => this represents the EC2 architecture 
# value = "ami-0ff8a91507f77f867" = >this represents the AMI ID 

# -- Under Resources section -- 
Properties: 
      ImageId: !FindInMap
        - RegionMap
        - !Ref 'AWS::Region'
        - HVM64

```

1. **Fn:ImportValue**

used to export value from **stack A** and import it into **stack B**

1. Fn:Join 
2. Fn:Sub
3. Condition Functions 

Use cases:

- used to create or not create resources based on a condition ( also can be used to size resources ) .. for exp based on a condition, for a dev env create a smaller resource than what would be created for a prod env for cost savings

 

### CloudFormation Wait Conditions and Creation Policies

**Wait conditions**

- **It”s a resource itself**
- used to send signals to cloudFormation ( for exp signalling the termination of creation of an EC2 instance or send signals if EC2 creation fails .. ) and CF will wait for these success signals before proceeding to creating more resources
- if we specify timeout period for CF and don’t receive success signals then our stack creation will fail ( ERROR MSG: timeout period expired )

Wait conditions properties:

- *Count* : nb of success signals that CF must receive before it continues the stack creation process
- *Handle*
- *Timeout*

**Creation policies** 

- **is an attribure associated with resources** and they basically do the same task as wait conditions
- best suited for hybrid environments - for EC2 and Autoscaling

### CloudFormation helper scripts

Helper scripts : a set of python scripts that bootstrap instances ( like EC2 ) during startup 

Exps of helper scripts:

- cfn-init
- cfn-signal
- cfn-hup
- cfn-get-metadata

### CloudFormation Stack Protection

There are 4 ways to protect stacks:

1. Termination protection 
2. Stack-level policies 
3. Resource-level policies 
4. IAM policies 

**Termination protection**

For stack that hold critical resources ( e.g production stacks ) you can apply termination protection.