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


- Provision a private S3 bucket with **Block All Public Access** and **Bucket Versioning** enabled. Block All Public Access prevents public access to the bucket, while the S3 bucket policy restricts access to the authorized CloudFront distribution using Origin Access Control (OAC). Bucket Versioning preserves previous versions of objects, enabling recovery from accidental deletions or overwrite

  



























