# Live AWS Project Using Shell Scripting

Generally, AWS resource monitoring is viewed through a reporting dashboard.  
However, for practicing purposes, we will use shell scripting.

## Services Included

- S3
- EC2
- Lambda
- IAM

---

## The Script

```bash
#!/bin/bash

#############################
# Author: Joemar Catipon
# Date: May 7, 2026
#
# Version: v1
#
# This script reports AWS resource usage
#############################

set -ex

# AWS S3
# AWS EC2
# AWS Lambda
# AWS IAM Users

echo "Print list of s3 buckets" > resourceTracker
aws s3 ls --no-cli-pager >> resourceTracker

echo "Print list of ec2 instances" >> resourceTracker
aws ec2 describe-instances --no-cli-pager \
| jq '.Reservations[].Instances[].InstanceId' >> resourceTracker

echo "Print list of lambda functions" >> resourceTracker
aws lambda list-functions --no-cli-pager >> resourceTracker

echo "Print list of iam users" >> resourceTracker
aws iam list-users --no-cli-pager >> resourceTracker
```

---

## Commands Used

### `set -ex`

```bash
set -ex
```

| Option | Meaning                                        |
| ------ | ---------------------------------------------- |
| `-e`   | Exit the script immediately if a command fails |
| `-x`   | Display commands before executing them         |

Useful for debugging shell scripts.

---

## `>`

```bash
echo "Print list of s3 buckets" > resourceTracker
```

Writes output to a file.

If the file already exists, old contents are overwritten.

---

## `>>`

```bash
aws s3 ls --no-cli-pager >> resourceTracker
```

Appends output to a file without overwriting existing contents.

---

## `--no-cli-pager`

Example:

```bash
aws s3 ls --no-cli-pager
```

Disables AWS CLI pager output.

Without this option, long outputs may open inside:

- `less`
- `more`

Using:

```bash
--no-cli-pager
```

prints output directly to the terminal or output file.

---

## AWS CLI Commands

### List S3 Buckets

```bash
aws s3 ls
```

Lists all S3 buckets in the AWS account.

---

### Describe EC2 Instances

```bash
aws ec2 describe-instances
```

Returns detailed EC2 information in JSON format.

---

### List Lambda Functions

```bash
aws lambda list-functions
```

Lists Lambda functions in the AWS account.

---

### List IAM Users

```bash
aws iam list-users
```

Lists IAM users in the AWS account.

---

# Getting Only the EC2 Instance ID

AWS CLI outputs are commonly returned in JSON format.

To filter specific values from JSON, we use:

| Tool | Purpose     |
| ---- | ----------- |
| `jq` | JSON parser |
| `yq` | YAML parser |

Since AWS CLI outputs JSON, we use `jq`.

---

## Example

```bash
aws ec2 describe-instances | jq '.Reservations[].Instances[].InstanceId'
```

---

## Pipe Operator `|`

```bash
|
```

Passes output from one command into another command.

Flow:

```text
AWS CLI Output → jq → Filtered Output
```

---

## jq Query Breakdown

```bash
.Reservations[].Instances[].InstanceId
```

| Part              | Meaning                      |
| ----------------- | ---------------------------- |
| `.Reservations[]` | Loop through reservations    |
| `.Instances[]`    | Loop through EC2 instances   |
| `.InstanceId`     | Extract only the instance ID |

---

## Example Output

```text
"i-06f965214df7d84da"
```

---

## Running the Script

### Make the Script Executable

```bash
chmod +x aws-resource-tracker.sh
```

Adds executable permission to the script.

---

### Run the Script

```bash
./aws-resource-tracker.sh
```

Executes the shell script.

---

## Output File

The script stores the output inside:

```text
resourceTracker
```

View the file using:

```bash
cat resourceTracker
```

or

```bash
less resourceTracker
```

---

## Skills Practiced

- Linux fundamentals
- Shell scripting
- AWS CLI
- IAM Roles
- JSON parsing using `jq`
- Output redirection
- Basic automation
