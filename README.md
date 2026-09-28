CloudDrop — Secure Ephemeral File Sharing

CloudDrop is a modern, high-performance ephemeral file-sharing platform designed to make temporary file sharing simple and secure.

Users can upload files, encrypt them directly in the browser, and share them through temporary links. Uploaded files are automatically deleted after a defined expiration period, helping minimize long-term data persistence.

🚀 Features

Ephemeral Storage — Files are automatically deleted according to a 24-hour lifecycle policy.

Client-Side Encryption — Files are encrypted in the browser using AES-GCM 256-bit encryption.

Secure Short Links — Redis-backed temporary links map short aliases to encrypted file metadata.

Scalable AWS Architecture — Designed using AWS services for high availability and scalability.

Pre-Signed Downloads — S3 pre-signed URLs provide controlled access to uploaded files.

Automatic Cleanup — S3 lifecycle policies automatically remove expired files.

🏗️ System Architecture

CloudDrop uses a multi-tier AWS architecture designed for availability, scalability, and secure file delivery.

1. Edge & Web Tier

Users access the application through Amazon CloudFront.

CloudFront caches and securely delivers static assets.

Incoming traffic enters the AWS VPC through an Internet Gateway.

An Application Load Balancer (ALB) distributes requests across web servers.

Web servers are deployed across multiple Availability Zones.

2. Application Tier

The Web Layer forwards API requests through an internal load balancer.

FastAPI application servers process API requests.

Redis / Amazon ElastiCache stores temporary short-link mappings and frequently accessed metadata.

Redis reduces unnecessary database lookups and improves response times.

3. Database Tier

PostgreSQL stores persistent application metadata.

The architecture supports a primary database instance and a replica across Availability Zones.

Database records can include user session information, file metadata, and audit information.

4. Storage Layer

Uploaded files are stored in Amazon S3.

S3 lifecycle policies automatically delete expired files.

Download requests generate temporary pre-signed S3 URLs.

CloudFront can securely deliver files to users.

🛠️ Tech Stack
Backend

Python 3.11

FastAPI

Uvicorn

Frontend

HTML5

CSS3

JavaScript

AWS Infrastructure

Amazon EC2

Amazon S3

Application Load Balancer

Amazon CloudFront

Redis / Amazon ElastiCache

Amazon RDS PostgreSQL

AWS Systems Manager

Security

AES-GCM 256-bit encryption

S3 pre-signed URLs

Temporary file-sharing links

Automatic file expiration

💻 Local Development
1. Clone the repository
git clone https://github.com/Surya11205/cloud_drop.git
cd cloud_drop

2. Create a virtual environment
Linux / macOS
python -m venv venv
source venv/bin/activate

Windows
python -m venv venv
venv\Scripts\activate

3. Install dependencies

Navigate to the backend directory:

cd backend
pip install -r requirements.txt

4. Configure environment variables

Create a .env file inside the backend directory:

REDIS_HOST=localhost
REDIS_PORT=6379
S3_BUCKET_NAME=your-dev-bucket
AWS_REGION=us-east-1

# Add your database configuration when required
DATABASE_URL=your-database-url


Do not commit real AWS credentials, database passwords, API keys, or other secrets to GitHub.

5. Run the application
uvicorn main:app --reload --port 8000


Open:

http://localhost:8000

📦 Deployment

CloudDrop supports automated AWS deployment using AWS Systems Manager (SSM).

The deployment workflow can:

Package the backend application.

Upload the deployment package to Amazon S3.

Trigger an AWS Systems Manager Run Command.

Deploy the application to EC2 instances.

Restart the CloudDrop systemd service.

For detailed infrastructure and deployment instructions, see:

aws_deployment_guide.md

deploy_userdata.sh

🔐 Security Notes

CloudDrop is designed around temporary storage and controlled file access.

For production deployments:

Never commit .env files.

Never commit AWS access keys or secret keys.

Use IAM roles instead of hard-coded AWS credentials.

Restrict S3 bucket access.

Use HTTPS for production traffic.

Configure appropriate S3 lifecycle rules.

Rotate credentials and secrets regularly.

Validate uploaded files and enforce appropriate size/type limits.

📁 Project Structure

A typical project structure looks like:

cloud_drop/
├── backend/
│   ├── main.py
│   ├── requirements.txt
│   └── ...
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── script.js
├── architecture.jpg
├── aws_deployment_guide.md
├── deploy_userdata.sh
├── README.md
└── .gitignore

📄 License

This project is licensed under the MIT License.
