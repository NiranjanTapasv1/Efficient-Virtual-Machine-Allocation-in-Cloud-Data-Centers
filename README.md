# Efficient Virtual Machine Allocation in Cloud Data Centers

This solution focuses on optimizing the allocation of virtual machines (VMs) in cloud data centers, ensuring efficient resource utilization through advanced bin-packing strategies.





## Key Terminology

*Container Types*

A container represents a resource unit that holds virtual machines. It has predefined dimensions and may include specialized features such as high-performance computing or GPU acceleration.

*Virtual Machine (VM) Types*

VMs represent computational units that require allocation within containers. Each VM has specific resource requirements, such as CPU, memory, and storage, and may need specialized container features.

*Allocation Request*

An allocation request defines the number and types of VMs that need to be assigned to containers. Users can specify VM types and their respective quantities for optimal distribution across available containers.

*Allocation Plan*

An allocation plan is generated based on an allocation request, outlining how VMs are efficiently assigned to containers.

## Solution Architecture



1. A single-page application hosted on Amazon S3 and distributed through Amazon CloudFront for efficient content delivery.
2. Amazon Cognito handles authentication and authorization to ensure secure access to APIs.
3. Amazon API Gateway provides a REST API for managing VM allocation requests and processing CRUD operations.
4. An AWS Lambda proxy function performs API operations and initiates the allocation solver.
5. The solver AWS Lambda function runs a bin-packing algorithm to optimize VM allocation, storing results in Amazon DynamoDB. Once optimization is complete, it retrieves the WebSocket connection ID from DynamoDB and updates the corresponding client.
6. A bi-directional WebSocket API enables real-time updates for connected clients.
7. Another AWS Lambda function maps WebSocket connection IDs to client metadata for session tracking.
8. Amazon DynamoDB serves as a scalable storage solution for VM allocation data and WebSocket metadata.


## Project Structure

This solution is developed using C# and TypeScript, with key components structured as follows:

Allocation Solver (C#) – application/csharp/AWS.Prototyping.Pacman.Solver/src/AWS.Prototyping.Pacman.Solver
Implements the core VM allocation logic using an advanced bin-packing algorithm.

Shared Data Models (TypeScript) – application/typescript/packages/@aws-prototype/shared-types
Defines data models shared between the backend API and the front-end application.

API Layer (TypeScript) – application/typescript/packages/@aws-prototype/api
Manages AWS Lambda proxy functions and REST API endpoints.

Event Handlers (TypeScript) – application/typescript/packages/@aws-prototype/subscription
Handles real-time pub/sub communication between DynamoDB and WebSocket APIs.

User Interface (React.js & TypeScript) – application/typescript/packages/@aws-prototype/website
Provides an interactive front-end for managing VM allocation requests and viewing results.

Infrastructure as Code (AWS CDK & TypeScript) – application/typescript/packages/@aws-prototype/infra
Defines cloud infrastructure using AWS Cloud Development Kit (CDK).

## Build

### Prerequisites

Ensure the following dependencies are installed and available in your system's PATH:

Node.js (version 14+)
.NET 6.0
Yarn
Npx
Docker
AWS CLI
AWS CDK
cfn-nag (install via gem install cfn-nag)

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
CI/CD Pipeline Notifications
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



