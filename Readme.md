# Deploy Static Website on AWS using S3, CloudFront, and IAM Policy.

## Pre-requisites:
<a href="https://github.com/arabog/launch-and-connect-to-ec2" target="_blank">Launch and Connect to an Elastic Compute Cloud(EC2) instance</a>  
<a href="https://github.com/arabog/IAM-Policy" target="_blank">IAM policy</a>  
<a href="https://github.com/arabog/EC2-Auto-Scaling-Group" target="_blank">EC2 auto scaling grp</a>    
<a href="https://github.com/arabog/Elastic-Load-Balancing" target="_blank">Elastic Load Balancers</a> 

<a href="https://github.com/arabog/simple-notification-service" target="_blank">SNS</a>  
<a href="https://github.com/arabog/simple-queue-service" target="_blank">SQS</a>  
<a href="https://github.com/arabog/elastic-container-service-cluster" target="_blank">Elatic Container Service: Create a Cluster</a>  
<a href="https://github.com/arabog/cloudTrail" target="_blank">Cloud Trail</a>  

<a href="https://github.com/arabog/cloudWatch" target="_blank">Cloud Watch</a>  
<a href="https://github.com/arabog/CloudFormation" target="_blank">Cloud Formation</a>  
<a href="https://github.com/arabog/aws-cli" target="_blank">AWS Command Line Interface(CLI)</a>  
<a href="https://github.com/arabog/aws-cli-and-s3-bucket" target="_blank">Interacting with S3 bucket using CLI</a>  

# Introduction
Hosting static websites that only include HTML, CSS, and JavaScript files that require no server-side processing. The whole project has two major intentions to implement:  
Hosting a static website on S3 and
Accessing the cached website pages using CloudFront, a content delivery network (CDN) service. which offers low latency and high transfer speeds during website rendering.  

*Note that Static website hosting essentially requires a public bucket, whereas the CloudFront can work with public and private buckets.*  

## Create S3 Bucket
![bucket1](bucket1.png?raw=true "bucket1")


## Upload files to S3 Bucket
Do not select the udacity-starter-website folder. Instead, upload its content one-by-one. Upload the files and folder such dt the `index.html`
is in the root folder.

![bucket2](bucket2.png?raw=true "bucket2")

*Or use code*  
![bucket3](bucket3.png?raw=true "bucket3")

## Secure Bucket via IAM
{
    "Version":"2012-10-17",
    "Statement":[
        {
            "Sid":"AddPerm",
            "Effect":"Allow",
            "Principal": "*",
            "Action":["s3:GetObject"],
            "Resource":["arn:aws:s3:::your-website/*"]
        }
    ]
}  

This step is required for static website hosting.  

Note - If we were not learning about static website hosting, we could have made the bucket private and wouldn't have to specify any bucket access policy explicitly. In such a case, once we set up the CloudFront distribution, it will automatically update the current bucket access policy to access the bucket content. The CloudFront service will make this happen by using the Origin Access Identity user.  

## Configure S3 Bucket
![bucket4](bucket4.png?raw=true "bucket4")


![bucket5](bucket5.png?raw=true "bucket5")


## Distribute Website via CloudFront
Domain Name	Don't select the bucket from the dropdown list. Paste the Static website hosting endpoint of the form   
<bucket-name>.s3-website-region.amazonaws.com  

From the CloudFront dashboard, click “Create Distribution”.  

![bucket7](bucket7.png?raw=true "bucket7")

![bucket8](bucket8.png?raw=true "bucket8")

Leave the defaults for the rest of the options, and click “Create Distribution”. It may take up to 10 minutes for the CloudFront Distribution to get created.  

![bucket9](bucket9.png?raw=true "bucket9")


## Access Website in Web Browser
https://d10n1yblhthtc8.cloudfront.net  

![bucket10](bucket10.png?raw=true "bucket10")  

Access the website via website-endpoint, such as http://<bucket-name>.  s3-website.us-east-2.amazonaws.com/  

Access the bucket object via its S3 object URL, such as,   
https://<bucket-name>.s3.amazonaws.com/index.html

All three links: CloudFront domain name, S3 object URL, and website-endpoint will show you the same index.html content.  
