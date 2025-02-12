# Bin Packing Solution

The AWS Bin Packing Solution enables efficient packing of items into containers within a 3D space to optimize resource utilization.





## Key Terminology

*Container Types*

A container holds items and has specific dimensions. It may also include special features like refrigeration.

*Item Types*

An item type represents the objects that are packed into containers. Each item type has its own dimensions and may require specific container features (e.g., refrigeration).

*Shipment*

A shipment consists of items that need to be packed into containers. Users can specify item types and their respective quantities to be allocated across available containers.

*Manifest*

A manifest is generated for each shipment, detailing how items are packed within containers.

## Solution Architecture



1. A single-page application hosted on Amazon S3, distributed via Amazon CloudFront.
2. Amazon Cognito manages authentication and authorization for API access.
3. Amazon API Gateway serves as a REST API to handle CRUD operations on data models.
4. AWS Lambda proxy function executes CRUD operations and triggers the packing solver.
5. Solver AWS Lambda function runs the bin-packing algorithm to determine an optimized packing solution, storing the results in Amazon DynamoDB. After optimization, the function retrieves a WebSocket connection 6. ID from DynamoDB and sends an update to the corresponding client.
7. Bi-directional WebSocket API facilitates real-time updates for connected clients.
8. Another AWS Lambda function links WebSocket connection IDs with client metadata for session tracking.
9. Amazon DynamoDB serves as the storage layer for data objects and WebSocket metadata.


## Project Structure

This solution is developed using C# and TypeScript. The key components include:

Packing Solver (C#) – application/csharp/AWS.Prototyping.Pacman.Solver/src/AWS.Prototyping.Pacman.Solver

Shared Types (TypeScript) – application/typescript/packages/@aws-prototype/shared-types

Defines data model objects shared between the API and front-end.
API (TypeScript) – application/typescript/packages/@aws-prototype/api

Implements the AWS Lambda proxy function REST API.
Subscription Handlers (TypeScript) – application/typescript/packages/@aws-prototype/subscription

Manages pub/sub communication between DynamoDB and WebSocket APIs.
Website (TypeScript & React.js) – application/typescript/packages/@aws-prototype/website

A React.js-based demo front-end for user interaction.
Infrastructure (TypeScript & AWS CDK) – application/typescript/packages/@aws-prototype/infra

Contains Infrastructure as Code (IaC) required for deployment using AWS CDK.


## Build

### Prerequisites

Ensure the following dependencies are installed and available in your system PATH:

node (version 14+)
.NET 6.0
yarn
npx
Docker
AWS CLI
AWS CDK
cfn-nag (install using gem install cfn-nag)

# Complete Build Instructions
1) Build the Packing Solver (C# component)
   '''cd application/csharp/AWS.Prototyping.Pacman.Solver/src/AWS.Prototyping.Pacman.Solver  
docker build '''.

2) Build TypeScript Components
  ''' cd application/typescript  
yarn run build'''
This command installs dependencies and builds all packages in the correct dependency order.


## Bootstrapping new accounts

Before deploying to a new AWS account, bootstrap it using the following command:


```cdk bootstrap --profile <target-account-profile> --trust <pipeline-account-id> --cloudformation-execution-policies arn:aws:iam::aws:policy/AdministratorAccess```

## Configuration

### Pipeline Notifications

To configure Slack notifications for the CI/CD pipeline, create a notifications.json file in the root directory of this repository and define the following structure:

```json
{
  "slackChannelConfigurationName":"my-cicd-notifications",
  "slackWorkspaceId":"TXXXXXXXXXX",
  "slackChannelId":"CXXXXXXXXXX"
}
```

## Deployment
Ensure you have met the AWS CDK prerequisites before proceeding.

To deploy the solution, navigate to the infrastructure directory and run:
'''cd application/typescript/packages/@aws-prototype/infra  
cdk deploy'''

This command will provision all necessary AWS resources and deploy the Bin Packing Solution to your AWS account.



