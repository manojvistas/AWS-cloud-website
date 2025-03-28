# AWS-Based Website Hosting with CI/CD 

This guide helps you **host a static website on AWS S3**, secure it with **CloudFront**, and automate deployments using **GitHub Actions**—all **without paying for Route 53**.

## 🛠 Steps to Deploy

### 1️⃣ Host Website on S3
1. Create an **S3 Bucket** (e.g., `my-cloud-website`).
2. Enable **Static Website Hosting** (Properties → Static Website Hosting → Enable).
3. Upload your **website files** (`index.html`, `style.css`, etc.).
4. Set **Bucket Policy** to allow public access:

   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Principal": "*",
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::your-bucket-name/*"
           }
       ]
   }Access your site via:

http://your-bucket-name.s3-website-region.amazonaws.com




---

2️⃣ Secure with CloudFront (CDN & HTTPS)

1. Create a CloudFront Distribution (Origin → Select S3 Bucket).


2. Set Viewer Protocol Policy to Redirect HTTP to HTTPS.


3. Get the CloudFront URL:

https://d3example.cloudfront.net


4. (Optional) Use a Free Domain (e.g., Freenom or GitHub Pages).




---

3️⃣ Automate Deployments with GitHub Actions

1. Push your website files to a GitHub Repository.


2. Add a GitHub Actions workflow:

name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          region: us-east-1

      - name: Sync files to S3
        run: aws s3 sync . s3://your-bucket-name --delete


3. Commit & push → Deployments are now automated! 🚀

