---
title: "Proposal"
date: "2025-12-08"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# FindNest

## AWS Serverless AI Platform for Smart Accommodation Search

### 1. Executive Summary

The **FindNest** platform leverages **AI understanding** and **AWS Serverless architecture** to transform accommodation search into a contextual, intelligent experience. By combining **Amazon Bedrock** for natural language processing and **Amazon Location Service** for spatial analysis, it enables users to find rooms using natural queries like "affordable room near Thu Duc with gym and safe area."

The system (Frontend: React hosted on Amplify, Backend: AWS Lambda + API Gateway) stores data in DynamoDB, handles authentication via Cognito, and enriches listings with contextual insights such as food density, nearby amenities, and safety index. Notifications and OTP authentication are handled via Amazon SNS. The entire platform operates under AWS Free Tier with an estimated cost of ~$0.5/month.

### 2. Problem Statement

### What's the Problem?

Current platforms only support simple filters such as price or area and lack the ability to understand nuanced user intent. Users must manually review hundreds of listings without AI assistance or location intelligence. There’s no mechanism to understand complex preferences like _“safe neighborhood with good food options”_ or _“easy commute to District 1.”_

### The Solution

An **AI-enhanced, serverless platform** that interprets user intent through natural language, automatically enriches listings with contextual data (restaurants, safety, routes), and recommends relevant results using **Amazon Bedrock** and **Amazon Location Service**. The backend filters listings stored in DynamoDB and ranks results using AI scoring.

### Benefits and Return on Investment

- **AI Semantic Search**: Bedrock interprets user intent beyond filters.
- **Contextual Recommendations**: Listings enriched by Location Service provide real-world relevance.
- **Serverless Scalability**: Fully managed services scale automatically with zero maintenance.
- **Cost Efficiency**: All components run within AWS Free Tier for MVP phase (~$0.5/month).

### 3. Solution Architecture

The platform utilizes a modular AWS Serverless design with AI enrichment and dynamic contextual data.

| Component             | Service / Technology          |
| --------------------- | ----------------------------- |
| Frontend Hosting      | AWS Amplify (React SPA)       |
| API Backend           | AWS Lambda + API Gateway      |
| Database              | DynamoDB                      |
| File Storage          | S3                            |
| User Management       | Cognito                       |
| Notifications         | Amazon SNS                    |
| Map & Location        | Amazon Location Service       |
| Recommendation Engine | Amazon Bedrock + Lambda Logic |

![FindNest Architecture](/images/2-Proposal/AWSProject.drawio.png)

### AWS Services Used

- **AWS Lambda**: Executes backend logic including AI processing and search queries.
- **Amazon API Gateway**: Provides RESTful endpoints for client requests.
- **Amazon DynamoDB**: Stores user profiles, listings, and enriched context data.
- **Amazon S3**: Stores room images and frontend static files.
- **AWS Amplify**: Hosts and manages frontend deployment.
- **Amazon Cognito**: Manages authentication and authorization flows.
- **Amazon SNS**: Sends OTP codes and user notifications.
- **Amazon Location Service**: Fetches surrounding POIs, routes, and safety context.
- **Amazon Bedrock**: Interprets natural language search and performs semantic ranking.

### Component Design

- **Frontend Application**: React SPA hosted on Amplify for responsive, dynamic user experience.
- **API Layer**: Express app deployed on Lambda via API Gateway handling AI search, listings, and enrichment.
- **Database**: DynamoDB tables for listings, users, and search history.
- **Storage**: S3 bucket stores images; public read via signed URLs.
- **Recommendation Engine**: Bedrock interprets user queries and ranks listings.
- **Map Integration**: Amazon Location Service provides contextual location and POI visualization.
- **Notification System**: SNS delivers OTPs and alerts for new listings.
- **User Management**: Cognito handles registration, login, and secure tokens.

### 4. Technical Implementation

**Recommendation Engine Approach**

- **MVP Phase**: Bedrock interprets user query → DynamoDB filters + Location Service enrichment.
- **Expansion Phase**: Continuous enrichment jobs to compute contextual indexes (food_density, safety_score, comfort_index).
- **Advanced Phase**: Adaptive learning — store user feedback to refine Bedrock prompt responses.

**Technical Requirements**

- **Frontend**: React + Amplify UI with AI-integrated search bar and location maps.
- **Backend**: Node.js Lambda app using AWS SDK for Bedrock, DynamoDB, and Location.
- **Database**: DynamoDB tables for users, listings, and search history.
- **Storage**: S3 buckets for file storage.
- **Authentication**: Cognito + SNS OTP login flow.

### 5. Timeline & Milestones

**Project Timeline**

- **Week 1-2**: Design AWS architecture, configure Amplify, and deploy base API.
- **Week 3-4**: Implement Bedrock semantic search and Location enrichment logic.
- **Week 5**: Integrate frontend and refine AI-driven search flow.
- **Week 6**: Finalize testing and deployment.
- **Post-Launch**: Collect user data for AI improvement and feedback loops.

### 6. Budget Estimation

### Infrastructure Costs

| Component            | Service               | Estimated Cost    |
| -------------------- | --------------------- | ----------------- |
| Lambda + API Gateway | Backend               | $0.22/month       |
| DynamoDB             | Database              | $0.10/month       |
| S3                   | Storage               | $0.20/month       |
| Cognito + SNS        | Authentication + OTP  | $0.13/month       |
| Bedrock              | AI Processing         | $7.5/month        |
| Location Service     | Map & Geospatial Data | $3.30             |
| **Total**            |                       | **~$24.32/month** |

**Note**: All services operate under Free Tier usage limits during MVP phase with minimal operational cost.

### 7. Risk Assessment

#### Risk Matrix

- **Bedrock Misinterpretation**: Medium impact, medium probability.
- **Lambda Cost Spike (Scaling)**: Low impact, medium probability.
- **Incomplete Contextual Data**: Medium impact, low probability.

#### Mitigation Strategies

- **Prompt Engineering**: Optimize Bedrock input and fallback to simple filters.
- **Caching**: Cache Location and AI results for frequent queries.

#### Contingency Plans

- **Bedrock Limitations**: Fallback to DynamoDB-only filter logic.
- **Timeout Issues**: Split enrichment jobs into smaller Lambda batches.
- **High API Load**: Scale API Gateway with usage throttling.

### 8. Expected Outcomes

#### Technical Improvements

- AI-powered natural language search via Bedrock.
- Automatic contextual enrichment using Location Service.
- Scalable, low-cost infrastructure powered by AWS Serverless stack.

#### Long-term Value

- **Continuous Learning**: Improve Bedrock prompts with user feedback.
- **Smart Context Awareness**: Build dynamic profiles for regions and user habits.
- **Scalable Foundation**: Ready for integration with Amazon Personalize or Bedrock fine-tuning.
- **Cost Efficiency**: Fully serverless with minimal maintenance and no fixed servers.

---

{{% notice info %}}
[Download the Proposal](../../static/proposal.docx)
{{% /notice %}}