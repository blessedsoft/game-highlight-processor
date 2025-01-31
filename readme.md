## NCAA Game Highlights Processor
The NCAA Game Highlights Processor project uses RapidAPI to fetch NCAA game highlights. The project runs inside a Docker container and leverages AWS MediaConvert for media file processing.

## Key Features
- Dockerized environment for portability
- Automated highlight fetching from RapidAPI
- AWS MediaConvert for video processing
- S3 storage for raw and processed videos

# Prerequisites
- RapidAPI Account: Sign up at RapidAPI and obtain an API Key
- AWS Account: Basic knowledge of AWS IAM, S3, ECS, and Terraform
- AWS CLI Installed: Used for AWS interactions
- Docker Installation: Required for containerized execution

## Technical Diagram
<img width="443" alt="image" src="https://github.com/user-attachments/assets/603ee1c5-f4c8-455b-a264-dae919e331ab" />


## Technologies
- Cloud Provider: AWS
- Core Services: S3, IAM, ECR, ECS, MediaConvert
- Programming Language: Python 3.x
- Containerization: Docker
- Infrastructure as Code: Terraform


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
└── .gitignore

## Setup Instructions
# Clone the Repository
git clone https://github.com/blessedsoft/game-highlight-processor.git
cd src

# Store API Key in AWS Secrets Manager
aws secretsmanager create-secret \
    --name my-api-key \
    --description "API key for accessing the Sport Highlights API" \
    --secret-string '{"api_key":"YOUR_ACTUAL_API_KEY"}' \
    --region us-east-1

# Update .env File
RapidAPI_KEY=your_rapidapi_key
AWS_ACCESS_KEY_ID=your_aws_access_key_id
AWS_SECRET_ACCESS_KEY=your_aws_secret_access_key
S3_BUCKET_NAME=your_S3_bucket_name
MEDIACONVERT_ENDPOINT=https://your_mediaconvert_endpoint.amazonaws.com

# Secure the .env File
chmod 600 .env

# Build & Run Docker Container Locally
docker build -t highlight-processor .
docker run --env-file .env highlight-processor


## Review Processed Video Files
- Login to AWS Console and Navigate to the S3 Bucket.
- Ensure a JSON file exists in highlights/.
- Confirm a processed video is in videos/.

## Key Takeaways
- Working with Docker and AWS services.
- Implementing IAM best practices.
- Enhancing media quality via AWS MediaConvert.

## Future Enhancements
- Implement Terraform for Infrastructure as Code (IaC).
- Scale video processing with AWS MediaConvert.
- Modify date filters for dynamic time frames (e.g., last 30 days).
- Implement CI/CD pipelines for automation.









