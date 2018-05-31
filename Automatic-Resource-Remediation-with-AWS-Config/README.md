# Automatic Resource Remediation with AWS Config

CloudCraft: <https://cloudcraft.cloudassessments.com/#/labs/details/ef4e8928-da70-4e8e-a9ad-974acdc392be>

## AWS Config Setup

1. Enable `restricted-ssh` rule in Config
1. Add to IAM role `config-role-us-east-1...` the following: `AWSConfigRulesExecutionRole` and `AWSLambdaExecute`
1. Create SNS topic to receive noncompliance notifications (i.e. `config-rules-compliance`)
1. Create policy `AllowSNSPublish`:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "Stmt1485832788000",
                "Effect": "Allow",
                "Action": [
                    "sns:Publish"
                ],
                "Resource": [
                    "arn:aws:sns:*"
                ]
            }
        ]
    }
    ```

1. Attach policy to `config-role-us-east-1`.
1. Create Lambda function to poll Config ~60s to detect noncompliant resources (see `poll_config.py`)
1. Create schedule in CloudWatch Events

Lambda function needs the following:

```json
{
    "Effect": "Allow",
        "Action": [
            "ec2:AuthorizeSecurityGroupIngress",
            "ec2:DescribeSecurityGroups",
            "ec2:RevokeSecurityGroupIngress"
        ],
    "Resource": "*"
}
```
