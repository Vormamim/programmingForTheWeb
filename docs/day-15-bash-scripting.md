# Day 15 — BASH: Scripting and Automation

**Syllabus:** Develop a web application using shell scripts to make files and directories, and search for text in a text file
**Outcomes:** SE-12-02
**Textbook:** Section 12.5

---

## Key Terms

**Shell script** — A text file containing a sequence of BASH commands that can be run as a program.

**Shebang** — The first line of a shell script: `#!/bin/bash`. Tells the system which interpreter to use.

**Variable** — A named value in a script. Set with `NAME=value`, accessed with `$NAME`.

**Conditional** — An `if/then/else` block that runs different commands based on a condition.

**Loop** — A block of commands that repeats. BASH supports `for`, `while`, and `until` loops.

**Exit code** — A number returned by a command indicating success (0) or failure (non-zero). Check with `$?`.

**chmod** — Command to change file permissions. `chmod +x script.sh` makes a script executable.

---

## Theory

### Creating and Running a Script

```bash
# Create the script file
touch setup.sh

# Make it executable
chmod +x setup.sh

# Run it
./setup.sh
```

### Script Structure

```bash
#!/bin/bash
# This is a comment
# Script: setup.sh
# Purpose: set up a Flask project structure

echo "Setting up project..."

# Variables
PROJECT_NAME="my-web-app"
PYTHON_VERSION=$(python3 --version)

echo "Project: $PROJECT_NAME"
echo "Python: $PYTHON_VERSION"
```

### Variables

```bash
#!/bin/bash

NAME="HSC Project"
PORT=5000
DATE=$(date +%Y-%m-%d)   # command substitution — stores output of date command

echo "Project: $NAME"
echo "Port: $PORT"
echo "Created: $DATE"
```

### Conditionals

```bash
#!/bin/bash

FILE="app.py"

if [ -f "$FILE" ]; then
    echo "$FILE exists"
else
    echo "$FILE not found — creating it"
    touch "$FILE"
fi

# Common test operators
# -f  file exists
# -d  directory exists
# -z  string is empty
# -n  string is not empty
# ==  strings are equal
# !=  strings are not equal
# -eq  numbers are equal
# -gt  greater than
```

### Loops

```bash
#!/bin/bash

# For loop over a list
for dir in static templates tests; do
    mkdir -p "$dir"
    echo "Created: $dir"
done

# For loop over files
for file in *.py; do
    echo "Python file: $file"
done

# While loop
COUNT=0
while [ $COUNT -lt 5 ]; do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### Practical Script — Flask Project Setup

This script automates creating a complete Flask project structure:

```bash
#!/bin/bash
# setup-flask.sh — creates a Flask project skeleton

PROJECT=$1   # first argument passed to script

if [ -z "$PROJECT" ]; then
    echo "Usage: ./setup-flask.sh <project-name>"
    exit 1
fi

if [ -d "$PROJECT" ]; then
    echo "Error: directory '$PROJECT' already exists"
    exit 1
fi

echo "Creating Flask project: $PROJECT"

# Create directory structure
mkdir -p "$PROJECT"/{static/{css,js,images},templates,tests}

# Create files
touch "$PROJECT"/app.py
touch "$PROJECT"/requirements.txt
touch "$PROJECT"/static/css/style.css
touch "$PROJECT"/static/js/script.js
touch "$PROJECT"/templates/index.html

# Write basic Flask app
cat > "$PROJECT/app.py" << 'EOF'
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
EOF

# Write requirements
echo "flask" > "$PROJECT/requirements.txt"

# Write basic template
cat > "$PROJECT/templates/index.html" << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Flask App</title>
    <link rel="stylesheet" href="/static/css/style.css">
</head>
<body>
    <h1>Flask is running</h1>
</body>
</html>
EOF

echo "Done. To run:"
echo "  cd $PROJECT"
echo "  pip install -r requirements.txt"
echo "  python3 app.py"
```

Run with:
```bash
chmod +x setup-flask.sh
./setup-flask.sh my-project
```

### Searching Logs

Web servers generate log files. BASH is useful for searching them:

```bash
#!/bin/bash
# search-logs.sh — find errors in a log file

LOGFILE="server.log"
KEYWORD="ERROR"

if [ ! -f "$LOGFILE" ]; then
    echo "Log file not found: $LOGFILE"
    exit 1
fi

echo "Searching $LOGFILE for '$KEYWORD'..."
COUNT=$(grep -c "$KEYWORD" "$LOGFILE")
echo "Found $COUNT occurrences"
echo "---"
grep -n "$KEYWORD" "$LOGFILE"
```

---

## Objective Response Questions

**1.** What is the purpose of `chmod +x script.sh` before running a shell script?

- A) It installs the script as a system command
- B) It grants the file execute permission so it can be run directly
- C) It compiles the script into a binary
- D) It sets the script to run automatically on startup

**Answer: B**

---

**2.** A script contains `PROJECT=$1`. What does `$1` represent?

- A) The process ID of the script
- B) The first argument passed to the script when it was run
- C) The exit code of the previous command
- D) The current line number

**Answer: B** — `$1` is the first positional argument. `$2` is the second, and so on.

---

**3.** Which line should appear at the very beginning of a BASH script?

- A) `# BASH script`
- B) `bash start`
- C) `#!/bin/bash`
- D) `run bash`

**Answer: C** — The shebang line tells the OS to use `/bin/bash` to interpret the script.

---

## Try It

1. Create `setup-flask.sh` from the example above
2. Make it executable and run it:
   ```bash
   chmod +x setup-flask.sh
   ./setup-flask.sh test-project
   ```
3. Verify the structure was created:
   ```bash
   find test-project -type f
   ```
4. Modify the script to also create a `README.md` in the project root containing the project name and creation date (use `$(date)`)
5. Write a second script `find-todos.sh` that searches all `.py` and `.html` files in the current directory for the word "TODO" and prints each match with its line number
