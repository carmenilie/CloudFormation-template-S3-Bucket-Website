# CloudFormation-template-S3-Bucket-Website
This repository contains the steps I took in creating a CloudFormation template for an S3 bucket Website.

**Step 1** - Create a new .yaml file, give it an appropriate name (in my case, it is called template.yaml), and fill it with the information below:

```sh
AWSTemplateFormatVersion: 2010-09-09
Description: A simple CloudFormation template
Resources:
    Bucket:
        Type: AWS::S3::Bucket
        Properties:
            BucketName: carmenilie-s3-bucket
```
*Note:* The *BucketName* property needs to be unique, otherwise trying to create the stack will result in failure.

**Step 2** - Deploy the stack

To deploy the simple template that was just created using AWS CLI, the following command must be entered:

```sh
aws cloudformation deploy --template-file template.yaml --stack-name static-website
```

In order to check the deployment status, open the AWS Console and check the state of the stack by opening CloudFormation. The task was successful if the state shows the *CREATE COMPLETE* message. Then, by navigating to Services and selecting S3 under the scroll-down pannel, the newly created S3 bucket can be visualised.

**Step 3** - After the successful creation of a bucket, create a new .html file that will serve as the new website. Give the file an appropriate name (in my case, it is called index.html). Fill it in with any desired information. In my simple example, I entered the following text:

```html
<html>
  <body>
    <h1>My First Site Deployed Using CloudFormation</h1>
    <p>Hello! This site was deployed using CloudFormation.</p>
  </body>
</html>
```

Upload the file to the previously created bucket. To do this, the next command can be used:

```sh
aws s3 cp .\index.html s3://carmenilie-s3-bucket/
```

**Step 4** - I changed the template.yaml file as below in order to enable website hosting:

```sh
AWSTemplateFormatVersion: 2010-09-09
Description: A simple CloudFormation template
Resources:
    Bucket:
        Type: AWS::S3::Bucket
        Properties:
            BucketName: carmenilie-s3-bucket
            AccessControl: PublicRead
            WebsiteConfiguration:
                IndexDocument: index.html
Outputs:
    WebsiteURL:
        Value: !GetAtt [Bucket, WebsiteURL]
        Description: URL for website hosted on S3
```

Deploy the new changes by running the same command as in **Step 2**:

```sh
aws cloudformation deploy --template-file template.yaml --stack-name static-website
```

**Step 5** - Confirm the result by checking the CloudFormation console. Once the stack's state shows the *UPDATE_COMPLETE* message, check the Outputs tab, where the *WebsiteUrl* was added. Click on the URL provided and visualise the site in your browser.

## Bilbiography
https://adamtheautomator.com/aws-cli-cloudformation/
https://docs.aws.amazon.com/AmazonS3/latest/userguide/upload-objects.html
