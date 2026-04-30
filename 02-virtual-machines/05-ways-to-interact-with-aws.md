# Ways to Interact with AWS

## AWS Management Console (UI)

- Web-based interface (GUI)
- Easy and interactive for beginners
- Good for quick and small tasks

Limitations:

- Not efficient for repetitive or large-scale work
- Requires manual actions

For deeper work, you can connect to EC2 using:

- EC2 Instance Connect (browser)
- SSH client (recommended for better efficiency)

---

## AWS CLI (Command Line Interface)

- Tool to manage AWS using commands
- Faster and more efficient than UI
- Useful for automation and scripting

Setup:

- Install AWS CLI:
  https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

- Configure credentials:

  ```bash
  aws configure
  ```

  `Example for creating ec2:
aws ec2 run-instances \
--image-id ami-xxxxxxxx \
--count 1 \
--instance-type t2.micro \
--key-name MyKeyPair \
--security-group-ids sg-903004f8 \
--subnet-id subnet-6e7f829e
`

---

## AWS CloudFormation

- Infrastructure as Code (IaC) service
- Define resources using templates (YAML/JSON)
- Automatically creates and manages infrastructure

Beginner idea:

Instead of creating resources manually, you write a template and AWS builds everything for you.

Sample templates:
https://github.com/aws-cloudformation/aws-cloudformation-templates

---

## AWS API

- Programmatic way to interact with AWS
- Commonly used through SDKs like Python Boto3
- Uses credentials from aws configure
- Call AWS services using functions

Example code:
`import boto3

ec2 = boto3.client('ec2')
ec2.describe_instances()
`

---

## Final Note

You can always go to AWS CLI or other ways to connect documentations for commands or other ways to connect.
