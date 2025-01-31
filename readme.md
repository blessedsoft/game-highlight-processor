## NCAA Game Highlights Processor
The NCAA Game Highlights Processor project uses RapidAPI to fetch NCAA game highlights. The project runs inside a Docker container and leverages AWS MediaConvert for media file processing.

# File Overview

- config.py

1. Loads environment variables and assigns default values.

2. Allows configuration for different environments (development, staging, production).

- fetch.py

1. Determines the date and league (NCAA) for fetching highlights.

2. Retrieves highlights via API and stores them as basketball_highlight.json in an S3 bucket.

- process_one_video.py

1. Reads the JSON file from the S3 bucket.

2. Extracts the first video URL and downloads it into memory.

3. Stores the video in the S3 bucket (videos/ folder).

4. Logs the process.

- mediaconvert_process.py

1. Configures and submits an AWS MediaConvert job.

2. Adjusts codec, resolution, bitrate, and audio settings.

3. Saves the processed video in the S3 bucket.

- run_all.py
Runs all scripts sequentially, allowing buffer time for execution.

- .env
Stores sensitive environment variables securely.

- Dockerfile
Defines build instructions for the Docker image.


## Prerequisites

1. Create a RapidAPI Account

- Sign up on RapidAPI.

- Use the Sports Highlights API (basic plan is free).

2. Verify Installed Dependencies

# Check Docker version
docker --version

# Check AWS CLI version
aws --version

# Check Python3 version
python3 --version

3. Retrieve AWS Account ID

- Log in to AWS Management Console.

- Copy your AWS Account ID from the top-right corner.

4. Retrieve AWS Access Keys

- Navigate to IAM → Users → Security Credentials.

- Generate an Access Key ID and Secret Access Key.


## Technical Diagram
<img width="443" alt="image" src="https://github.com/user-attachments/assets/603ee1c5-f4c8-455b-a264-dae919e331ab" />

# Project Structure
src/
├── Dockerfile
├── config.py
├── fetch.py
├── mediaconvert_process.py
├── process_one_video.py
├── requirements.txt
├── run_all.py
├── .env
├── .gitignore
└── .gitignore

# Getting Started
Step 1: Clone the Repository
git clone https://github.com/alahl1/NCAAGameHighlights.git
cd src

Step 2: Store API Key in AWS Secrets Manager

aws secretsmanager create-secret \
    --name my-api-key \
    --description "API key for accessing the Sport Highlights API" \
    --secret-string '{"api_key":"YOUR_ACTUAL_API_KEY"}' \
    --region us-east-1

Step 3: Create an IAM Role or User

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "ec2.amazonaws.com",
          "ecs-tasks.amazonaws.com",
          "mediaconvert.amazonaws.com"
        ],
        "AWS": "arn:aws:iam::<your-account-id>:user/<your-iam-user>"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}

Step 4: Update .env File

RapidAPI_KEY=your_rapidapi_key
AWS_ACCESS_KEY_ID=your_aws_access_key_id
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key
S3_BUCKET_NAME=your_S3_bucket_name
MEDIACONVERT_ENDPOINT=https://your_mediaconvert_endpoint.amazonaws.com

Step 5: Secure the .env File

chmod 600 .env

Step 6: Build & Run Docker Container Locally

docker build -t highlight-processor .
docker run --env-file .env highlight-processor

Key Learnings

Working with Docker and AWS services.

Implementing IAM best practices.

Enhancing media quality via AWS MediaConvert.

Future Improvements

Implement Terraform for Infrastructure as Code (IaC).

Scale video processing with AWS MediaConvert.

Modify date filters for dynamic time frames (e.g., last 30 days).

Part 2 - Terraform Bonus

Initialize Terraform

cd terraform
terraform init
terraform validate
terraform plan
terraform apply -var-file="terraform.dev.tfvars"

Build and Push Docker Image to AWS ECR

docker build -t highlight-pipeline:latest .
docker tag highlight-pipeline:latest <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/highlight-pipeline:latest

Authenticate and Push to ECR

aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

docker push <AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/highlight-pipeline:latest

Clean Up ECS and ECR Resources

bash ncaaprojectcleanup.sh

Reviewing Processed Video Files

Navigate to the S3 Bucket.

Ensure a JSON file exists in highlights/.

Confirm a processed video is in videos/.

Additional Learnings

Deploying local Docker images to AWS ECR.

Understanding Terraform configurations.

Setting up networking: VPCs, subnets, internet gateways.

Managing secrets securely with AWS SSM.

Future Enhancements

Automate VPC and networking setup.

Improve media processing pipeline for efficiency.


