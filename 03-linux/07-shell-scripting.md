# Shell Scripting Fundamentals

Author: Joemar Catipon  
Level: Beginner  
Focus: Linux automation, DevOps basics, and practical scripting

---

## 1. What is Shell Scripting?

Shell scripting means writing Linux commands inside a file so they can be executed automatically.

Instead of typing commands one by one in the terminal, you can place them inside a script.

Example:

```bash
#!/bin/bash

echo "Updating system..."
sudo apt update
sudo apt upgrade -y
```

This script updates the system using commands that would normally be typed manually.

---

## 2. Basic Script Structure

```bash
#!/bin/bash

#############################
# Author: Joemar Catipon
# Date: May 11, 2026
# Version: v1
# Description: Basic shell script structure.
#############################

echo "Hello, Jom!"
```

Explanation:

```bash
#!/bin/bash
```

This is called the **shebang**. It tells Linux to run the script using Bash.

```bash
# This is a comment
```

Comments are ignored by the shell. They are useful for explaining what your script does.

```bash
echo "Hello, Jom!"
```

The `echo` command prints text to the terminal.

---

## 3. Creating and Running a Script

Create a file:

```bash
nano hello.sh
```

`nano` opens a terminal text editor where you can write your script.

Make the script executable:

```bash
chmod +x hello.sh
```

`chmod +x` gives the script permission to run as a program.

Run the script:

```bash
./hello.sh
```

`./` means run the file from the current directory.

---

## 4. Variables

Variables store values.

```bash
#!/bin/bash

name="Jom"
course="BSIT Web Development"

echo "Hello, $name"
echo "Course: $course"
```

Correct syntax:

```bash
name="Jom"
```

Wrong syntax:

```bash
name = "Jom"
```

In Bash, do not put spaces around the `=` sign when assigning variables.

---

## 5. Reading User Input

```bash
#!/bin/bash

read -p "Enter your name: " name

echo "Hello, $name"
```

`read` accepts user input from the terminal.

`-p` allows you to show a prompt before the user types their input.

---

## 6. Command-Line Arguments

Command-line arguments are values passed when running a script.

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Number of arguments: $#"
echo "All arguments: $@"
```

Run:

```bash
./script.sh Jom AWS
```

Common argument variables:

| Variable | Meaning             |
| -------- | ------------------- |
| `$0`     | Script name         |
| `$1`     | First argument      |
| `$2`     | Second argument     |
| `$#`     | Number of arguments |
| `$@`     | All arguments       |

---

## 7. Conditional Statements

### Basic `if` Statement

```bash
#!/bin/bash

age=20

if [ "$age" -ge 18 ]; then
  echo "You are an adult."
else
  echo "You are a minor."
fi
```

Common number comparisons:

| Operator | Meaning               |
| -------- | --------------------- |
| `-eq`    | Equal                 |
| `-ne`    | Not equal             |
| `-gt`    | Greater than          |
| `-lt`    | Less than             |
| `-ge`    | Greater than or equal |
| `-le`    | Less than or equal    |

### String Comparison

```bash
#!/bin/bash

name="Jom"

if [ "$name" = "Jom" ]; then
  echo "Welcome, Jom!"
else
  echo "Unknown user."
fi
```

Always quote variables like this:

```bash
"$name"
```

This helps prevent errors when the variable is empty or contains spaces.

---

## 8. File and Directory Checks

```bash
#!/bin/bash

file="notes.txt"

if [ -f "$file" ]; then
  echo "File exists."
else
  echo "File does not exist."
fi
```

Common file checks:

| Check          | Meaning                        |
| -------------- | ------------------------------ |
| `-f file`      | Checks if a file exists        |
| `-d directory` | Checks if a directory exists   |
| `-e path`      | Checks if a path exists        |
| `-r file`      | Checks if a file is readable   |
| `-w file`      | Checks if a file is writable   |
| `-x file`      | Checks if a file is executable |

---

## 9. Loops

### For Loop

```bash
#!/bin/bash

for name in Jom Alex Maria; do
  echo "Hello, $name"
done
```

Example with files:

```bash
#!/bin/bash

for file in *.txt; do
  echo "Found text file: $file"
done
```

### While Loop

```bash
#!/bin/bash

count=1

while [ "$count" -le 5 ]; do
  echo "Count: $count"
  count=$((count + 1))
done
```

`$(( ))` is used for arithmetic operations in Bash.

---

## 10. Functions

Functions help organize reusable code.

```bash
#!/bin/bash

greet_user() {
  echo "Hello, $1"
}

greet_user "Jom"
greet_user "AWS Learner"
```

Inside a function, `$1` means the first argument passed to that function.

---

## 11. Exit Status

Every Linux command returns an exit status.

| Exit Status | Meaning          |
| ----------- | ---------------- |
| `0`         | Success          |
| Non-zero    | Error or failure |

Example:

```bash
#!/bin/bash

if mkdir test-folder; then
  echo "Folder created successfully."
else
  echo "Failed to create folder."
fi
```

This is cleaner than manually checking `$?`.

---

## 12. Error Handling

For safer scripts, add this near the top:

```bash
set -euo pipefail
```

Meaning:

| Option            | Meaning                                 |
| ----------------- | --------------------------------------- |
| `set -e`          | Stop the script when a command fails    |
| `set -u`          | Error when using an undefined variable  |
| `set -o pipefail` | Fail if any command in a pipeline fails |

Example:

```bash
#!/bin/bash
set -euo pipefail

echo "Installing nginx..."
sudo apt update
sudo apt install nginx -y

echo "Nginx installed successfully."
```

---

## 13. Redirection and Logging

Overwrite output to a file:

```bash
echo "Hello" > output.txt
```

Append output to a file:

```bash
echo "New log entry" >> output.txt
```

Redirect errors:

```bash
ls /wrong/path 2> error.log
```

Redirect both output and errors:

```bash
./script.sh > output.log 2>&1
```

Common cron logging example:

```bash
0 8 * * * /home/ubuntu/script.sh >> /home/ubuntu/script.log 2>&1
```

---

## 14. Working with curl

`curl` is commonly used to call APIs.

Basic request:

```bash
curl https://api.github.com
```

Save output to a file:

```bash
curl -o response.json https://api.github.com
```

Use an authorization header:

```bash
curl -H "Authorization: token YOUR_TOKEN" https://api.github.com/user
```

For GitHub API scripts, avoid hardcoding tokens inside the script. Use environment variables instead.

```bash
export username="your-github-username"
export token="your-github-token"
```

---

## 15. Working with wget

`wget` is commonly used to download files.

```bash
wget https://example.com/file.zip
```

Save with another filename:

```bash
wget -O myfile.zip https://example.com/file.zip
```

`-O` lets you choose the output filename.

---

## 16. Crontab Basics

Open crontab:

```bash
crontab -e
```

Cron format:

```bash
minute hour day month weekday command
```

Run a script every day at 8 AM:

```bash
0 8 * * * /home/ubuntu/scripts/backup.sh
```

Run every 5 minutes:

```bash
*/5 * * * * /home/ubuntu/check-server.sh
```

Important reminder:

Use full paths inside cron scripts because cron has a limited environment.

---

## 17. Useful Beginner Template

```bash
#!/bin/bash

#############################
# Author: Joemar Catipon
# Date: May 11, 2026
# Version: v1
# Description: Describe what this script does.
#############################

set -euo pipefail

main() {
  echo "Starting script..."

  # Your commands here

  echo "Script completed successfully."
}

main "$@"
```

Why this template is good:

- It includes script information.
- It uses safer error handling.
- It organizes the script using a `main` function.
- It passes all command-line arguments using `"$@"`.

---

## 18. Beginner Best Practices

1. Always quote variables.

```bash
"$name"
```

2. Use meaningful variable names.

```bash
backup_dir="/home/ubuntu/backups"
```

3. Check arguments before using them.

```bash
if [ "$#" -ne 2 ]; then
  echo "Usage: $0 <owner> <repo>"
  exit 1
fi
```

4. Do not hardcode secrets.

Bad:

```bash
token="my-secret-token"
```

Better:

```bash
export token="your-token"
```

5. Test scripts manually before adding them to cron.

6. Add comments, but do not over-comment obvious commands.

7. Use logs for scheduled scripts.

```bash
./backup.sh >> backup.log 2>&1
```

---

## 19. What to Learn Next

After the basics, learn these:

- `jq` for reading JSON responses properly
- `awk` for text processing
- `sed` for text replacement
- `grep` for searching text
- `find` for searching files
- `xargs` for passing command output to another command
- AWS CLI automation
- GitHub API automation
- Server health monitoring scripts

Example with `jq`:

```bash
curl -s https://api.github.com/users/octocat | jq '.login'
```

`jq` is better than using `grep` and `awk` when handling JSON.

---

## 19. Useful Commands to Learn Next

After learning the shell scripting fundamentals, these commands are very useful for real automation tasks.

### `jq` for reading JSON responses properly

`jq` is used to read and filter JSON data.

This is useful when working with APIs such as the GitHub API or AWS CLI JSON output.

Example:

```bash
curl -s https://api.github.com/users/octocat | jq '.login'
```

This extracts the `login` value from the JSON response.

`jq` is better than using `grep` and `awk` when handling JSON because JSON has structure.

---

### `awk` for text processing

`awk` is used to process text by columns or patterns.

Example:

```bash
df -h | awk '{print $1, $5}'
```

This prints selected columns from the `df -h` command output.

Common use cases:

- Extracting specific columns
- Filtering command output
- Processing logs
- Creating simple reports

---

### `sed` for text replacement

`sed` is used to search and replace text.

Example:

```bash
sed 's/error/warning/g' app.log
```

This replaces the word `error` with `warning` in the output.

Common use cases:

- Replacing text in files
- Cleaning command output
- Editing configuration files
- Modifying logs or text data

---

### `find` for searching files

`find` is used to search files and directories.

Example:

```bash
find /home/ubuntu -name "*.log"
```

This searches for `.log` files inside `/home/ubuntu`.

Common use cases:

- Finding files by name
- Finding files by extension
- Finding old files
- Finding large files
- Searching directories recursively

Example for finding old log files:

```bash
find /var/log/myapp -name "*.log" -type f -mtime +7
```

This finds `.log` files older than 7 days.

---

### `xargs` for passing command output to another command

`xargs` takes output from one command and passes it as input to another command.

Example:

```bash
find . -name "*.log" | xargs ls -lh
```

This finds `.log` files and passes them to `ls -lh`.

Common use cases:

- Running commands on many files
- Deleting files found by `find`
- Processing long command output
- Combining commands in scripts

Example:

```bash
find . -name "*.tmp" | xargs rm
```

This removes all `.tmp` files found by `find`.

Be careful with delete commands. Always test first before using `rm`.

---

## 20. Simple Summary

Shell scripting is useful for:

- Automating repeated Linux commands
- Server setup
- Backups
- Health checks
- Log cleanup
- API calls
- Scheduled tasks using cron
- AWS CLI and DevOps workflows

Good mindset:

> If I repeat a Linux command more than twice, I should consider turning it into a shell script.
