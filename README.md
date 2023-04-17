# HOSTING STATIC WEBSITE ON AWS SIMPLE STORAGE SERVICE (S3)
## Introduction
Hosting a static website on the cloud has become a popular choice for many individuals and businesses today. It offers scalability, flexibility, and cost-effectiveness that traditional hosting options can't match. Amazon Web Services (AWS) provides an excellent platform for hosting static websites through its Simple Storage Service (S3).



*A static website is a website that consists of fixed web pages that are displayed to the user exactly as they were created. These web pages are typically created using HTML, CSS, and other front-end web development technologies. Unlike dynamic websites, which generate content on the fly using server-side scripting languages, static websites have pre-built pages that are stored on a server and sent to the user's web browser upon request.*

**Some Benefits of Using S3 and Route 53 for Hosting the Website**
+ Cost-effective: Amazon S3 and Route 53 are both very cost-effective solutions for hosting static websites. You only pay for the storage and bandwidth that you use, and there are no upfront costs or minimum fees.

+ Scalability: S3 and Route 53 are both highly scalable services, meaning they can easily handle a large volume of traffic and data without any performance issues.

+ Reliability: Both services offer high availability and durability, with multiple backups and redundancies built in to ensure your website is always accessible.

+ Security: S3 and Route 53 both offer robust security features, such as encryption at rest and in transit, access control, and data protection.

+ Easy to use: Setting up and managing a static website on S3 and Route 53 is relatively easy and straightforward, even for non-technical users.

+ Custom domain support: Route 53 makes it easy to map your custom domain name to your S3 bucket, allowing you to use your own branded domain for your static website.



In this project, I will be focusing on how to host a static website on S3 and link it to a domain name [qerbos.com](qerbros.com) purchased from [GoDaddy.com](GoDaddy.com.) Additionally, I will be using Amazon Route 53 to create a hosted zone for our domain name, which will allow us to map our domain name to the S3 bucket hosting our website.

This project will provide you with a step-by-step guide to creating and configuring your S3 bucket, setting up your hosted zone on Route 53, and linking the two together to create a functional website.


## Setting up S3 bucket for hosting the static website
A series of processes were caried out in setting up the S3 for this purpose.
1. Log in to your AWS account and navigate to the S3 console. ![S3 Console](images\aws_images\s3_console.PNG)
2. Click the "Create bucket" button and enter a unique name for your bucket, this case it is qerbros.com since the bucket name must match the domain name. 
![qerbros_bucket_name](images\aws_images\bucket_name.PNG). In creating the bucket, all settings were left to be deafult. The region of choice is US East (N. Virginia) us-east-1 as seen in the image above.
3. Upload the website files into the bucket as seen below
![qerbros_bucket_name](images\aws_images\web_file.PNG)
4. Uder the properties tab, Click on "Static website hosting" and choose "Use this bucket to host a website  after clicking the edit option. ![qerbros_bucket_name](images\aws_images\static.PNG)
Under the edit button, I indicated the landing page of my website which is the index.html file. I did not use an error file in this project hence was left blanl.
![qerbros_bucket_name](images\aws_images\index.PNG)
5. Under the permissions menu, public access is enabled for the backet.![qerbros_bucket_name](images\aws_images\public.PNG)
 A bucket policy was then created. The policy is as witten bellow
 ```
 {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::qerbros.com/*"
        }
    ]
}
```
The object ownership was changed to ACLs enabled
![qerbros_bucket_name](images\aws_images\acl.PNG)

Access Control List was edited to suit the project as indicated below
![qerbros_bucket_name](images\aws_images\Control.PNG)


