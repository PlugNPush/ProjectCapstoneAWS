# ProjectCapstoneAWS
Vincent MARGUET + Logan LE LAY + Michaël NASS


# AWS Cloud & Big Data Architectures - Project

---

Table of Contents

1.  [AWS Deploymnet](#deploy)
2.  [Quizz](#quizz)
3.  [IAM](#iam)
4.  [AWS QuickSight](#sight)

---

<div id='deploy'/>

# AWS Deployment

This part is about the deployment of a PHP Website using AWS Cloud9, EC2 and RDS on a tailored network infrastructure. All the screenshots and details are in the AWSDeployment folder of this repository.

<div id='quizz'/>

# QUIZ

There are some images inside **iam quizz** and **network quizz** folder. The answers have been checked on the images themselves.

For the IAM Quizz:  
1.png: **Option 3**  
2.png: **Option 1**  
3.png: **Option 3,4**  
4.png: **Option 1**  
5.png: **Option 2**  
6.png: **Option 3**  
7.png: **Option 1**  

For the Network Quizz:  
1.png: **Option 3**  
2.png: **Option 3**  
3.png: **Option 1,3,6**  
4.png: **Option 4**  

<div id='iam'/>
  
# IAM

This part is to make sure you are able to evaluate and apply some policy constraintes.

## Policies evaluation

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowEC2AndS3",
      "Effect": "Allow",
      "Action": [
        "ec2:RunInstances",
        "ec2:TerminateInstances",
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:ec2:us-east-1:123456789012:instance/*",
        "arn:aws:s3:::example-bucket/*"
      ]
    }
  ]
}
```

### Question: What actions are allowed for EC2 instances and S3 objects based on this policy? What specific resources are included?

Actions allowed for EC2 instances are to run and terminate an instance, and S3 objects are to get and put an object.
The specific resources of EC2 is the instance itself from us-east-1 region and the specific resources of S3 is the bucket example-bucket and all its objects.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowVPCAccess",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeVpcs",
        "ec2:DescribeSubnets",
        "ec2:DescribeSecurityGroups"
      ],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-west-2"
        }
      }
    }
  ]
}
```

### Question: Under what condition does this policy allow access to VPC-related information? Which AWS region is specified?

The condition states that access is allowed to VPC-related information if the region is us-west-2.
The AWS region specified is us-west-2.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowS3ReadWrite",
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:PutObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::example-bucket",
        "arn:aws:s3:::example-bucket/*"
      ],
      "Condition": {
        "StringLike": {
          "s3:prefix": ["documents/*", "images/*"]
        }
      }
    }
  ]
}
```

### Question: What actions are allowed on the "example-bucket" and its objects based on this policy? What specific prefixes are specified in the condition?

The actions allowed are to get, put and list objects from the bucket example-bucket.
The specific prefixes specified in the condition are documents/* and images/*.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowIAMUserCreation",
      "Effect": "Allow",
      "Action": "iam:CreateUser",
      "Resource": "arn:aws:iam::123456789012:user/${aws:username}"
    },
    {
      "Sid": "AllowIAMUserDeletion",
      "Effect": "Allow",
      "Action": "iam:DeleteUser",
      "Resource": "arn:aws:iam::123456789012:user/${aws:username}"
    }
  ]
}
```

### Question: What actions are allowed for IAM users based on this policy? How are the resource ARNs constructed?

The actions allowed are to create and delete an IAM user.
The resource ARNs are constructed with the account ID and the username.

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": ["iam:Get*", "iam:List*"],
    "Resource": "*"
  }
}
```

### Questions:

- Which AWS service does this policy grant you access to?
- Does it allow you to create an IAM user, group, policy, or role?
- Go to https://docs.aws.amazon.com/IAM/latest/UserGuide/ and in the left navigation expand Reference > Policy Reference > Actions, Resources, and Condition Keys. Choose Identity And Access Management. Scroll to the Actions Defined by Identity And Access Management list.Name at least three specific actions that the **iam:Get\*** action allows.

This policy grants access to IAM service.
It allows to retrieve information about IAM users, groups, policies and roles, but not to create them.
The three specific actions that the iam:Get\* action allows are:
- iam:GetUser
- iam:GetGroup
- iam:GetPolicy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Condition": {
        "StringEquals": {
          "ec2:InstanceType": ["t2.micro", "t2.small"]
        }
      },
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Action": ["ec2:RunInstances", "ec2:StartInstances"],
      "Effect": "Deny"
    }
  ]
}
```

### Questions:

- What actions does the policy allow?

The policy allows to run and start instances of type t2.micro and t2.small.
It denies the action for all other instance types.


- Say that the policy included an additional statement object, like this **example:**

```json
{
  "Effect": "Allow",
  "Action": "ec2:*"
}
```

- How would the policy restrict the access granted to you by this additional statement?
- If the policy included both the statement on the left and the statement in question 2, could you terminate an m3.xlarge instance that existed in the account?

It would allow all actions on ec2 instances.
If the policy included both statements, you could not terminate an m3.xlarge instance that existed in the account because the first statement would deny the action. The second statement would allow all actions on ec2 instances, but the first statement would deny the action for all instance types that are not t2.micro or t2.small.

<div id='sight'/>
  
# AWS QuickSight

This part is about data analysis using AWS QuickSight. All the screenshots and details are in the AWSQuickSight folder of this repository.
