# CloudFormation template for the "Working with CloudWatch Logs" learning activity

CloudCraft: <https://cloudcraft.cloudassessments.com/#/labs/details/d2cf6817-f0f3-4e92-b6f7-5211da4d9825>

To attach a role to the EC2 instance which allows the CloudWath agent to send logs, attach a managed policy using the `ManagedPolicyArns` attribute and the ARN for the `CloudWatchAgentServerPolicy`:

```json
"StudentEC2InstanceRole": {
            "Type": "AWS::IAM::Role",
            "Properties": {
                "Path": "/",
                "ManagedPolicyArns": ["arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy"],
                "AssumeRolePolicyDocument": {
                    "Statement": [
                        {
                            "Action": [
                                "sts:AssumeRole"
                            ],
                            "Effect": "Allow",
                            "Principal": {
                                "Service": [
                                    "ec2.amazonaws.com"
                                ]
                            }
                        }
                    ]
                }
            }
        },
```

The IAM actions `iam:List*` and `iam:Get*` allow `cloud_user` to see the IAM policy attached to the EC2 instance.

## Installing the Unified Agent

```bash
# Download Agent
wget https://s3.amazonaws.com/amazoncloudwatch-agent/linux/amd64/latest/AmazonCloudWatchAgent.zip

# Unzip Package
unzip AmazonCloudWatchAgent.zip

# Install Package
sudo ./install.sh

# Configure the agent
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard

# Start the agent
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:configuration-file-path -s

# View agent status
/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a status
```
