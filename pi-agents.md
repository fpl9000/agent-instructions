# Your Harness

- You are running in the Pi minimal agent harness. Use the harness-supplied tools (bash, read, write, and edit)
  to examine and manipulate the local computer.

  - Never recursively `grep` the entire filesystem. Recursive `grep` commands are fine in directories that do
    not contain too many files.

  - Do not use `find` to search the filesystem for files by name. To find files by name, use the `es` command
    in Bash. It's a CLI front-end to the Everything search tool that does extremely fast filesystem-wide filename searches.

  - Run `es --help` for full usage. This is the abbreviated usage:

    ```
    usage: es [ -d | -s | -f ] [ -u | -w ] [ STRING | -r REGEX ]

    Matching is always case-insensitive. Matches across '/' chars in the pathname. Sorts by
    absolute pathname by default. Quote STRING and REGEX to escape shell metacharacters.

    -r  =>  Match filenames using the given REGEX instead of a fixed STRING.
    -d  =>  Sort results by modification time (newest first).
    -s  =>  Sort results by size (largest first)
    -f  =>  Show only the result pathnames without the DTM or size.
    -u  =>  Display pathname with UNIX-style forward slashes (default).
    -w  =>  Display pathname with Windows-style backslashes.
    ```

# OS Environment

- This computer is a Windows 11 system with Cygwin installed.

- Cygwin's `/bin` directory is in the `PATH` environment variable.

- Your Bash commands are executed by the Cygwin Bash shell at `C:\apps\cygwin\bin\bash.exe`, which
  is at `/bin/bash` inside any Cygwin app.

- Most Linux commands are available in the Cygwin Bash shell. If you need any that are not
  installed, ask me to install them.

- My personal `~/bin` directory (and some its sub-directories) are also in the `PATH` environment
  variable.

  - Directory `~/bin` and its sub-directories contain scripts and tools that I regularly use. You're
    free to use or copy them, but read them before running them.

- The Cygwin man pages are installed. Use `man COMMAND >/tmp/man.txt` to save a man page in plain
  text that you can read.

- Cygwin symlinks corresponding to each Windows drive letter have been created as follows:

  ```
  /a -> /cygdrive/a
  /b -> /cygdrive/b
  /c -> /cygdrive/c
  ...
  /z -> /cygdrive/z
  ```

  - Keep in mind that native Windows apps and commands cannot follow Cygwin symlinks, even when
    invoked from a Cygwin Bash shell.

  - The target of a Cygwin symlink can found using `readlink -m SYMLINK`.

## Home Directories

- My Cygwin home directory is `C:\franl\`. My Windows home directory is `C:\Users\flitt\`.

- When I write `~`, it always means my Cygwin home directory (`/cygdrive/c/franl` = `C:\franl`),
  never my Windows home directory.

- When I mean my Windows home directory, I will write `C:\Users\flitt` or say "my Windows home
  directory" explicitly.

- In a Cygwin Bash shell and all Cygwin apps, the value of environment variable `HOME` is
  `/cygdrive/c/franl`, my Cygwin home directory.

- In all Windows apps, even those spawned by a Cygwin app, the value of `HOME` is `C:\franl`, even
  though that is not my windows home directory. This causes most Windows apps to use my Cygwin home
  directory, though some still use `C:\Users\flitt\`.

- The above is also true when a Cygwin or Windows app spawns an app of the other kind.

## Pathnames in Bash Commands

- I communicate pathnames to you in Cygwin style (for example, `/c/franl/bin/...`, `/c/temp`,
  `~/bin`), not Windows style.

  - Interpret my paths as Cygwin-style, and translate them yourself to whatever format each tool
    needs: Cygwin `/c/...` for the Bash tool, and Windows `C:\...` for the harness file tools
    (Read/Write/Edit) and native Windows apps. That translation is your responsibility, not mine.

- For relative pathnames in Bash commands:

  1. When invoking Cygwin apps, relative pathnames should use forward slashes. Example: `COMMAND
     path/to/file`.

  2. When invoking native Windows apps, relative pathnames should use backslashes and be
     single-quoted to escape the backslashes. Example: `COMMAND 'path\to\file'`.

- For absolute pathnames in Bash commands:

  1. When invoking Cygwin apps, absolute pathnames should start with a slash, followed by a Windows
     drive letter. Example: `COMMAND /c/path/to/file`.

  2. When invoking native Windows apps, absolute pathnames should contain backslashes, should be
     single-quoted to escape the backslashes, and should have a leading drive letter. Example:
     `COMMAND 'C:\path\to\file'`.

- In all cases, if a pathname contains whitespace or Bash metacharacters, the entire pathname must
  be single-quoted, regardless of whether it is being given to a Cygwin app or a native Windows app.

- If single-quotes are not an option for any reason, escape each backslash with another backslash,
  as follows: `C:\\path\\to\\file`.

## Installed Compilers and Tools

- The following compilers and tools are installed and available in the Bash shell: `gcc`, `g++`,
  `go`, `rustc`, `cargo`, `python`, `uv`, `uvx`, `npm`, `npx`, `git`, and `gh`.

- Node.js is installed and can be executed using command `node`.

- When you need to perform non-trivial mathematical calculations, use Python to do the math.

- If you need additional compilers or tools installed, confirm with the user before installing them.

## Choosing a Python Interpreter

- Three Python interpreters are available, and they behave differently in ways that cause subtle
  bugs, so choose deliberately:

  - Cygwin's `python` (`/usr/bin/python`) is a POSIX build: `os.path` uses posixpath (forward-slash)
    semantics, `os.getcwd()` returns a `/cygdrive/...` path, and it can follow the Cygwin drive
    symlinks (`/c` ... `/z`).

  - The native Windows Python (`/c/Windows/py.exe`, which runs `C:\Program
    Files\Python313\python.exe`) uses backslash-aware `ntpath` semantics, `os.getcwd()` returns a
    `C:\...` path, and it cannot follow Cygwin symlinks.

- Default to Cygwin's `python` for scripts you run yourself from the Bash shell, because it matches
  the shell's filesystem view (pipes, redirects, and the `/c` ... `/z` symlinks).

- For a script that something else launches, use and test with that same interpreter. Testing under
  the wrong interpreter can pass while production fails.

- When a script may run under either interpreter, parse pathnames separator-agnostically (for
  example, split on both `/` and `\`) rather than relying on `os.path`.

- For standalone script deliverables, keep using PEP 723 metadata with `uv`/`uvx` (see the Python
  Scripting Guidelines below); `uv` provisions its own interpreter.

# Creating and Editing Files

## File Encoding

- When you create new files containing source code or text, always use UTF-8 text encoding.

- When you modify existing files, use the same text encoding as the rest of the file.

## Newline Conventions

- When you create new files, you should use UNIX-style newlines (a single line-feed character).

- When you modify existing files, you must use the same newline convention as the rest of the file.

- Never convert an existing file from one newline convention to the other. If you have a compelling
  reason to do this, confirm with the user first.

## Writing Markdown

- It's OK to have indefinitely long lines (despite the below rule to limit the length of source code
  lines), because Markdown renderers handle that gracefully.

- Avoid hard line breaks with `<br/>`, because not all renderers handle that.

- Instead of `<br/>`, use two trailing spaces at the end of a line to indicate a line break.

- When creating a new Markdown document, list yourself as the author.

## Writing Source Code

- Keep lines of source code less than 100 columns wide.

- Avoid single-character identfiers.

- In loops, use meaningful identifiers, such as `index`, `counter`, and `loopCount`, instead of
  single-character identifiers.

- Prefer Python and Bash as scripting languages. Avoid Windows batch scripts and Powershell scripts,
  unless absolutely necessary.

### Python Scripting Guidelines

- In Python scripts, use PEP 723 metadata to specify dependencies.

- This allows `uv` and `uvx` to automatically obtain dependencies when the script is run.

### Bash Scripting Guidelines

- In Bash scripts, all variable names must be fully uppercase, as follows: `COUNT=0`,
  `FILENAME="file.txt"`, etc.

- In Bash scripts, local variables in Bash functions must start with a leading underscore to avoid
  shadowing global variables, as follows: `local _COUNTER=0`. Conversely, never use a leading
  underscore in a global variable.

- Prefer the new test command (`[[ ... ]]`) instead of the traditional one (`[ ... ]`).

  - Use the proper argument syntax for the new test command, such as using `&&` instead of `-a` to
    indicate Boolean AND operations, and `||` instead of `-o` to indicate Boolean OR operations.

### Comments in Source Code

- Always write well-commented source code.

- Comments should be complete sentences.

- Put comments on the line above the code they reference, rather than on the same line.

- Comments can appear on the same line as code only if the comment is very short. In this case, the
  comment is exempt from the rule that it must be a complete sentence.

- Comments should explain the purpose and rationale of the code and not simply restate what the code
  does.

- Do not talk to the user through comments in code.

- Aim to have nearly the same number of lines of comments as lines of code. This is a guideline not
  a hard and fast rule.

- Do not comment trivial code, such as Python and Go `import` statements or the initialization of
  local variables, unless the comment explains something important for a developer to understand.

# Accessing GitHub

- You have read access and write access to my GitHub repositories, as follows:

  - Use command `git` to access my GitHub repositories. No credentials are needed because SSH access
    to GitHub is already configured.
  - My GitHub user name is `fpl9000`.
  - My GitHub profile is located at `https://github.com/fpl9000`.

## Writing Skills

A skill is a collection of files (commonly packaged in ZIP format) with the extension `.skill` that
can be read by an AI at inference-time to learn new skills. When you write a skill, follow these
guidelines:

- The skill syntax specification is available at `https://agentskills.io`.

- Do not add any directories to a skill other than the standard ones defined by the Agent Skills
  specification, which are `scripts`, `references`, and `assets`.

- Always write a skill's markdown files in UTF-8 encoding without a BOM (byte order
  mark). Non-markdown files in a skill might need to deviate from this rule, such as scripts that do
  not work when written a non-ANSI encoding.

- Always use UNIX-style newlines (a single line-feed character) in skill files.

- The final ZIP'ed skill file should always have the `.skill` file extension.

# Building Applications

- When running commands to build executables, make sure the executable name ends with `.exe`,
  because this is a Windows system.

- When building a graphical Go application, always pass switch `-ldflags "-H windowsgui"` so that
  the application does not create a console window when it is launched.

- When using GCC to build a graphical application, always pass switch `-Wl,--subsystem,windows` so
  that the application does not create a console window when it is launched.
