![CI/CD](https://github.com/draptik/hugo-demosite/workflows/CI/CD/badge.svg)

# hugo-demosite

Goal: Each git commit to master branch automatically triggers deployment to cloud provider (currently only AWS with S3).

This hugo demo project uses GitHub Actions as CI/CD pipeline. See `.github/workflows/deploy.yml` for details.

## AWS

We want to publish to an external system: The external system (AWS / S3) has to be set up correctly.

This section describes AWS prerequisites.

### Create account

- Create an AWS account.
- Create a IAM account - We will be working with the IAM account.
- Login to AWS IAM account.
- Create and download Access Key ID and Secret Access Key

### Setup AWS CLI

- Install AWS CLI.
- Setup AWS CLI using `aws configure`. Use previously downloaded Access Key Id and Secret Access Key.

### Add AWS Credentials to GitHub Project Secrets

- Within GitHub project homepage: Create 2 new 'secrets' (Note: Secrets can't be viewed or modified once their created). These secrets are used in the deployment script `.github/workflows/deploy.yml`. Settings -> Secrets -> Add a new secret
  - `AWS_ACCESS_KEY_ID`
  - `AWS_SECRET_ACCESS_KEY`

### Setup S3 Bucket

- Create a new S3 bucket
- Save the following bucket policy (replace `my-hugo-demo` with the name of your bucket) to a file (ie `bucket-policy.json`):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::my-hugo-demo/*"
        }
    ]
}
```

Usage (again: replace `my-hugo-demo` with your bucket name):

```sh
aws s3api put-bucket-policy --bucket my-hugo-demo --policy file:///location/of/your/bucket-policy.json
```

This enables public read access for the S3 bucket.

### Summary

We are now set up for deploying Hugo from Github Actions to AWS S3.

## Azure

TODO
