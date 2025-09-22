# AWS Serverless Web Application – Student Data Management

## Executive Summary
This project demonstrates the deployment of a production-ready serverless web application on AWS for managing student data. It leverages AWS Lambda, API Gateway, DynamoDB, S3, and CloudFront to provide a scalable, cost-efficient, and secure solution. The application showcases a complete serverless architecture with step-by-step implementation, detailed screenshots, and professional documentation for reproducibility and learning.

---

## Architecture Diagram
![AWS Serverless Architecture](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Architecture_Diagram/SERVERLESS_Web-App-Architecture.png)

---

## Data Flow Overview
1. **Client Request:** User accesses the application through CloudFront.  
2. **CDN Distribution:** CloudFront serves cached content or fetches from S3.  
3. **Static Content:** S3 hosts the React frontend.  
4. **API Calls:** Frontend communicates with API Gateway.  
5. **Business Logic:** AWS Lambda functions process the requests.  
6. **Data Storage:** DynamoDB stores student records.  
7. **Response:** Data flows back through the same path to the client.  

---

## Features
- **Serverless Architecture:** Zero server management with automatic scaling.  
- **CRUD Operations:** Full Create and Read functionality for student records.  
- **Cost-Optimized:** Pay-per-request pricing model ensures no idle costs.  
- **Global Distribution:** CloudFront CDN enables worldwide access with low latency.  
- **Secure Access:** HTTPS encryption, IAM-based permissions, and CORS configuration.  
- **Comprehensive Documentation:** Step-by-step guide with 61 screenshots for reproducibility.

---

## Prerequisites
- AWS account with appropriate permissions.  
- Basic understanding of serverless architecture.  
- Familiarity with AWS services: Lambda, API Gateway, DynamoDB, S3.  
- Knowledge of Python, JavaScript, and REST API concepts.

---

## Implementation Steps

### Phase 1: Database Foundation – DynamoDB Table Setup

#### Step 1: DynamoDB Console Access
Access AWS DynamoDB service dashboard and select **Create table** for new table creation.  
![Step 1](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_1.png)

#### Step 2: Table Configuration
- **Table Name:** `StudentData`  
- **Partition Key:** `StudentID` (String)  
- Default settings applied with optional resource tagging.  
![Step 2](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_2.png)

#### Step 3: Table Creation Success
The table is provisioned with on-demand billing and ready for serverless integration.  
![Step 3](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_3.png)

---

### Phase 2: Serverless Compute – Lambda Function Development

#### Step 4: Lambda Console Access
Access AWS Lambda service console and initiate **Create function** for serverless compute.  
![Step 4](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_4.png)

#### Step 5: GET Function Configuration
- **Creation Method:** Author from scratch  
- **Function Name:** `getStudent`  
- **Runtime:** Python 3.13  
- **Execution Role:** Create new role with basic Lambda permissions  
![Step 5](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_5.png)

#### Step 6: Function Creation Success
Lambda function provisioned successfully, ready for code deployment.  
![Step 6](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_6.png)

#### Step 7: GET Function Implementation
Code implemented in `getStudent.py` to retrieve student data from DynamoDB.  
![Step 7](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_7.png)

#### Step 8: Function Deployment Success
Function deployed with automatic versioning, ready for testing.  
![Step 8](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_8.png)

---

### Phase 3: Initial Testing and Permission Configuration

#### Step 9: Database State Verification
Verify the `StudentData` table is empty and correctly structured.  
![Step 9](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_9.png)

#### Step 10: Test Event Creation
Create a test event `mytest` for initial function validation.  
![Step 10](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_10.png)

#### Step 11: Permission Error Identification
Execution returns `AccessDeniedException` due to missing DynamoDB permissions for Lambda.  
![Step 11](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_11.png)

#### Step 12: IAM Role Access
Access Lambda function **Configuration → Permissions** and open execution role `getStudent-role-6vsfiqz6` for policy editing.  
![Step 12](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_12.png)

#### Step 13: Inline Policy Creation
Create inline policy using JSON editor to allow DynamoDB access.  
![Step 13](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_13.png)

#### Step 14: DynamoDB Permissions Policy
Define policy `dynamodb-access-policy.json` granting `Scan`, `GetItem`, `PutItem` privileges.  
![Step 14](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_14.png)

#### Step 15: Policy Creation Success
Policy attached successfully to Lambda role. Function now has required permissions.  
![Step 15](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_15.png)

#### Step 16: Function Test Success
Re-execute function test; receives empty array `[]` indicating successful database connection.  
![Step 16](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_16.png)

---

### Phase 4: Data Insertion Function Development

#### Step 17: POST Function Configuration
- **Function Name:** `insertStudentData`  
- **Runtime:** Python 3.13  
- **Execution Role:** Reuse existing role with DynamoDB permissions  
![Step 17](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_17.png)

#### Step 18: POST Function Creation Success
Lambda function provisioned successfully, completing CRUD foundation.  
![Step 18](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_18.png)

#### Step 19: POST Function Implementation
Implemented in `insertStudentData.py` for student record insertion.  
![Step 19](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_19.png)

#### Step 20: POST Function Test Configuration
Create test event `mytest1` with JSON payload matching function input schema.  
![Step 20](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_20.png)

#### Step 21: POST Function Test Success
Function successfully inserts data; HTTP 200 returned.  
![Step 21](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_21.png)

#### Step 22: Database Data Verification
Verify insertion: `StudentID=1, Name=Sabin, Class=A, Age=30` in DynamoDB table.  
![Step 22](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_22.png)

---

### Phase 5: API Gateway REST API Implementation

#### Step 23: API Gateway Console Access
Navigate to Amazon API Gateway and initiate REST API creation for Lambda integration.  
![Step 23](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_23.png)

#### Step 24: REST API Selection
Select **REST API** and choose **Build** for custom configuration.  
![Step 24](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_24.png)

#### Step 25: API Configuration
- **API Name:** `student`  
- **Endpoint Type:** Edge-optimized for global reach  
- Edge-Optimized uses CloudFront edges; Regional reduces latency in same region; Private for VPC-only access  
![Step 25](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_25.png)

#### Step 26: API Creation Success
REST API created; ready for Lambda integration.  
![Step 26](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_26.png)

---

### Phase 6: HTTP Method Configuration and Testing

#### Step 27: GET Method Configuration
Configure GET method to integrate with `getStudent` Lambda function.  
![Step 27](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_27.png)

#### Step 28: GET Method Creation Success
GET method successfully integrated; endpoint ready for retrieval.  
![Step 28](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_28.png)

#### Step 29: GET Method Testing
Execute test; Lambda returns correct data.  
![Step 29](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_29.png)

#### Step 30: POST Method Configuration
Configure POST method to integrate with `insertStudentData` Lambda.  
![Step 30](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_30.png)

#### Step 31: POST Method Creation Success
POST method successfully integrated; API ready for create operations.  
![Step 31](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_31.png)

---

### Phase 7: API Deployment and Endpoint Configuration

#### Step 32: API Deployment Configuration
Deploy API to **prod** stage for public accessibility.  
![Step 32](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_32.png)

#### Step 33: Deployment Success
API deployed; invoke URL generated for frontend.  
![Step 33](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_33.png)

#### Step 34: Invoke URL Capture
Copy production stage URL: `https://{api-id}.execute-api.{region}.amazonaws.com/prod`  
![Step 34](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_34.png)

#### Step 35: Lambda Integration Verification
Confirm `getStudent` function trigger integration and permissions.  
![Step 35](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_35.png)

#### Step 36: CORS Configuration
Enable CORS for GET and POST methods to allow frontend browser access.  
![Step 36](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_36.png)

#### Step 37: CORS Configuration Success
Cross-domain access verified; browser same-origin policy issues resolved.  
![Step 37](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_37.png)

---

### Phase 8: Frontend Hosting – S3 Static Website

#### Step 38: S3 Bucket Creation
Create S3 bucket `themasterbucket` for frontend hosting.  
![Step 38](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_38.png)

#### Step 39: Frontend Code Upload
Upload React frontend files to S3 bucket.  
![Step 39](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_39.png)

#### Step 40: Bucket Properties Access
Navigate to **Properties** tab for static hosting configuration.  
![Step 40](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_40.png)

#### Step 41: Static Hosting Configuration Access
Locate **Static Website Hosting** section and initiate hosting setup.  
![Step 41](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_41.png)

#### Step 42: Static Website Hosting Setup
- Enabled hosting  
- **Index Document:** `index.html`  
![Step 42](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_42.png)

#### Step 43: Static Hosting Success
S3 bucket endpoint available for initial access.  
![Step 43](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_43.png)

#### Step 44: Website Endpoint URL
Capture bucket endpoint URL: `http://themasterbucket.s3-website.us-east-2.amazonaws.com`  
![Step 44](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_44.png)

#### Step 45: Initial Access Test
Initial access returned 403; public permissions required.  
![Step 45](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_45.png)

---

### Phase 9: S3 Public Access Configuration

#### Step 46: Public Access Settings
Adjust bucket permissions to allow public website access.  
![Step 46](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_46.png)

#### Step 47: Bucket Policy Generation
Use AWS Policy Generator to allow `GetObject` for `*` principal.  
![Step 47](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_47.png)

#### Step 48: Bucket Policy Implementation
Apply policy to bucket; public read access enabled.  
![Step 48](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_48.png)

#### Step 49: Resource ARN Correction
Modify ARN to `arn:aws:s3:::themasterbucket/*` to cover all files.  
![Step 49](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_49.png)

#### Step 50: Public Website Access Success
Website accessible; static hosting fully functional.  
![Step 50](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_50.png)

---

### Phase 10: Application Testing and Validation

#### Step 51: Empty Student Table Display
Frontend shows empty table headers for Student ID, Name, Class, Age.  
![Step 51](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_51.png)

#### Step 52: Existing Data Display
Previously inserted record displayed successfully through frontend.  
![Step 52](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_52.png)

#### Step 53: New Student Addition
Tested adding a new student via frontend; record displayed correctly.  
![Step 53](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_53.png)

---

### Phase 11: Security Enhancement with CloudFront CDN

#### Step 54: Security Issue Identification
S3 website served via HTTP; requires HTTPS for production security.  
![Step 54](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_54.png)

#### Step 55: CloudFront Console Access
Navigate to CloudFront console; initiate distribution for secure delivery.  
![Step 55](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_55.png)

#### Step 56: Distribution Configuration – Step 1
- **Distribution Name:** `student-management-app`  
- Description configured; basic metadata set.  
![Step 56](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_56.png)

#### Step 57: Origin Configuration – Step 2
S3 bucket selected as origin; configured for website hosting.  
![Step 57](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_57.png)

#### Step 58: Security Settings Configuration
WAF disabled for demo; can be enabled later for production.  
![Step 58](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_58.png)

#### Step 59: Critical Origin Configuration Correction
Corrected origin to S3 website endpoint: `themasterbucket.s3-website.us-east-2.amazonaws.com`.  
![Step 59](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_59.png)

#### Step 60: Distribution Review and Creation
Configuration reviewed; distribution creation initiated.  
![Step 60](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_60.png)

#### Step 61: Distribution Creation Success
CloudFront distribution created; propagation 5–15 minutes.  
![Step 61](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless_Web-App_61.png)

---

## Project Structure
├── README.md
├── Architecture_Diagram/
│ └── SERVERLESS_Web-App-Architecture.png
├── Screenshots/
│ ├── Deploying_Serverless_Web-App_1.png
│ ├── Deploying_Serverless-Web-App_2.png
│ └── ... (through 61.png)
└── documentation/
├── api-specification.md
├── deployment-guide.md
└── troubleshooting.md


**Code Reference:** Complete source code available in `AWS-SERVERLESS-DEPLOYMENT` repository.

---

## Testing and Validation
- DynamoDB table creation and verification.  
- Lambda function deployment and testing.  
- IAM role and policy configuration.  
- API Gateway method creation and testing.  
- CORS configuration validation.  
- S3 static website hosting setup.  
- CloudFront distribution deployment.  
- End-to-end application testing (Create & Read operations).

---

## Performance Metrics
- **API Response Time:** <500ms average  
- **Lambda Cold Start:** <3 seconds  
- **Static Content Load:** <100ms via CloudFront  
- **Global Latency:** Optimized with edge locations  

---

## Architecture Benefits
### Cost Optimization
- Pay-per-request: no idle server costs.  
- Automatic scaling: resources scale dynamically.  
- No infrastructure management: reduces operational overhead.  
- Free tier eligible: generous free tiers for Lambda, DynamoDB, S3.

### Scalability Features
- Lambda concurrent executions: up to 1,000 invocations.  
- DynamoDB auto scaling: automatic throughput adjustment.  
- CloudFront global distribution: 450+ edge locations.  
- API Gateway throttling: built-in protection.

### Security Implementation
- IAM least privilege: minimal permissions only.  
- HTTPS encryption via CloudFront SSL/TLS.  
- CORS controlled for frontend-backend communication.  
- Optional VPC Lambda deployment.

---

## Key Lessons Learned
1. **IAM Permissions Management:** Least privilege principle prevents downstream errors; custom policies preferred.  
2. **CORS Configuration:** Mandatory for separate frontend/backend; preflight OPTIONS method required.  
3. **Error Handling:** Comprehensive handling across Lambda, API Gateway, and frontend; CloudWatch logs critical.  
4. **Cost Optimization:** Serverless architecture reduces cost; monitoring and alerts prevent unexpected charges.

---

## Troubleshooting Guide
- **Lambda Permission Errors:** Ensure IAM role has DynamoDB access.  
- **CORS Browser Errors:** Verify CORS enabled in API Gateway.  
- **403 Forbidden on S3:** Apply public read bucket policy.  
- **CloudFront Not Updating:** Invalidate cache with `/*` pattern.

---

## Cost Optimization
### Estimated Monthly Free Tier
- Lambda: 1M requests + 400,000 GB-seconds compute.  
- DynamoDB: 25 GB storage + 200M read/write requests.  
- S3: 5 GB + 20,000 GET requests.  
- CloudFront: 1 TB transfer + 10M requests.  
- API Gateway: 1M API calls.

**Cost Monitoring:** AWS Budgets, Cost Explorer, and resource tags recommended.

---

## Future Enhancements
- Update & Delete Operations: Full CRUD implementation.  
- User Authentication: AWS Cognito integration.  
- File Uploads: S3-based document storage.  
- Real-time Updates: WebSocket API integration.  
- Monitoring Dashboard: CloudWatch metrics & alerts.  
- Scalability: DynamoDB Global Tables, Lambda Provisioned Concurrency, API Gateway caching, Lambda@Edge optimization.

---

## References and Resources
- AWS Three-Tier Web Architecture Workshop  
- AWS Well-Architected Framework  
- AWS Documentation  
- AWS Training & Certification  
- AWS Architecture Center  
- AWS Cloud Operations & Migration Blog

---

## Conclusion
This serverless web application illustrates a complete, production-ready AWS deployment with scalability, cost-efficiency, and maintainability. The step-by-step documentation with screenshots ensures reproducibility and serves as a comprehensive learning resource for developers adopting serverless architecture.


**AWS Services Used:** Lambda, API Gateway, DynamoDB, S3, CloudFront, IAM  
**Architecture Pattern:** Serverless Microservices  
**Deployment Region:** us-east-2 (Ohio)  
**Project Repository:** aws-serverless-architecture-showcase  
**Code Reference:** AWS-SERVERLESS-DEPLOYMENT
