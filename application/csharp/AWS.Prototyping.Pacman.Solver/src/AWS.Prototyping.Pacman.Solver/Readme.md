AWS Lambda Empty Function Project by Niranjan Tapasvi
This starter project includes:

Function.cs – A class file containing a function handler method.
aws-lambda-tools-defaults.json – Default configuration settings for Visual Studio and command-line deployment tools for AWS.
Depending on the options selected, a test project may also be included.

The generated function handler is a basic method that takes a string input and returns its uppercase version. Modify the method’s logic and parameters as needed to suit your requirements.

Steps for Visual Studio Deployment :
To deploy your function to AWS Lambda, right-click the project in Solution Explorer and select Publish to AWS Lambda.
To view the deployed function, open the Function View window by double-clicking the function name under the AWS Lambda node in the AWS Explorer tree.
To test your function after deployment, use the Test Invoke tab in the Function View window.
To configure event sources (e.g., triggering the function when an object is created in an S3 bucket), use the Event Sources tab.
To modify the runtime settings of the deployed function, navigate to the Configuration tab.
To review execution logs of function invocations, check the Logs tab.

Steps for Command-Line Deployment : 

Follow these steps to deploy your AWS Lambda function using the Amazon.Lambda.Tools Global Tool.

1. Install Amazon.Lambda.Tools (if not already installed)
Run the following command to install the AWS Lambda global tool:
dotnet tool install -g Amazon.Lambda.Tools  

2. Check for Available Updates
If you already have Amazon.Lambda.Tools installed, update it to the latest version:
dotnet tool update -g Amazon.Lambda.Tools  

3. Navigate to the Test Directory and Run Unit Tests
Before deployment, ensure your function works correctly by executing unit tests:
cd "AWS.Prototyping.Pacman.Solver/test/AWS.Prototyping.Pacman.Solver.Tests"  
dotnet test 

4. Navigate to the Source Directory
Move to the source directory where your Lambda function is located:
cd "AWS.Prototyping.Pacman.Solver/src/AWS.Prototyping.Pacman.Solver"  

5. Deploy Function to AWS Lambda
Deploy your function using the following command:
dotnet lambda deploy-function  

This will package and deploy your function to AWS Lambda based on your project settings. Once deployed, you can invoke, monitor, and manage the function using the AWS Lambda console or CLI.


 