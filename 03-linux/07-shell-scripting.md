# Shell Scripting

## What is Shell Scripting?

Shell scripting is writing a sequence of commands in a file to automate tasks in a Linux/Unix system.

---

## Shebang

```bash
#!/bin/bash
```

- Tells the OS which interpreter to use
- Must be at the top of the script

---

## Comments

```bash
# This is a comment
```

- Used to describe the script or explain commands

---

## Debugging & Best Practices

```bash
set -x           # Debug mode: prints commands before executing
set -e           # Exits the script when an error occurs
set -o pipefail  # Catches errors inside pipes
```

Avoid using this in one line:

```bash
set -exo
```

Reason:

- It is less flexible and harder to control when debugging.

---

## Common Commands

### System Information

```bash
df -h        # Shows disk usage in human-readable format
nproc        # Shows the number of CPU cores
ps -ef       # Lists running processes
```

Example:

```bash
ps -ef | grep "/bin/bash"
```

- Finds running processes that contain `/bin/bash`.

---

## Filtering Output

```bash
ps -ef | grep /bin/bash | awk '{print $2}'
```

- `grep` filters lines that match a word or pattern.
- `awk` extracts specific columns from the output.
- `$2` means the second column.

---

## File Handling

### Open a File in Read-Only Mode

```bash
vim -r test.txt
```

---

## Finding Files and Directories

```bash
find /etc -name pam.d -type f
```

- Finds a file named `pam.d` inside `/etc`.

```bash
find /etc -name pam.d -type d
```

- Finds a directory named `pam.d` inside `/etc`.

```bash
find /data -name "nodeHealth*"
```

- Finds files or directories starting with `nodeHealth`.

```bash
find /data -iname "nodeHealth*"
```

- Same as `-name`, but ignores uppercase/lowercase.

---

## Data Transfer

### curl

`curl` is used to retrieve data from the internet or APIs.

Examples:

```bash
curl https://example.com/log.txt
```

```bash
curl https://example.com/log.txt | grep "ERROR"
```

```bash
curl -X GET api.example.com
```

Common use cases:

- Fetch logs
- Call APIs
- Test endpoints
- Get website responses

---

### wget

`wget` is used to download files from the internet.

Example:

```bash
wget https://example.com/log.txt
```

Difference:

- `curl` is commonly used for APIs and viewing responses.
- `wget` is commonly used for downloading files.

---

## Loops

Loops allow a block of code to run repeatedly.

Example:

```bash
for item in apple banana cherry; do
  echo "I like $item"
done
```

Output:

```bash
I like apple
I like banana
I like cherry
```

---

## Trap Signals

`trap` is used to handle signals like `CTRL+C`.

Example:

```bash
trap "echo Don't use CTRL+C" SIGINT
```

- `SIGINT` is triggered when the user presses `CTRL+C`.
- This helps control what happens when a script is interrupted.

---

## Scheduling Script Execution

### Crontab

`crontab` is used to schedule scripts or commands.

Open crontab editor:

```bash
crontab -e
```

Example use cases:

- Run a script every day
- Clean logs automatically
- Run health checks on schedule

---

## Links

### Soft Link

A soft link, also called a symbolic link, points to another file.

If the original file is deleted, the soft link will stop working.

Example:

```bash
ln -s original.txt shortcut.txt
```

---

### Hard Link

A hard link is another reference to the same file data.

Even if the original file is deleted, the hard link can still access the data.

Example:

```bash
ln original.txt hardlink.txt
```

---

## Log Management

### Sort

`sort` is used to sort lines in a file.

Example:

```bash
sort names.txt
```

---

### Log Rotation

`logrotate` is used to manage large log files.

It can:

- Compress old logs using gzip
- Rotate logs daily, weekly, or monthly
- Delete old logs after a certain number of days

Example use case:

- Managing large logs generated every day

---

## Disadvantages of Shell Scripting

Shell scripting also has limitations:

- Errors can happen easily
- Execution speed can be slow
- Not ideal for large and complex applications
- Limited data structures compared to other programming languages
- Each shell command usually starts a new process

---

## Final Note

Shell scripting is commonly used by DevOps Engineers to automate tasks, check system health, manage logs, transfer data, and schedule jobs.

It is useful for simple automation and system administration tasks.
