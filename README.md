This project demonstrates a serverless asset delivery pipeline built with Amazon S3 and Amazon CloudFront to deliver media assets globally with low latency, high availability, and enhanced security. It uses CloudFront Origin Access Control (OAC) to keep the S3 origin bucket private, restrictive bucket policies to allow access only through the authorized CloudFront distribution, and S3 lifecycle rules to manage website assets efficiently.

# Architecture Overview

                      [ Internet Users ]
                               │
                               │ HTTPS (Port 443)
                               ▼
                     ┌────────────────────┐
                     │  Amazon CloudFront │
                     │   (Edge Network)   │
                     └─────────┬──────────┘
                               │
                               │ Origin Access Control (OAC)
                               ▼
                     ┌────────────────────┐
                     │ Private S3 Bucket  │
                     │ (Media & Web Files)│
                     └────────────────────┘

# What each part of the pipeline does

- Amazon S3 stores the original media assets and static website files

- S3 Block Public Access prevents public bucket and object access

- S3 bucket policy grants the designated CloudFront distribution permission to read objects

- CloudFront OAC authenticates CloudFront requests to the private S3 origin

- Amazon CloudFront delivers files globally and caches them at edge locations

- S3 lifecycle rules transitions or deletes objects according to defined retention policies



# Deployment Steps

## Create the Private S3 Bucket and Upload Content

- Provision a private S3 bucket with **Block All Public Access** and **Bucket Versioning** enabled. Block All Public Access prevents public access to the bucket, while the S3 bucket policy restricts access to the authorized CloudFront distribution using Origin Access Control (OAC). Bucket Versioning preserves previous versions of objects, enabling recovery from accidental deletions or overwrite

![screenshots](media-portfolio/media-portfolio-private-s3bucket.png)

![screenshots](media-portfolio/media-portfolio-s3bucket-versioning.png)

- Upload a static web application consisting of index.html which serves as the entry point and main page of the application, error.html which provides a custom error page for displaying error messages, and an assets/ folder that contains the application's static media files

![screenshots](media-portfolio/media-portfolio-s3-objects.png)

## Set Up CloudFront with Origin Access Control (OAC)

- Create a CloudFront distribution with Origin Access Control(OAC) configured for the S3 bucket origin. Configure the distribution to redirect HTTP requests to HTTPS to ensure secure access to the website, and set index.html as the default root object so that it is served when users access the distribution's root URL

![screenshots](media-portfolio/media-portfolio-cloudfront-oac.png)

![screenshots](media-portfolio/media-portfolio-viewer-protocol-policy.png)

![screenshots](media-portfolio/media-portfolio-default-root-object.png)

- Ensure that the S3 bucket policy grants access only to the authorized CloudFront distribution 

```
{
	"Version": "2008-10-17",
	"Id": "PolicyForCloudFrontPrivateContent",
	"Statement": [
		{
			"Sid": "AllowCloudFrontServicePrincipal",
			"Effect": "Allow",
			"Principal": {
				"Service": "cloudfront.amazonaws.com"
			},
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::media-portfolio-mosun-o/*",
			"Condition": {
				"ArnLike": {
					"AWS:SourceArn": "arn:aws:cloudfront::834863651662:distribution/E161E9SY7IYPEM"
				}
			}
		}
	]
}
```



















