## Golden AMI Pipeline with Tenable.io

Create a Golden AMI Pipeline integrated with a Tenable.io Scanner for vulnerability assessments of your AWS Amazon Machine Images(AMIs), and store approved images as Golden AMIs. This repository will also help you set up continuous monitoring of Amazon EC2 instances in your environment, and alert you in case an unapproved AMI is running. 

This repo contains resources for building a Golden AMI Pipeline with [AWS Systems Manager](https://aws.amazon.com/systems-manager/), [AWS Config](https://aws.amazon.com/config/), [AWS Lambda](https://aws.amazon.com/lambda/), [AWS CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html), [AWS Service Catalog](https://aws.amazon.com/servicecatalog/), and the [tenable Scanner from AWS Marketplace](https://aws.amazon.com/marketplace/pp/B01BLHP02I?qid=1548203238792&sr=0-3&ref_=srh_res_product_title).


## Overview
Here is the architecture diagram of the Golden AMI Creation process. For more details, please refer to the [Golden AMI Pipeline Set Up Guide](/resources/docs/Golden-AMI-Pipeline-Guide-Version1.3.docx).

![architecture-diagram](/resources/images/arch-diagram.png)

### A. Tenable setup
1. Log in to [AWS Marketplace](https://aws.amazon.com/marketplace/pp/B07J2HN7KV?qid=1571182872249&sr=0-1&ref_=srh_res_product_title) and subscribe to [Tenable.io](https://aws.amazon.com/marketplace/pp/B07J2HN7KV?qid=1571182872249&sr=0-1&ref_=srh_res_product_title). 
**_Note_**: If you're using your employer's AWS account, make sure that you consult your company's procurement team or your IT manager before purchasing a subscription on AWS Marketplace. Alternatively, you can request for a free trial on [Tenable.io](https://www.tenable.com/try).

![awsmp-tenable](/resources/images/awsmp-tenable.png)



2. Once you have credentials for [Tenable.io](www.tenable.io), go to `My Account` on your Tenable.io homepage, and click on the tab for `API Keys`.

![architecture-diagram](/resources/images/api-key.png)


3. Click `Generate` to generate your API key and secret key. **Store these with you locally**, so that you can use it in the next steps.

4. On the Tenable homepage, click on `Scans`, and then click on `Scanners` on the left navigation bar. Copy the `Linking Key` from this page and **store these with you locally**, so that you can use it in the next steps.
![architecture-diagram](/resources/images/linking-key.png)

5. Log in to [AWS Marketplace](https://aws.amazon.com/marketplace/pp/B01LXCD58S?qid=1571184129453&sr=0-1&ref_=srh_res_product_title) and subscribe to [Nessus Scanner (Pre-Authorized)](https://aws.amazon.com/marketplace/pp/B01LXCD58S?qid=1571184129453&sr=0-1&ref_=srh_res_product_title). This is a Bring Your Own License(BYOL) product, and you will use the license from Step 1 to provision this scanner.

6. Please refer to the [AWS set up guide](https://docs.tenable.com/other/TenableioAWSIntegrationGuide.pdf) provided by Tenable to set up the pre-authorized scanner in your environment. Note the `Scanner Name` that you created using this process.

### B. AWS setup

For Step-By-Step instructions guide for this work, please refer [Golden AMI Pipeline Set Up Guide](/resources/docs/Golden-AMI-Pipeline-Guide-Version1.3.docx).

## License Summary
This sample code is made available under a modified MIT-0. See the [LICENSE](LICENSE) file.

## Contributing
Your contributions are always welcome! Please have a look at the [contribution guidelines](CONTRIBUTING.md) first. :tada:

## License
This library is licensed under the MIT-0 License. See the [LICENSE](LICENSE) file.

