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
![web_files](images\aws_images\web_file.PNG)
4. Uder the properties tab, Click on "Static website hosting" and choose "Use this bucket to host a website  after clicking the edit option. ![static](images\aws_images\static.PNG)
Under the edit button, I indicated the landing page of my website which is the index.html file. I did not use an error file in this project hence was left blanl.
![index](images\aws_images\index.PNG)
5. Under the permissions menu, public access is enabled for the backet.![public](images\aws_images\public.PNG)
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
![acl](images\aws_images\acl.PNG)

Access Control List was edited to suit the project as indicated below
![acl_more](images\aws_images\Control.PNG)

The deafult website acess point is given as *http://qerbros.com.s3-website-us-east-1.amazonaws.com*


## Configuring Route 53 
Amazon Route 53 is a highly scalable and reliable cloud-based Domain Name System (DNS) web service offered by AWS (Amazon Web Services). It provides developers and businesses with a way to route end-users to Internet applications by translating human-readable domain names (such as www.example.com) into the IP addresses (such as 192.0.2.1) that computers use to identify each other on the Internet.

Route 53 is designed to provide a fast and reliable way to route traffic to web applications by leveraging the vast infrastructure of AWS. It can be used to register domain names, create DNS zones, and manage the traffic routing policies for your web applications. It also provides advanced features such as traffic flow management, health checks, and DNS failover, which can help you ensure the availability and reliability of your web applications.

In summary, Amazon Route 53 is a powerful DNS service that can be used to manage your domain names and route traffic to your web applications. It is a key component of the AWS ecosystem and is widely used by developers and businesses to manage their web infrastructure on the cloud.

In this project however, the Domain name was bought from GoDaddy. Route 53 was used to create a hosted Zone for which the name servers for replaced in Godaddy with the ones provided by the Route 53 Hosted Zone.

In configuring the Route 53, follow the steps stipulated below:

1. Log in to your AWS account and navigate to the Route 53 console.
![route53_dashboard](images\aws_images\reoute53.PNG)

2. Click on "Hosted Zones" in the left-hand menu and then click on the "Create Hosted Zone" button.
3. Enter your domain name (qerbros.com) and click "Create Hosted Zone.". Ensure you select public zone since it will be assessible via the internet.
![route53_dashboard](images\aws_images\hosted_zone.PNG)
4. You will be redirected to the "Record Sets" tab. Click on "Create Record Set."

5. Leave the name field blank, select "A - IPv4 address" for the type, and choose "Yes" for "Alias."
6. In the "Alias Target" field, select your S3 bucket from the dropdown list.
7. Click on "Create" to create the record set.

![route53_dashboard](images\aws_images\arecord.PNG)

8. Go back to your S3 bucket and click on the "Properties" tab.
9. Click on "Static Website Hosting" and copy the Endpoint URL.
```
http://qerbros.com.s3-website-us-east-1.amazonaws.com

```
10. Go back to the Route 53 console, click on "Create Record Set" again, and enter "www" for the name.
11. Select "CNAME - Canonical name" for the type, and leave the "Alias" field as "No."
12. In the "Value" field, paste the Endpoint URL of your S3 bucket (e.g. "my-bucket.s3-website-us-west-2.amazonaws.com"). 
![route53_dashboard](images\aws_images\cname1.PNG)
13. Click on "Create" to create the record set.
![route53_dashboard](images\aws_images\records.PNG)
The above image containes all the records created for the hosted zone. Te NS (Name Server) record containes the name servers that will be doing the resolution. They are stated below:
```
ns-873.awsdns-45.net.
ns-1383.awsdns-44.org.
ns-1767.awsdns-28.co.uk.
ns-66.awsdns-08.com.
```

Since my domain name was bought on Godaddy, I set these NS values in the GoDaddy NS settings as show below.
![route53_dashboard](images\aws_images\godaddy.PNG)



