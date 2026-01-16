# AWS Labs and Challenges Repository

## Overview

This repository contains a collection of labs, challenges, and practical exercises completed as part of my training for the AWS Certified Solutions Architect certification. It serves as a comprehensive record of hands-on experience with various AWS services and architectures.

## Café Business Case Introduction

Welcome to AWS Academy Cloud Architecting. The challenge labs in this course are built around a fictional business case. The business case provides a way to explore cloud computing topics in the context of relatable business needs. This scenario is intended to provide an example of the real-world applicability of technical concepts that you will learn.

### Café Business Scenario

Frank and Martha opened a café and bakery in their retirement. Their staff and a few AWS consultants who are also customers are helping them use the cloud to address the needs of their growing business.

Frank and Martha dreamed of opening a café and bakery in retirement. They were not ready to stop working entirely. They wanted to do something that incorporated their love of baking and supplemented their income.

To make their dreams a reality, Frank and Martha recently opened a café and bakery at the base of their building. Since opening the café and bakery, they enjoy interacting with the people in their neighborhood. They also support community events with their baked goods and coffees.

Frank and Martha have experienced an increase in local business. They also sometimes receive inquiries for products from business travelers and tourists who pass through the area. Together with their staff and café visitors (some of whom are AWS consultants), they discover how cloud computing can help their business grow.

In each of the challenge (café) labs, we take on the role of the café staff, and receive advice and assistance from AWS consultants who occasionally pass through the café. we will architect cloud solutions that help fulfill the business needs of the café.

### Café Business Owners and Staff

- **Frank**: Co-owner of the café, retired from the navy, likes to bake, nontechnical
- **Martha**: Co-owner of café, retired accountant, knows how to use spreadsheets but otherwise nontechnical
- **Sofía**: Daughter of Frank and Martha, supply chain manager for the café, future business administration student, has technical skills including programming
- **Nikhil**: Café employee with visual design skills, interested in learning cloud computing, might take on more responsibilities when Sofía starts university studies

### Café Visitors Who Are AWS Consultants

- **Olivia**: AWS solutions architect, specialty in databases and network technologies
- **Faythe**: Developer, experience with AWS programming interfaces, knowledgeable about cloud security
- **Mateo**: Systems administrator and engineer, likes to automate and create repeatable solutions, knows the importance of backups and disaster recovery

### The Evolving Café Architecture

| Version | Business Reason for Update | Technical Requirements and Architecture Update |
|---------|---------------------------|-----------------------------------------------|
| V1 | Build a static website for a small business. | Host the website on Amazon S3. |
| V2 | Update the website to support dynamic content and online ordering. | Deploy the web application and database on Amazon EC2. |
| V3 | Reduce the effort to maintain the database and secure its data. | Separate web and database layers. Migrate the database to Amazon RDS on a private subnet. |
| V4 | Enhance the security of the web application. | Use Amazon VPC features to configure and secure public and private subnets. |
| V5 | Ensure that the website can handle an expected increase in traffic and remain highly available and resilient to failure. | Add a load balancer, implement auto scaling on the EC2 instances, and distribute compute and database instances across two Availability Zones. |
| V6 | Automate deployments so that the café can consistently deploy, manage, and update café resources across Regions. | Build a version-controlled CloudFormation template to deploy the network and application layers. Deploy the CloudFormation stack to another Region. |
| V7 | Add reporting capabilities while reducing the operational and maintenance burden, improving performance, and reducing costs. | Deploy Lambda functions that connect to the Amazon RDS database and generate a report based on a schedule. |

Across the challenge labs in the course, you help Frank and Martha evolve their cloud architecture. You take it from a simple static website in the cloud to a secure, scalable, and resilient cloud-based application that helps them optimize costs.

## Repository Structure

The repository is organized by AWS service or lab topic. Each directory contains:

- **README.md**: Detailed instructions and objectives for the lab
- **Documentation**: Step-by-step completion reports with screenshots
- **Code/Config Files**: Any scripts, configurations, or code used
- **Images**: Screenshots and diagrams from the lab execution

### Current Labs

- **[Introducing Amazon Elastic File System (Amazon EFS)](Introducing%20Amazon%20Elastic%20File%20System%20(Amazon%20EFS)/)**
  - Lab on creating and configuring EFS file systems
  - Mounting to EC2 instances
  - Performance monitoring and benchmarking

## Certification Preparation

This work is part of my preparation for the **AWS Certified Solutions Architect - Associate** certification. The labs cover key AWS services including:

- Compute (EC2, Lambda)
- Storage (S3, EFS, EBS)
- Networking (VPC, Security Groups, Route 53)
- Databases (RDS, DynamoDB)
- Security and Identity (IAM, KMS)
- Monitoring and Logging (CloudWatch, CloudTrail)

## How to Use This Repository

1. **Browse by Topic**: Navigate to specific lab directories for detailed documentation
2. **Review Implementation**: Check the documentation files for step-by-step completion
3. **Reference Screenshots**: View images to understand AWS console interactions
4. **Code Examples**: Use any provided scripts or configurations as reference

## Learning Objectives

Through these labs, I aim to:

- Gain practical experience with AWS services
- Understand best practices for cloud architecture
- Develop skills in deploying and managing AWS resources
- Prepare for real-world AWS solution design scenarios

## Tools and Technologies

- AWS Management Console
- AWS CLI
- Linux command line
- Performance benchmarking tools (fio)
- Monitoring tools (CloudWatch)

## Progress Tracking

This repository will be updated regularly as I complete more labs and challenges. Each completed lab includes comprehensive documentation of the implementation process.

## Contact

For questions or discussions about these labs, feel free to reach out.

---

*Note: This repository is for educational purposes and contains work completed during AWS training programs.*
