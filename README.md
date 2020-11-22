# UT1 Demo React

This project is the base for my presentation of a CI/CD pipeline. Using GitHub Actions, the application can deploy a static React site to an AWS S3 bucket.  

## Prerequisites           

* An AWS Account
* A fork of this repository set to private (WARNING : For security purposes make sure your secrets are stored in the repository settings and not in code)
* AWS secrets configured in the repository

## Creating a user

* Naviagte to AWS and the **IAM** service
* Select **Add User**
* Give your user a name (ex : demo-pipeline-githubactions) and select **Programmatic Access** as the access type
* Naviagte to **Permissions** and **Attach an exisiting policies directly**
* Select **AmazonS3FullAccess** as the policy. This ensures that your user has the appropriate access rights for it's purpose
* Add tags if necessary
* Create the User and download the CSV file if necessary. Alternatively copy the **Access Key ID** and **Secret Access Key** for the next steps

## Creating the S3 Bucket

* Naviagte to AWS and the **S3** service
* Select **Create a Bucket*
* Provide a bucket name and preferred region. Note that all bucket names must be unique
* Untick **Block all public access**.
* Click on **Create Bucket**

## Configuring the S3 Bucket for static website hosting

* Navigate to your S3 buckets and select the bucket you just created
* Select the **Properties** tab
* Under **Static website hosting** select Edit
* Choose Use this bucket to host a website.
* Under **Static website hosting** choose **Enable**
* In Index document, enter the file name of the index document, typically **index.html**
* Click on **Save changes**

## Updating the S3 Bucket Policy

* Navigate to your S3 bucket
* Select the **Permissions** tab
* Under **Bucket Policy** select **Edit**
* Add the following JSON Bucket policy : 

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::example.com/*"
            ]
        }
    ]
}
```
* Note : ake sure to update the **Example.com** with the name of your S3 Bucket
* Save the changes

## Adding secrets to your repository

The GitHUb Actions workflow utilizes three secret variables : 

1. AWS_S3_BUCKET: Your AWS Bucket Name
2. AWS_ACCESS_KEY_ID: Your AWS User Access Key ID
3. AWS_SECRET_ACCESS_KEY: Your AWS User Secret Key
4. AWS_REGION: Your prefered AWS Region (ex : eu-west-3 for the Paris region)

* With the previously downloaded csv file or copied variables naviagte to your GitHub repository
* Navigate to the **Secrets** tab
* Click on **New repository secret**
* Add the variable names and associated secret values for each of the parameters

## Running the pipeline

When commiting changes the pipeline will run automatically on the branch named **main**. The **main.yml** file can be updated to follow a GitFlow  or Trunk based Develoment workflow. 



