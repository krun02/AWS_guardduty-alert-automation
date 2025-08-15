# AWS_guardduty-alert-automation
This project implements a serverless security response workflow on AWS that automatically quarantines EC2 instances flagged in high-severity Amazon GuardDuty findings. It’s designed to showcase AWS security automation, detection engineering, and incident response skills.

**Flow**
1) GuardDuty emits a finding  
2) EventBridge rule (`source=aws.guardduty`) invokes Lambda  
3) Lambda identifies affected EC2 (Describe* APIs)  
4) Lambda replaces ENI security groups with `gd-isolation-sg` (no inbound/outbound)  
5) SNS sends notification; Lambda logs to CloudWatch


**Technologies Used**
| Technology                  | Purpose                                                                                  |
|-----------------------------|------------------------------------------------------------------------------------------|
| Amazon GuardDuty            | Continuous threat detection and monitoring for AWS accounts, workloads, and data.       |
| Amazon EventBridge          | Event routing to trigger automation when GuardDuty emits a finding.                      |
| AWS Lambda (Python)         | Serverless compute to run quarantine and notification logic.                             |
| Amazon EC2                  | Target workload that may be quarantined based on GuardDuty findings.                     |
| Amazon VPC Security Groups  | Network isolation for quarantined instances.                                             |
| Amazon SNS                   | Notification service for sending alerts via email or webhook.                           |
| Amazon CloudWatch Logs      | Centralized logging for Lambda execution and troubleshooting.                            |
| AWS IAM                     | Secures Lambda execution with least privilege permissions.                               |
| AWS Budgets (optional)      | Cost control and alerts to monitor free tier usage.                                      |


| Feature                   | Security Benefit                                                                 |
|---------------------------|-----------------------------------------------------------------------------------|
| Automated Threat Response | Instantly isolates compromised EC2 instances, reducing attacker dwell time.      |
| Network Quarantine        | Blocks all inbound/outbound traffic to stop lateral movement and exfiltration.   |
| Severity-Based Action     | Acts only on medium/high severity findings to avoid false positives.             |
| Least Privilege IAM Role  | Grants Lambda only necessary permissions for remediation and alerts.             |
| Comprehensive Logging     | CloudWatch logs provide full audit trails for investigations.                    |
| Immediate Alerts          | SNS notifies security teams instantly for rapid follow-up.                       |
| Safe Testing              | GuardDuty sample findings allow safe, controlled testing.                        |


**Architecture**

**Screenshots**
| Item            | Description |
|-----------------|-------------|
| IAM Roles List  | Displays all IAM roles in the account, including the `gd-quarantine-lambda-role` created for the GuardDuty automation Lambda. |

<img width="1510" height="663" alt="Screenshot 2025-08-15 at 3 16 43 PM" src="https://github.com/user-attachments/assets/f7bcd0d0-5fb6-424f-833e-81b0c4ddf0ef" />






| Item           | Description |
|----------------|-------------|
| IAM Dashboard  | Overview of IAM resources, showing total roles, policies, and security configuration in the account. |

<img width="1509" height="586" alt="Screenshot 2025-08-15 at 3 16 18 PM" src="https://github.com/user-attachments/assets/588ae922-4bd0-4cd1-8c67-c8a1f677ea61" />






| Item          | Description |
|---------------|-------------|
| VPC Dashboard | Shows details of the default VPC (`vpc-0ba352dfcacd6a19f`) where the GuardDuty automation resources are running. |

<img width="1511" height="824" alt="Screenshot 2025-08-15 at 3 11 28 PM" src="https://github.com/user-attachments/assets/16e93fcf-8b24-4697-bd57-583134cd53a8" />






| Item                  | Description |
|-----------------------|-------------|
| Lambda Functions List | Lists the Lambda function `gd-quarantine-lambda` used to isolate EC2 instances on GuardDuty findings. |

<img width="1510" height="826" alt="Screenshot 2025-08-15 at 3 09 05 PM" src="https://github.com/user-attachments/assets/7de05db0-a351-4b3b-b085-4b20f6cd72c3" />






| Item               | Description |
|--------------------|-------------|
| GuardDuty Summary  | Overview of GuardDuty status and findings, showing total findings and severity distribution. |

<img width="1504" height="837" alt="Screenshot 2025-08-15 at 3 04 22 PM" src="https://github.com/user-attachments/assets/2146f0c0-df48-4b8e-8832-a9d1d2e8a893" />







| Item                   | Description |
|------------------------|-------------|
| GuardDuty Findings List| Detailed list of GuardDuty findings, including a low-severity finding about root credential usage. |

<img width="1507" height="854" alt="Screenshot 2025-08-15 at 3 04 48 PM" src="https://github.com/user-attachments/assets/8d00356a-9a19-4ebe-b373-ea3e60c69398" />
























