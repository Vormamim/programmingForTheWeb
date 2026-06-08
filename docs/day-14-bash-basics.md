# Day 14 — BASH: Navigation, Files and Directories

**Syllabus:** Develop a web application using shell scripts to make files and directories, and search for text in a text file
**Outcomes:** SE-12-02
**Textbook:** Section 12.5

---

## Key Terms

**BASH (Bourne Again SHell)** — A command-line interpreter and scripting language used on Linux and macOS. The default shell in GitHub Codespaces.

**Terminal** — The application used to enter BASH commands. In Codespaces, it is the bottom panel.

**Command** — An instruction typed into the terminal. Example: `ls` lists files.

**Flag/option** — A modifier added to a command to change its behaviour. Example: `ls -l` (long format).

**Path** — The location of a file or directory in the filesystem. `/home/user/project/app.py`

**Absolute path** — Full path from the root `/`. Always works regardless of current location.

**Relative path** — Path relative to the current directory. `../docs/index.md`

**stdin / stdout** — Standard input (keyboard) and standard output (terminal screen). Commands read from stdin and write to stdout.

---

## Theory

### Why BASH for Web Development

Web servers run Linux. Deploying, configuring, and managing web applications requires command-line skills. BASH is also used to automate repetitive tasks — creating project structures, running tests, deploying code.

### Navigation Commands

```bash
pwd                  # print working directory — where am I?
ls                   # list files and directories
ls -l                # long format — shows permissions, size, date
ls -la               # long format including hidden files (start with .)
cd docs              # change into docs directory
cd ..                # go up one level
cd ~                 # go to home directory
cd /                 # go to root directory
```

### File and Directory Operations

```bash
# Create
mkdir project               # make a directory
mkdir -p project/src/css    # make nested directories
touch index.html            # create empty file
touch style.css script.js   # create multiple files

# Copy and move
cp index.html backup.html   # copy file
cp -r docs docs-backup      # copy directory (recursive)
mv old-name.html new-name.html  # rename file
mv file.html docs/          # move file to docs/

# Delete
rm file.html                # delete file
rm -r old-project/          # delete directory and contents (careful!)

# View file contents
cat index.html              # print entire file
head -20 index.html         # first 20 lines
tail -20 index.html         # last 20 lines
less index.html             # scroll through file (q to quit)
```

### Searching for Text

```bash
# grep — search for text in files
grep "flask" app.py              # find "flask" in app.py
grep -i "flask" app.py           # case-insensitive
grep -r "import" .               # search all files in current directory
grep -n "def " app.py            # show line numbers
grep -l "TODO" *.py              # list files containing "TODO"

# find — search for files by name
find . -name "*.html"            # find all HTML files
find . -name "*.py" -type f      # find Python files only
find . -name "*.css" -newer style.css  # files newer than style.css
```

### Useful Shortcuts

```bash
# Pipes — send output of one command as input to next
ls -l | grep ".py"          # list only Python files
cat app.py | grep "def "    # list all function definitions

# Redirect output to a file
ls > filelist.txt           # write output to file (overwrites)
ls >> filelist.txt          # append to file

# View command history
history
history | grep "git"        # find previous git commands

# Keyboard shortcuts
Ctrl+C      # cancel running command
Ctrl+L      # clear terminal
Tab         # autocomplete file/directory names
Up arrow    # previous command
```

### Working with the Web Project

```bash
# Common workflow in Codespaces
ls                          # check where you are
mkdir -p static/css static/js static/images   # create project structure
touch templates/index.html  # create template file
grep -r "TODO" .            # find all TODO comments in project
find . -name "*.html"       # list all HTML files
```

---

## Objective Response Questions

**1.** A developer wants to search all Python files in a project for the word "password" to check for hardcoded credentials. Which command is correct?

- A) `find "password" *.py`
- B) `grep -r "password" --include="*.py" .`
- C) `ls -r password .py`
- D) `cat *.py | password`

**Answer: B** — `grep -r` searches recursively, `--include="*.py"` limits to Python files, `.` specifies the current directory.

---

**2.** Which command creates the directory structure `project/static/css` in a single command?

- A) `mkdir project static css`
- B) `touch project/static/css`
- C) `mkdir -p project/static/css`
- D) `cd project && mkdir static && mkdir css`

**Answer: C** — The `-p` flag creates all intermediate directories as needed.

---

**3.** A developer runs `ls > files.txt`. What is the result?

- A) The contents of `files.txt` are listed in the terminal
- B) The output of `ls` is appended to `files.txt`
- C) The output of `ls` is written to `files.txt`, overwriting any existing content
- D) `files.txt` is deleted

**Answer: C** — `>` redirects stdout to a file, overwriting it. `>>` appends.

---

## Try It

In your Codespaces terminal:

1. Run `pwd` — record where you are
2. Create the following structure in one session:
   ```
   mkdir -p static/css static/js static/images templates
   touch static/css/style.css static/js/script.js templates/index.html
   ```
3. Run `find . -name "*.html"` — verify your files appear
4. Add a comment to `script.js`: `echo "// TODO: add interactivity" >> static/js/script.js`
5. Run `grep -r "TODO" .` — verify it finds your comment
6. List all files including hidden ones with `ls -la` — find the `.git` directory and note what it is
