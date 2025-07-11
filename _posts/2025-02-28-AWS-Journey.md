---
title: "CI&CD in AWS"
date: 2025-02-28 00:00:00 +0800
categories: [DevOps]
image: assets/images/images.png
tags: [Pipelines]
---  

# AWS DevOps & Automation in Action: My Journey to Lightning-Fast Deployments

## The Beginning of the Journey

I was working on a rapidly evolving SaaS project a few years ago when I found myself at a turning point. Every release seemed like a gamble due to our engineering workflow's inconsistencies, manual deployments, and issues that made their way into production.

Even the once-weekly deployment felt dangerous. I was aware that we required a shift—something that would enable us to grow and deliver with assurance.

## A Vision for Change

In my ideal world, developers wouldn't have to worry about deployments and could concentrate on building code. Building a completely automated, scalable DevOps pipeline on AWS was my lofty ambition.

Three fundamental ideas served as the cornerstone of my approach:

- **GitOps**: Git would be the single source of truth  
- **Infrastructure as Code**: Every component versioned, auditable, and reproducible  
- **Immutable Deployments**: Each deployment should be clean, predictable, and repeatable  

## Building the Dream Pipeline

I began assembling the architecture using AWS-native services that offered flexibility and tight integration:

| Component                | AWS Service / Tool               |
|--------------------------|----------------------------------|
| Source Control           | AWS CodeCommit                   |
| CI/CD                    | AWS CodePipeline + CodeBuild     |
| Infrastructure as Code   | AWS CloudFormation + CDK         |
| Container Orchestration  | Amazon ECS with Fargate          |
| Monitoring & Logging     | Amazon CloudWatch + AWS X-Ray    |
| Secrets Management       | AWS Secrets Manager              |
| Notifications            | Amazon SNS + Slack Integration   |

### Pipeline Flow

1. Code was pushed to CodeCommit by a developer, usually myself.  
2. CodeBuild used SonarQube for static analysis, Trivy for vulnerability scanning, and unit tests.  
3. After being packaged as Docker containers, the build artefacts were uploaded to Amazon ECR.  
4. ECS services on Fargate were updated or deployed using AWS CDK.  
5. Zero downtime was guaranteed by blue/green deployments.  
6. X-Ray and CloudWatch provided complete visibility into app problems and performance.  

## Transformation in Numbers

After implementing this pipeline, I started tracking key performance metrics. The improvement was massive:

| Metric                     | Before            | After                 | Improvement         |
|----------------------------|---------------    |---------------------- |---------------------|
| Deployment Frequency       | Weekly            | Multiple times/day    | +700%               |
| Deployment Time            | ~45 mins          | < 7 mins              | -84%                |
| Manual Intervention        | Frequent          | Near Zero             | -95%                |
| Environment Drift Issues   | Common            | None                  | Eliminated          |
| Developer Productivity     | Moderate          | High                  | +60%                |
| Cost Optimization          | Underutilized EC2 | Fargate (pay-per-use) | -35% infrastructure cost |

## Unlocking ROI

The move to Fargate helped eliminate idle server costs. Build servers auto-scaled with demand. Monitoring tools provided real-time insights to fine-tune resource allocation.

**ROI within 3 months:**

- 3× faster feature delivery  
- 35% reduction in infrastructure costs  
- 2× faster developer onboarding  

## Lessons from the Journey

Reflecting on this experience, here are a few key lessons I learned:

- Start small. Don’t automate everything on day one.  
- Prioritize observability from the beginning.  
- Treat infrastructure like application code—version it, test it, and document it.  
- Build security into every stage of the pipeline.  

## The Road Ahead

My approach to software delivery has changed as a result of this experience. Our team gained a significant competitive edge from what began as a side project. Our release cycle sped up, developers increased their productivity, and downtime virtually vanished.

It was a true, dirty, and fulfilling process for me. Additionally, it set the stage for all of the DevOps transformations I have since spearheaded.
