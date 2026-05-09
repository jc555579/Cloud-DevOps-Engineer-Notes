# GitHub API Integration via Shell Script

## Overview

The goal is to list users or collaborators who have read access to a specific GitHub repository.

---

## Ways to Communicate with GitHub

There are two common ways to interact with GitHub:

### 1. GitHub CLI

GitHub CLI allows users to interact with GitHub using commands.

Example:

```bash
gh repo list
```

### 2. GitHub API

GitHub API allows scripts or applications to communicate with GitHub using HTTP requests.

Example:

```bash
curl https://api.github.com
```

In this project, the GitHub API is used with a shell script.

---

## Project Flow

```text
Launch EC2 Instance
        ↓
SSH into EC2
        ↓
Clone the repository
        ↓
Set GitHub username and token
        ↓
Run the shell script
        ↓
Fetch repository collaborators from GitHub API
```

---

## Authentication

Before using the GitHub API, authentication is needed.

Authentication proves that the request is coming from an authorized user.

For this script, GitHub username and Personal Access Token are used (You can navigate to settings > Developer settings > personal access tokens > Tokens (classic)).

Set them as environment variables:

```bash
export username="your-github-username"
export token="your-github-token"
```

These variables will be used inside the shell script.

> Note: Do not hardcode your token directly inside the script. Use environment variables instead.

---

## Running the Script

Command format:

```bash
./list-users.sh <repo-owner> <repo-name>
```

Example:

```bash
./list-users.sh my-org my-repository
```

Where:

- `<repo-owner>` can be a GitHub username or organization name
- `<repo-name>` is the name of the repository

---

## Script Explanation

The script does the following:

1. Defines the GitHub API URL
2. Reads GitHub username and token from environment variables
3. Accepts repository owner and repository name as command-line arguments
4. Sends a request to the GitHub API
5. Filters users who have read access
6. Prints the result

---

## Shell Script Used

```bash
#!/bin/bash

################################
# About:
# This script lists GitHub users/collaborators
# with read access to a repository using GitHub API.
#
# Input:
# ./list-users.sh <repo-owner> <repo-name>
#
# Owner: Joemar Catipon
#
# Date: May 9, 2026
#
# Version: v1
################################

helper()

# GitHub API URL
API_URL="https://api.github.com"

# GitHub username and personal access token from environment variables
USERNAME=$username
TOKEN=$token

# Repository owner and repository name from command-line arguments
REPO_OWNER=$1
REPO_NAME=$2

# Function to make a GET request to the GitHub API
function github_api_get {
    local endpoint="$1"
    local url="${API_URL}/${endpoint}"

    curl -s -u "${USERNAME}:${TOKEN}" "$url"
}

# Function to list users with read access to the repository
function list_users_with_read_access {
    local endpoint="repos/${REPO_OWNER}/${REPO_NAME}/collaborators"

    collaborators="$(github_api_get "$endpoint" | jq -r '.[] | select(.permissions.pull == true) | .login')"

    if [[ -z "$collaborators" ]]; then
        echo "No users with read access found for ${REPO_OWNER}/${REPO_NAME}."
    else
        echo "Users with read access to ${REPO_OWNER}/${REPO_NAME}:"
        echo "$collaborators"
    fi
}

function helper {
    expected_cmd_args = 2
    if [ $# -ne $expected_cmd_args]; then
        echo "please execute the script with required cmd args"
}

echo "Listing users with read access to ${REPO_OWNER}/${REPO_NAME}..."
list_users_with_read_access
```

---

## Function 1: `github_api_get`

```bash
function github_api_get {
    local endpoint="$1"
    local url="${API_URL}/${endpoint}"

    curl -s -u "${USERNAME}:${TOKEN}" "$url"
}
```

This function sends a GET request to the GitHub API.

### What happens inside?

```bash
local endpoint="$1"
```

- Gets the API endpoint passed to the function

Example endpoint:

```text
repos/my-org/my-repository/collaborators
```

```bash
local url="${API_URL}/${endpoint}"
```

- Combines the base API URL and endpoint

Example final URL:

```text
https://api.github.com/repos/my-org/my-repository/collaborators
```

```bash
curl -s -u "${USERNAME}:${TOKEN}" "$url"
```

- Sends the request to GitHub API
- `-s` means silent mode
- `-u` sends username and token for authentication

---

## Function 2: `list_users_with_read_access`

```bash
function list_users_with_read_access {
    local endpoint="repos/${REPO_OWNER}/${REPO_NAME}/collaborators"

    collaborators="$(github_api_get "$endpoint" | jq -r '.[] | select(.permissions.pull == true) | .login')"

    if [[ -z "$collaborators" ]]; then
        echo "No users with read access found for ${REPO_OWNER}/${REPO_NAME}."
    else
        echo "Users with read access to ${REPO_OWNER}/${REPO_NAME}:"
        echo "$collaborators"
    fi
}
```

This function gets the list of collaborators from a repository and filters users with read access.

### What happens inside?

```bash
local endpoint="repos/${REPO_OWNER}/${REPO_NAME}/collaborators"
```

- Creates the GitHub API endpoint for repository collaborators

Example:

```text
repos/my-org/my-repository/collaborators
```

```bash
github_api_get "$endpoint"
```

- Calls the first function
- Sends a request to GitHub API
- Returns collaborator information in JSON format

```bash
jq -r '.[] | select(.permissions.pull == true) | .login'
```

- `jq` is used to read and filter JSON data
- `.[]` loops through each collaborator
- `select(.permissions.pull == true)` checks users with read permission
- `.login` prints only the GitHub username

---

## Meaning of `$1` and `$2`

```bash
REPO_OWNER=$1
REPO_NAME=$2
```

These are command-line arguments.

Example command:

```bash
./list-users.sh my-org my-repository
```

Meaning:

```text
$1 = my-org
$2 = my-repository
```

So the script reads:

```bash
REPO_OWNER=my-org
REPO_NAME=my-repository
```

---

## Tools Used

### AWS EC2

Used as the Linux virtual machine where the script runs.

### GitHub API

Used to get repository collaborator information.

### curl

Used to send HTTP requests to GitHub API.

### jq

Used to filter JSON output from the API.

### Bash / Shell Script

Used to automate the process.

---

## Important Notes

- Keep your GitHub token private
- Do not push tokens to GitHub
- Use environment variables for credentials
- Make sure `jq` is installed before running the script
- The token must have permission to read repository collaborators

---

## Final Note

This project shows how shell scripting can be used to automate GitHub tasks.

It is useful for DevOps because many real-world tasks involve automation, API calls, authentication, and filtering command-line output.
