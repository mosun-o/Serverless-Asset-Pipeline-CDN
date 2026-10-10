## Live Demo

Access the application website served through Amazon CloudFront over HTTPS: [Visit the Website](https://dtwnywmybuh6j.cloudfront.net/)

## S3 Access Forbidden (403)

Attempt to access the S3 bucket directly to verify that a 403 Forbidden response is returned, confirming that public access is blocked and the bucket remains private

## CloudFront Success (HTTP 200)

This confirms that CloudFront successfully retrieves and delivers the website content from the S3 origin to users


## S3 Bucket Policy

The S3 bucket policy restricts access to the authorized CloudFront distribution through Origin Access Control (OAC), keeping the bucket private and preventing direct public access









