## My Project

## golden-ami-pipeline-with-tenable

Create a Golden AMI Pipeline integrated with a tenable Scanner for vulnerability assessments

This repo contains resources for building a Golden AMI Pipeline with [AWS Systems Manager](https://aws.amazon.com/systems-manager/), [AWS Config](https://aws.amazon.com/config/), [AWS Lambda](https://aws.amazon.com/lambda/), [AWS CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html), [AWS Service Catalog](https://aws.amazon.com/servicecatalog/), and the [tenable Scanner from AWS Marketplace](https://aws.amazon.com/marketplace/pp/B01BLHP02I?qid=1548203238792&sr=0-3&ref_=srh_res_product_title).

Here is a link to the Step-By-Step instructions guide for this work - [Golden-AMI-Pipeline-Guide](/Golden-AMI-Pipeline-Guide - Version 1.3.docx)

## Table of Contents

### Overview
Here is the architecture diagram of the Golden AMI Creation process, please refer to the [Golden-AMI-Pipeline-Guide](/Golden-AMI-Pipeline-Guide - Version 1.3.docx) for details.
[Golden-AMI-Pipeline-Guide V1.3.pdf](/Golden-AMI-Pipeline-Guide - Version 1.3.docx)

### A. Tenable setup
1. Log in to [AWS Marketplace](https://aws.amazon.com/marketplace/pp/B07J2HN7KV?qid=1571182872249&sr=0-1&ref_=srh_res_product_title) and subscribe to [Tenable.io](https://aws.amazon.com/marketplace/pp/B07J2HN7KV?qid=1571182872249&sr=0-1&ref_=srh_res_product_title). 
**_Note_**: If you're using your employer's AWS account, make sure that you consult your company's procurement team or your IT manager before purchasing a subscription on AWS Marketplace. Alternatively, you can request for a free trial on [Tenable.io](https://www.tenable.com/try).
2. Once you have credentials for [Tenable.io](www.tenable.io), go to `My Account` on your Tenable.io homepage, and click on the tab for `API Keys`.
<Add image>
3. Click `Generate` to generate your API key and secret key. Store these with you locally, so that you can use it in the next steps.
4. On the Tenable homepage, Click on `Scans`, and then Click on `Scanners` on the left navigation bar. Copy the `Linking Key` from this page, so that you can use it in the next steps.
4. Log in to [AWS Marketplace](https://aws.amazon.com/marketplace/pp/B01LXCD58S?qid=1571184129453&sr=0-1&ref_=srh_res_product_title) and subscribe to [Nessus Scanner (Pre-Authorized)](https://aws.amazon.com/marketplace/pp/B01LXCD58S?qid=1571184129453&sr=0-1&ref_=srh_res_product_title). This is a Bring Your Own License(BYOL) product, and you will use the license from Step 1 to provision this scanner.
5. <Copy from PDF or redirect> https://docs.tenable.com/other/TenableioAWSIntegrationGuide.pdf  <todo>
- scanner set up wit script etc

### B. AWS setup

Follow the guide for detailed steps on how to set this solution up.  <Link to doc>

_**Express setup with limited details below:**_
**Setup Golden AMI Pipeline infrastructure**
1. Log in to the AWS account where you want to set up the infrastructure for golden AMI pipeline with Tenable scanner installed. 
2. To set up the golden AMI pipeline infrastructure in the master account:
    1.	Click on this Launch Stack button <todo>
    2.	Sign-in to the AWS Management console using the master account’s credentials and choose CloudFormation in the Services menu.
    3.	Ensure that you are in the correct region.
    4.	Choose Create Stack.
    5.	On the Select Template page, choose Upload a template to Amazon S3.
    6.	Choose Choose File and then choose the CloudFormation template you downloaded. 
    7.	Choose Next.
    8.	On the Specify Details page, specify a Stack name and following parameters (values are case-sensitive):
            <todo>

**For distributing golden AMIs to multiple accounts**
In this step, you will sign-in to the child account and execute a CloudFormation template. The CloudFormation template will set up a cross-account role. To know more about the cross-account role, see documentation on Tutorial: Delegate Access Across AWS Accounts Using IAM Roles.  The golden AMI pipeline will assume this cross-account role to create golden AMI metadata in the child account. This guide assumes that you are distributing to only one account, however, if you have multiple child accounts, you would need to do this step in each child account.
 
 To create a cross-account role in the child account:
1.	Open the following link, choose Raw and then download the JSON file to your computer: https://github.com/aws-samples/golden-ami-pipeline-with-tenable/blob/master/Golden-AMI-Cross-Account-Role.json
2.	Sign-in to the AWS Management console using child account’s credentials and choose CloudFormation in the Services menu.
3.	Ensure that you are in the correct region.
4.	Choose Create Stack.
5.	On the Select Template page, choose Upload a template to Amazon S3.
6.	Choose Choose File and then choose the CloudFormation template you downloaded. 
7.	Choose Next.
8.	On the Specify Details page specify following:
a.	Stack Name as Golden-AMI-Cross-AccountRole-Cost-Center 
b.	roleName as goldenAMICrossAccountRole-Cost-Center
c.	parentAWSAccountID as Master-Account-ID
9.	Choose Next.
10.	 On the Options page, specify following key-value pairs as Tags:
a.	Key as Cost-Center and corresponding value as Cost-Center provided to you. 
b.	Key as Generated-By and corresponding value as Golden-AMI-Pipeline.
11.	Choose Next.
12.	On the Review page, choose the check-box next to the following message: 
“I acknowledge that AWS CloudFormation might create IAM resources with custom names.”
13.	Choose Create. The CloudFormation template creates an AWS Identity and Access Management (IAM) cross-account role.

**Set up a compliance check in the child account(s)**
You need to set up AWS Config in child account’s region if it is not set up. To know more about how to set up AWS Config, see documentation on Setting up AWS Config with the Console. While setting up the AWS Config for this guide, you do not need to select any of the existing rules.  

Next, you will run a CloudFormation template to create an AWS Config rule to flag any EC2 instances not created from the golden AMIs as non-compliant. 

Note
If you are distributing your golden AMI to multiple accounts/regions, you will need to run the CloudFormation Template in each child account in each region in which an EC2 instance of a golden AMI will be launched. AWS CloudFormation Stacksets provide an elegant way to execute a single CloudFormation template in multiple accounts in multiple regions simultaneously. To know more about CloudFormation Stacksets, see documentation on Working with AWS CloudFormation StackSets.  Before you execute the above CloudFormation Template using Stacksets, ensure that you have performed stacksets account setup in parent as well as all child accounts.

This guide assumes that you will distribute the golden AMI to only one account and hence run the CloudFormation template only once. To set up the rule:
1.	Open the following link, choose Raw and then download the JSON file to your computer.  https://github.com/aws-samples/golden-ami-pipeline-with-tenable/blob/master/Golden-AMI-Compliance-CFT.json
2.	Sign-in to AWS Management console using child account’s credentials and choose CloudFormation in the Services menu.
3.	Ensure that you are in the correct region (the region in which end-user will deploy an EC2 instance of the golden AMI).
4.	Choose Create Stack.
5.	On the Select Template page, choose Upload a template to Amazon S3.
6.	Choose Choose File and then choose the CloudFormation template you downloaded.
7.	Choose Next.
8.	On the Specify Details page, specify:
a.	Stack Name as Compliance-Cost-Center. 
b.	Specify PathToSSMParameter as /GoldenAMI/latest. This is the SSM parameter path on which comma-separated list of active golden AMIs will become available.
9.	Choose Next.
10.	On the Options page, specify following key-value pairs as Tags:
a.	Key as Cost-Center and corresponding value as Cost-Center provided to you. 
b.	Key as Generated-By and corresponding value as Golden-AMI-Pipeline.
11.	Choose Next.
12.	On the Review page, choose the check-box next to the following message: “I acknowledge that AWS CloudFormation might create IAM resources.”
13.	Choose Create.  

The rule triggers an AWS Lambda function which reads the list of active AMIs from a parameter in the SSM parameter store and marks any instance that has an AMI-ID that does not match with any of the active golden AMIs, as non-compliant.  The AWS Config rule gets evaluated on following occasions:
    1.	Whenever there is any configuration change detected in any EC2 instance in that region.
    2.	Every one hour. This is necessary to update compliance results once the list of active golden AMIs has changed (due to creation/decommissioning of a golden AMI)


### C. Golden AMI Pipeline Execution
To create a golden AMI:
1.	Sign-in to the AWS Management Console using master AWS account credentials and then navigate to Systems Manager service. 
2.	Ensure that you are in the correct region.
3.	In the navigation panel, choose Automation under Actions drop-down.
4.	Choose Execute Automation.
5.	Filter automations visible by choosing owned by me filter option.
6.	Choose the GoldenAMIAutomationDoc document name that you noted in the output tab of CloudFormation stack, in Step 3. 
7.	Choose following values:
a.	Document version as the latest version at runtime.
b.	Leave execution mode as it is. 
c.	Most input parameters will be pre-populated except the sourceAMIid and AMIVersion. If you plan to change the input parameters, make sure they are reflected in Step 9 (7c) as well.
d.	Specify appropriate values for following parameters (values are case-sensitive):
<todo>

1.	You can leave remaining parameters as it is. Automation document launches instances in a private subnet, with a security group that has no inbound access for launching instances. 
2.	Next, choose Execute Automation.
You have successfully triggered creation of a golden AMI for a product. The output product can be a standardized OS that you want to distribute to your teams or it can be an application specific AMI that has agents, code, and necessary tools built in. Every time you create a new version of the golden AMI, ensure that you have specified the consistent product-name as well as the operating system. 

**For additional details, refer <todo> guide**


## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.


## License

This library is licensed under the MIT-0 License. See the LICENSE file.

