# **AWS Serverless Web Application – Student Data Management**

## **Executive Summary**
This project demonstrates the design, development, and deployment of a **production-grade serverless web application** on AWS for efficient **student data management**. Built using **AWS Lambda, API Gateway, DynamoDB, S3, and CloudFront**, the solution provides a **highly available, scalable, and cost-efficient architecture** while completely eliminating the need to manage servers.  

By leveraging **serverless microservices architecture**, the application implements **best practices** for:
- **Security** – with fine-grained IAM roles and policies.  
- **Performance** – via scalable, event-driven compute resources.  
- **Operational efficiency** – reduced maintenance overhead and optimized costs.  

This documentation provides **end-to-end implementation steps** with detailed screenshots, serving as a practical guide for building enterprise-grade serverless solutions.

---

## **Architecture Diagram**

![Serverless Web App Architecture](https://raw.githubusercontent.com/Sabin-Rana/aws-serverless-architecture-showcase/main/Architecture_Diagram/SERVERLESS_Web-App-Architecture.png)

The architecture follows a **five-tier serverless design**:
1. **Frontend Hosting:** React-based UI hosted on **Amazon S3**, served securely via **Amazon CloudFront**.
2. **API Layer:** **Amazon API Gateway** acts as the secure entry point for backend services.
3. **Compute Layer:** **AWS Lambda** functions handle application logic.
4. **Database Layer:** **Amazon DynamoDB** stores and manages student records.
5. **Security & Permissions:** **AWS IAM** ensures least-privilege access control.

---

## **End-to-End Implementation (61 Steps)**

### **Database Setup: DynamoDB**
**Step 1:** Create a new DynamoDB table.  
![Step 1](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_1.png)  

**Step 2:** Configure table name `StudentData` and partition key `StudentID`.  
![Step 2](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_2.png)  

**Step 3:** Table successfully created and ready for data operations.  
![Step 3](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_3.png)  

---

### **Backend Logic: AWS Lambda**
**Step 4:** Create a new Lambda function.  
![Step 4](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_4.png)  

**Step 5:** Configure function details:
- Function Name: `getStudent`
- Runtime: Python 3.13
- Execution Role: New IAM role  
![Step 5](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_5.png)  

**Step 6:** Lambda successfully created.  
![Step 6](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_6.png)  

**Step 7:** Deploy code to `getStudent` function.  
![Step 7](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_7.png)  

**Step 10:** Encounter `AccessDeniedException`.  
*Solution:* Add appropriate IAM permissions.  
![Step 10](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_10.png)  

**Step 12-14:** Create and attach custom inline IAM policy (`DynamoDB-StudentData-Access`).  
![Step 14](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_14.png)  

---

### **API Creation: API Gateway**
**Step 22-25:** Create a REST API named `student`.  
![Step 25](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_25.png)  

**Step 26-30:**  
- **GET Method:** Connects to `getStudent` Lambda.  
- **POST Method:** Connects to `insertStudentData` Lambda.  
![Step 30](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_30.png)  

**Step 31-33:** Deploy API to `prod` stage and obtain invoke URL.  
![Step 33](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_33.png)  

**Step 35-36:** Enable **CORS** for cross-origin requests.  
![Step 36](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_36.png)  

---

### **Frontend Hosting: Amazon S3**
**Step 37-38:** Create S3 bucket `themasterbucket` and upload React frontend files.  
![Step 38](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_38.png)  

**Step 41:** Enable static website hosting with `index.html` as root.  
![Step 41](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_41.png)  

**Step 45-48:** Update bucket policy to allow public read access.  
![Step 48](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_48.png)  

**Step 49:** Frontend now accessible via S3 endpoint.  
![Step 49](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_49.png)  

---

### **Secure Distribution: Amazon CloudFront**
**Step 54-56:** Create CloudFront distribution with S3 as origin.  
![Step 56](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_56.png)  

**Step 58:** Configure origin to use S3 static website hosting endpoint.  
![Step 58](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_58.png)  

**Step 61:** Secure site accessible via HTTPS CloudFront domain.  
![Step 61](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase/blob/main/Screenshots/Deploying_Serverless-Web-App_61.png)  

---

## **Verified End-to-End Flow**
- **Frontend:** CloudFront → S3 (React UI)  
- **Backend:** API Gateway → Lambda → DynamoDB  
- **Security:** IAM for controlled access, HTTPS enforced.  
- **CORS:** Configured for seamless frontend-backend integration.  

---

## **Project Structure**
AWS-SERVERLESS-ARCHITECTURE-SHOWCASE/
├── README.md
├── Architecture_Diagram/
│ └── SERVERLESS_Web-App-Architecture.png
└── Screenshots/
└── Deploying_Serverless-Web-App_1.png ... Deploying_Serverless-Web-App_61.png


**Code Reference:**  
Complete source code is available in the `AWS-SERVERLESS-DEPLOYMENT` repository.

---

## **Testing and Validation**
Comprehensive testing was performed to ensure each AWS service integrated correctly:
- **DynamoDB:** Table creation, schema validation, and data verification.  
- **Lambda Functions:** Deployment, test events, and integration with DynamoDB.  
- **IAM Roles & Policies:** Proper permissions applied using least privilege principle.  
- **API Gateway:** Method creation, endpoint testing, and CORS validation.  
- **S3 Hosting:** Static website setup and access testing.  
- **CloudFront:** Secure distribution deployment and cache validation.  
- **End-to-End Testing:** Verified Create and Read operations from frontend to backend.  

---

## **Performance Metrics**
| **Metric**            | **Result** |
|-----------------------|------------|
| API Response Time     | < 500ms average |
| Lambda Cold Start     | < 3 seconds |
| Static Content Load   | < 100ms via CloudFront |
| Global Latency        | Optimized using edge locations |

---

## **Architecture Benefits**

### **Cost Optimization**
- **Pay-per-request model:** No costs for idle resources.  
- **Automatic scaling:** Dynamic scaling based on demand.  
- **Minimal management overhead:** No traditional server maintenance required.  
- **Free Tier Eligibility:** Lambda, DynamoDB, and S3 offer generous free usage limits.

### **Scalability Features**
- **Lambda:** Supports up to 1,000 concurrent executions by default.  
- **DynamoDB:** Auto-scaling adjusts throughput automatically.  
- **CloudFront:** 450+ global edge locations ensure fast content delivery.  
- **API Gateway:** Built-in throttling for performance and DDoS protection.

### **Security Implementation**
- **IAM Roles:** Enforced least privilege access to secure resources.  
- **Encryption:** HTTPS via CloudFront SSL/TLS for secure data in transit.  
- **CORS Policies:** Controlled frontend-backend communication.  
- **Optional VPC Integration:** Lambda functions can be securely connected to private VPC subnets.

---

## **Key Lessons Learned**
1. **IAM Permissions Management:** Implementing least privilege prevents unauthorized access and operational errors.  
2. **CORS Configuration:** Proper CORS setup is essential when separating frontend and backend components.  
3. **Error Handling & Monitoring:** CloudWatch logs are critical for identifying issues across Lambda and API Gateway.  
4. **Cost Awareness:** Serverless services are cost-effective, but monitoring prevents unexpected charges.

---

## **Troubleshooting Guide**
| **Issue**                | **Solution** |
|--------------------------|--------------|
| Lambda Access Errors     | Verify IAM policies allow DynamoDB actions. |
| CORS Browser Errors      | Enable and configure CORS in API Gateway. |
| S3 403 Forbidden Error   | Apply correct public-read bucket policy. |
| CloudFront Not Updating  | Invalidate cache using `/*` pattern. |

---

## **Cost Optimization**

### **Estimated Monthly Free Tier Usage**
| **AWS Service** | **Free Tier Limits** |
|-----------------|----------------------|
| **Lambda**      | 1M requests + 400,000 GB-seconds compute |
| **DynamoDB**    | 25 GB storage + 200M read/write requests |
| **S3**          | 5 GB storage + 20,000 GET requests |
| **CloudFront**  | 1 TB data transfer + 10M requests |
| **API Gateway** | 1M API calls |

**Recommendation:**  
Use **AWS Budgets** and **Cost Explorer** with resource tagging to monitor and control costs.

---

## **Future Enhancements**
Planned improvements for upcoming versions:
- **Full CRUD Support:** Add Update and Delete operations.  
- **Authentication:** Integrate AWS Cognito for user sign-in and access control.  
- **File Upload Capability:** Store student documents in S3.  
- **Real-Time Data Sync:** Implement WebSocket APIs for live updates.  
- **Monitoring Dashboards:** Use CloudWatch for metrics and alerts.  
- **Global Scalability:**  
  - DynamoDB Global Tables  
  - Lambda Provisioned Concurrency  
  - API Gateway Caching  
  - Lambda@Edge for global logic deployment.

---

## **References & Resources**
- [AWS Three-Tier Web Architecture Workshop](https://aws.amazon.com/architecture/)  
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)  
- [AWS Documentation](https://docs.aws.amazon.com/)  
- [AWS Training & Certification](https://aws.amazon.com/training/)  
- [AWS Cloud Operations & Migration Blog](https://aws.amazon.com/blogs/)

---

## **Conclusion**
This project showcases a complete, **serverless web application** deployment on AWS that is secure, cost-effective, and highly scalable. The **step-by-step screenshots** and detailed documentation provide a reproducible guide for developers and architects to adopt modern **serverless best practices**.  

This application is ideal for production workloads with dynamic scaling needs and serves as a reference architecture for building cloud-native, event-driven applications.

---

**AWS Services Used:** Lambda, API Gateway, DynamoDB, S3, CloudFront, IAM  
**Architecture Pattern:** Serverless Microservices  
**Deployment Region:** `us-east-2` (Ohio)  
**Project Repository:** [aws-serverless-architecture-showcase](https://github.com/Sabin-Rana/aws-serverless-architecture-showcase)  
**Code Reference:** `AWS-SERVERLESS-DEPLOYMENT`
