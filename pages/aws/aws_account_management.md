---
title: AWS Account Management
keywords: aws
last_updated: January 26, 2017
tags: [AWS, cloud_basics, console, api, account_management]
summary: "AWS account management"
sidebar: mydoc_sidebar
permalink: aws_account_management.html
folder: aws
---


## Introduction


This page describes core AWS account management tasks in brief and then again in further detail. We assume that either
you are operating a research-credit based account provided directly from AWS or you are using a paid account established 
through the DLT third party provider. We make this and other distinctions as we go. 

For UW Researchers: It is also possible for you to create an account directly with AWS but we recommend looking into
DLT-based accounts first as they provide some particular benefits such as egress waiver to 15% of your monthly bill.


## Links
- [AWS](http://aws.amazon.com)
- [DLT portal for new AWS accounts](https://customerportal.dlt.com/internet2/)
- [Rotating access keys: Documentation](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html?icmpid=docs_iam_console#Using_RotateAccessKey)
- [Confederate a UW NetID with AWS (wiki)](https://wiki.cac.washington.edu/pages/viewpage.action?pageId=78712235)
- Smart phone: [Activating MFA](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) and [De-activating MFA](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_disable.html)


## Warnings


- ***Do not start using a new AWS account until you have trained up on how to keep the account secure.***


## So you want to open a paid AWS account


We assume you are at the University of Washington or are a covered *affiliate* of the University. 


- Establish a Blanket Purchase Order
- Email the UW help desk help at uw dot edu with the subject AWS Account. Ask for instructions on how to proceed. 


## Account set-up


- Set up your account to have "all green checkmarks". This means you are logged out and back in as an **admin**.
  - After a year or so you may be encouraged to 'rotate your access keys'; see the link provided above
  - One of those checkmarks is enabling MFA or Multi-Factor Authentication. Do this.
    - It is a little bit of a pain but it gets you another measure of security for your cloud account
    - Your MFA device is probably a smart phone. Here are the links for procedures to...
      - [Activate](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) 
      - [De-activate](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_disable.html)
- Start and Stop an EC2 instance: Know what it costs, what EBS is, and log in to it before you Stop it.
- Study up on cloud tech, enough to understand cost, security and capacity; and then make a plan. 


## Cost tracking via tags


On AWS you can allocate assets such as S3 storage buckets and EC2 compute instances. These in turn cost money
(either actual money or credits if you have them available) and you probably care about how much. To this end
you can tag each asset with any one of a number of supported keys. A tag is a key-value pair such as

```
Name: Kilroy
Discipline: Genome Sciences
Religion: Pastafarian
```

After a month of using your AWS account suppose you want to see how much your resources are costing you sorted
by discipline. First go back in time and tag everything; then use the AWS Console to sort by your values. 

DLT will sort values for the following keys. Notice they are business-oriented and that there are ten
generic 'CA' keys that you can make mean anything you like. You could for example decide to use CA003 
to record room temperature when you create the tag; and you could later sort on those temperatures. 
So again: You can tag with *any* key string you like but DLT will be able to sort on the following:

```
Application
CA001
CA002
CA003
CA004
CA005
CA006
CA007
CA008
CA009
CA010
Company
Contract
CostCode
CreationDate
Creator
Department
DeptCode
Environment
Grant
Location
Name
Order
Organization
OU
Owner
Payer
Product
Project
ProjectName
ProjectNumber
ProjectType
ResponsibleParty
Role
Service
Status
Use
```

For the HPC Club account we will be using the Project key to tag resources. Each student researcher will have
an assigned project string, let's call that <xyz>. It will be typically the student's name or something easily 
recognizable like that.  The asset should be tagged Project: <xyz> and when it is possible to name the 
asset or service it should be named <xyz>-etcetera. 


# IAM 


First let's get working definitions: 

- Key pair: A certificate used to access some resource like an EC2 instance
- Credential: Generically an access mechanism: User name + password; or access keys 
- User: A person who is affiliated with the account; has some form of access
  - i.e. is a person with an associated access key: Public and Private text strings; in one file, kept in a secure location
  - This is **distinct** from the key pair (official term) associated with an EC2 instance used for logging in to that instance!!!
- Group: A collection of Users
  - Can have a permission attached that applies to all IAM Users in that group
- Role: Similar to a User but without visible Credentials
  - A Role exists as an abstract Thing -- with a unique name -- where it is actually *defined* by the policies attached to it
  - Roles are nice because they have temporary Credentials that are automatically generated and invisible to our view of what is happening
  - You *assume* a role
    - An EC can assume a role on launch: For example **Can Write To S3**
    - Federated identities: Once you authenticate you assume a role based on some pre-existing rule / logic
    - Other AWS services can assume roles
    - IAM Users can assume roles
- Policy: Is a set of conditions that is *attached* to an IAM Role or to an IAM User or an IAM Group
  - Thought of as a set of permissions, or "the way that you assign permissions"
  - Policies are evaluated at the time of a request submitted to AWS; see below
- Policy Evaluation
  - By default: Everything starts with a **DENY**
  - Upon request: All policies are evaluated at once; there is no order of evaluation
    - If there is an explicit DENY it wins, even over an explicit ALLOW
    - If DENY is not present: Look for ALLOW: If found: Ok. Else: See default, above.
    - One more detail: There is also ALLOW a NOT which is not as strong as DENY: It can be overridden by another ALLOW.
  - Debugging concept: If you have explicitly set ALLOW for something that still does not happen 
    - There must be an explicit DENY somewhere else that you have not noticed
  - Basic elements of policies:
    - There is always an **Action**: What the API commands are. 
      - '*' is wildcard do anything.
      - 'S3:*' is like the S3 API call, again with wildcard: Can do any action on S3
    - There is always a Resource: In a sense the **Action**'s Direct Object: What the action is applied to
    - There is always an Effect: Logical ALLOW or DENY; logical condition as noted above
    - More: There is a great deal more to Policies and Policy Evaluation...
      - For example conditional stuff like CIDR blocks allowed to connect to a VPC
      - To pursue in depth: Don't use cloudmaven; go direct to the AWS resource content


# IAM Access Example: Spot market

When the Power User policy (an AWS-managed policy) is assigned to an IAM User they still do not have the ability to 
do IAM tasks such as passing a role to an instance. So Bill (an IAM User) has Power User access; which in turn has 
an explicit ALLOW of NOT IAM. This means IAM tasks are not available to Bill; but Bill wishes to use the Spot market
which in turn requires an EC2 instance to receive an appopriate Spot Fleet role. What to do? First let us anticipate 
that this problem will recur; therefore let's solve the problem with a Group. Second since the Power Access policy
does not include an DENY it can be overridden. Therefore:


- Create Group SpotFleetAccess
- Include Bill in this Group
- Create a new Policy (see below) that has the desired ALLOW actions
  - Lines that provide IAM Role assignment
  - Lines that allow S3 access
- Assign this Policy to the same Group


Whereupon Bill now logs in and can spin up EC2 instances that access S3; and these instances receive a role that
allows them to come from the Spot market pool. 


[This link gives the necessary Spot Fleet policy](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-requests.html).
Here is an example of the text; note the explicit ALLOW of PassRole, ListRoles, ListInstanceProfiles. This is required to work
with Spot Fleet.  Note: We recommend getting the actual text for this policy from the above link.


```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
              "iam:PassRole",
              "iam:ListRoles",
              "iam:ListInstanceProfiles"
            ],
            "Resource": "*"
        }
    ]
}
```




{% include links.html %}
