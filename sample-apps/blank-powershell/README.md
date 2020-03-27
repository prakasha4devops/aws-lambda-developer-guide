# Blank function (PowerShell)

![Architecture](/sample-apps/blank-powershell/images/sample-blank-powershell.png)

The project source includes function code and supporting resources:

- `function` - A PowerShell function.
- `template.yml` - An AWS CloudFormation template that creates an application.
- `1-create-bucket.sh`, `2-deploy.sh`, etc. - Shell scripts that use the AWS CLI to deploy and manage the application.

Use the following instructions to deploy the sample application.

# Requirements
- [PowerShell Core 6.0 or newer](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell#powershell-core)
- [.NET Core 2.1](https://www.microsoft.com/net/download)
- [AWSLambdaPSCore module](https://www.powershellgallery.com/packages/AWSLambdaPSCore/1.2.0.0)
- The Bash shell. For Linux and macOS, this is included by default. In Windows 10, you can install the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to get a Windows-integrated version of Ubuntu and Bash.
- [The AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html).

# Setup
Download or clone this repository.

    $ git clone git@github.com:awsdocs/aws-lambda-developer-guide.git
    $ cd aws-lambda-developer-guide/sample-apps/blank-powershell

To create a new bucket for deployment artifacts, run `1-create-bucket.sh`. Or, if you already have a bucket, create a file named `bucket-name.txt` that contains the name of your bucket.

    blank-powershell$ ./1-create-bucket.sh
    make_bucket: lambda-artifacts-a5e491dbb5b22e0d

# Deploy
To deploy the application, run `2-deploy.sh`.

    blank-powershell$ ./2-deploy.sh
    Restoring .NET Lambda deployment tool
    Initiate packaging
    Uploading to e678bc216e6a0d510d661ca9ae2fd941  28800329 / 28800329.0  (100.00%)
    Successfully packaged artifacts and wrote output template to file out.yml.
    Waiting for changeset to be created..
    Waiting for stack create/update to complete
    Successfully created/updated stack - blank-powershell

This script uses AWS CloudFormation to deploy the Lambda functions and an IAM role. If the AWS CloudFormation stack that contains the resources already exists, the script updates it with any changes to the template or function code.

# Test
To invoke the function, run `3-invoke.sh`.

    blank-powershell$ ./3-invoke.sh
    {
        "StatusCode": 200,
        "ExecutedVersion": "$LATEST"
    }

The application uses AWS X-Ray to trace requests. Open the [X-Ray console](https://console.aws.amazon.com/xray/home#/service-map) to view the service map. The following service map shows the function calling Amazon S3.

![Service Map](/sample-apps/blank-powershell/images/blank-powershell-servicemap.png)

Choose a node in the main function graph. Then choose **View traces** to see a list of traces. Choose any trace to view a timeline that breaks down the work done by the function.

![Trace](/sample-apps/blank-powershell/images/blank-powershell-trace.png)

Finally, view the application in the Lambda console.

*To view the application*
1. Open the [applications page](https://console.aws.amazon.com/lambda/home#/applications) in the Lambda console.
2. Choose **blank-powershell**.

# Cleanup
To delete the application, run `4-cleanup.sh`.

    blank-powershell$ ./4-cleanup.sh