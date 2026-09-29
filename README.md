## TriageX.ai: Intelligent Cloud Incident Summarizer

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)
![React](https://img.shields.io/badge/react-%2320232a.svg?style=for-the-badge&logo=react&logoColor=%2361DAFB)
![Microsoft Entra ID](https://img.shields.io/badge/Entra%20ID-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

TriageX.ai is an automated, fully serverless enterprise architecture designed to drastically reduce Mean Time To Resolution (MTTR) for Cloud Support Engineers and Site Reliability Engineers (SREs). 

By ingesting complex cloud incident logs (e.g., AWS CloudWatch, Security Hub) and passing them through Amazon Bedrock (Anthropic Claude 3 Haiku), TriageX.ai generates human-readable executive summaries, identifies root causes, and provides step-by-step AWS CLI remediation commands. The platform is gated by Microsoft Entra ID to ensure secure, role-based enterprise access.

## 🏗️ Architecture Overview

The application follows an event-driven, least-privilege serverless design pattern:

1. **User Authentication:** Support Engineers log into the React SPA using Microsoft Entra ID (OIDC/OAuth2).
2. **API Routing:** The frontend passes a JWT token to Amazon API Gateway.
3. **Compute & Validation:** AWS Lambda (Python) intercepts the request, validates the Entra ID token signature, and extracts the requested incident ID.
4. **Data Retrieval:** Lambda fetches the raw JSON incident payload from Amazon DynamoDB.
5. **AI Inference:** Lambda constructs a highly engineered prompt and invokes the Amazon Bedrock API.
6. **Response:** Claude 3 Haiku processes the logs and returns a formatted markdown summary to the engineer's dashboard.

All AWS resources are strictly provisioned and managed using HashiCorp Terraform.

## 🧰 Technology Stack

* **Frontend:** React.js, Vite, Tailwind CSS
* **Authentication:** Microsoft Entra ID (MSAL)
* **API Layer:** Amazon API Gateway
* **Compute:** AWS Lambda (Python 3.10+, `boto3`)
* **Database:** Amazon DynamoDB (NoSQL)
* **Generative AI:** Amazon Bedrock (Claude 3 Haiku)
* **Infrastructure as Code (IaC):** HashiCorp Terraform

## 🚀 Prerequisites

Before deploying this project, ensure you have the following configured:

* An active **AWS Account** with administrator access.
* **Amazon Bedrock Model Access** specifically enabled for *Anthropic Claude 3 Haiku* in your deployment region.
* A **Microsoft Entra ID** tenant with permissions to create an App Registration.
* [Terraform CLI](https://developer.hashicorp.com/terraform/downloads) installed locally.
* Node.js (v18+) and npm installed.
* Python 3.10+ installed.

## 🛠️ Deployment Guide

### 1. Identity Configuration (Microsoft Entra ID)
1. Navigate to the Azure Portal and register a new Single-Page Application (SPA).
2. Set the redirect URI to your local development environment (e.g., `http://localhost:5173`).
3. Note the **Client ID** and **Tenant ID**.

### 2. Infrastructure Deployment (Terraform)
Navigate to the `terraform/` directory to provision the AWS backend.
```bash
cd terraform
terraform init
terraform plan
terraform apply -auto-approve

