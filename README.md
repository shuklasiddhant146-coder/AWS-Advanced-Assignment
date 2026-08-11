# AWS Advanced Scalable Web Application

A .NET 8 web application deployed on AWS using Elastic Beanstalk, Amazon RDS, Amazon S3, SQS, SNS, Lambda, DynamoDB, EventBridge, Secrets Manager, IAM and CloudWatch.

## Overview

This project demonstrates deployment of a scalable web application on AWS with secure database connectivity, event-driven processing, scheduled automation, monitoring and alerting.

The solution uses AWS managed services to demonstrate scalability, security, reliability, monitoring and serverless automation.

---

## Architecture

```text
                         ┌──────────────────┐
                         │      USER        │
                         └────────┬─────────┘
                                  │
                               Internet
                                  │
                                  ▼
                    ┌─────────────────────────┐
                    │   Elastic Beanstalk     │
                    │    ASP.NET Core .NET 8  │
                    │       Auto Scaling      │
                    └────────────┬────────────┘
                                 │
                         MySQL : 3306
                         Restricted SG
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │       Amazon RDS        │
                    │          MySQL          │
                    │      Private Subnet     │
                    └─────────────────────────┘
                                 ▲
                                 │
                    ┌─────────────────────────┐
                    │   AWS Secrets Manager   │
                    │   RDS Connection Secret  │
                    └─────────────────────────┘


 ┌─────────────────┐
 │   Amazon S3     │
 │ Versioning      │
 │ Lifecycle       │
 └───────┬─────────┘
         │
         ├──────────────────────┐
         │                      │
         ▼                      ▼
 ┌───────────────┐       ┌───────────────┐
 │  Amazon SQS   │       │  Amazon SNS   │
 │ File Events   │       │ File Events   │
 └───────────────┘       └───────┬───────┘
                                 │
                                 ▼
                         ┌─────────────────┐
                         │     Lambda      │
                         │ SNS Subscriber  │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │ CloudWatch Logs │
                         └─────────────────┘


                         ┌─────────────────┐
                         │  EventBridge    │
                         │ Every 5 Minutes │
                         └────────┬────────┘
                                  │
                                  ▼
                         ┌─────────────────┐
                         │     Lambda      │
                         │ Scheduled Job   │
                         └────────┬────────┘
                                  │
                              PutItem
                                  │
                                  ▼
                         ┌─────────────────┐
                         │   DynamoDB      │
                         │assignment-meta  │
                         └─────────────────┘


              ┌──────────────────────────────────┐
              │         Amazon CloudWatch        │
              │                                  │
              │ • EB CPU Metrics                │
              │ • RDS CPU Metrics               │
              │ • RDS Memory Metrics             │
              │ • Lambda Logs/Metrics            │
              │ • S3 Metrics                     │
              │ • DynamoDB Metrics               │
              │ • Alarms                         │
              │ • Dashboard                      │
              └──────────────────────────────────┘


              ┌──────────────────────────────────┐
              │             IAM                  │
              │      Least Privilege Roles       │
              │                                  │
              │ EB Role                          │
              │ Lambda Execution Roles           │
              │ Restricted DynamoDB Permissions   │
              └──────────────────────────────────┘
