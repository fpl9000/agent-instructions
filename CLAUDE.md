# Command Execution

- This computer is a Windows 11 system with Git Bash installed.

- Most Linux commands are available in the Git Bash shell, such as `cd`, `ls`, `grep`, `find`, `wc`,
  `cp`, `mv`, `rm`, `sed`, `awk`, `sha1sum`, `sha256sum`, and many others.

- If you need to execute the `rm` command using Bash, always execute it using its full pathname:
  `/bin/rm`.  This avoids a confirmation prompt displayed by my personal `rm` wrapper script.


## Pathnames in Bash Commands

- When giving pathnames to commands executed by the Bash shell, always use forward slashes as the
  directory separator, even though this is a Windows system.

- Absolute pathnames in Bash commands should start with a slash, followed by a Windows drive letter,
  and then a forward slash. For example: `/c/path/to/file`.

  - If this fails with a 'File not found' error (or something similar), try starting the pathname
    with a drive letter followed by a colon, as follows: `C:/path/to/file`.


## Executing Python Code

- If you need to execute Python code, Python version 3.13.3 is available by running command
  `python`.


# Creating and Editing Files

## Newline Conventions

- When you create new files, you should use UNIX-style newlines (a single line-feed character).

- When you modify existing files, you must use the same newline convention as the rest of the file.

- Never convert an existing file from one newline convention to the other.  If you have a compelling
  reason to do this, confirm the operation with the user first, even if auto-confirmation is
  enabled.


## Writing Source Code

- Keep lines of source code less than 100 columns wide.

- Avoid single-character identfiers.  In loops, use meaningful identifiers, such as `index`,
  `counter`, and `loopCount`, instead of single-character identifiers.

- All scripts that you create must be Bash scripts.  Never write a Windows batch script or a
  Powershell script unless explicitly instructed to do so.

- In Bash scripts, all variable names must be fully uppercase.  For example: `COUNT=0`,
  `FILENAME="file.txt"`, etc.

- In Bash scripts, local variables in Bash functions must start with a leading underscore to avoid
  shadowing global variables.  For example: `local _COUNTER=0`.  Conversely, never use a leading
  underscore in a global variable.

- In Bash scripts use the new test command (`[[ ... ]]`) instead of the traditional one (`[ ... ]`).
  Use the proper argument syntax for the new test command, such as using `&&` instead of `-a` to
  indicate Boolean AND operations, and `||` instead of `-o` to indicate Boolean OR operations, as
  well as the other syntax differences required by the new test command.


## Comments in Source Code

- Always write well-commented source code.

- Comments should be complete sentences that start with a capital letter and end with a period.

- Sentences in comments should be separated by two spaces for readability.

- Put comments on the line above the code they reference, rather than on the same line.

- Comments can appear on the same line as code only if the comment is very short.  In this case, the
  comment is exempt from the rule that it must be a complete sentence.

- Comments should explain the purpose and rationale of the code and not simply restate what the code
  does.

- Do not talk to the user through comments in code.

- Aim to have nearly the same number of lines of comments as lines of code.  This is a guideline not
  a hard and fast rule.

- Do not comment trivial code, such as Python and Go `import` statements or the initialization of
  local variables, unless the comment explains something important for a developer to understand.


## Writing Skills

A skill is a collections of files typically in ZIP format with the extension `.skill` that can be
read at inference-time to learn new skills.  When you write a skill, follow these guidelines:

- The skill syntax specification is available at https://agentskills.io.

- Do not add any directories to a skill other than the standard ones defined by the Agent Skills
  specification: `scripts`, `references`, and `assets`.

- Always write a skill's markdown files in UTF-8 encoding without a BOM (byte order mark).
  Non-markdown files might need to deviate from this rule, such as scripts that do not work when
  written a non-ANSI encoding.

- Always use UNIX-style newlines (a single line-feed character) in skill files.

- The final ZIP'ed skill file should always have the `.skill` file extension.


# Building Applications

- When running commands to build executables, always make the executable name end with `.exe`,
  because this is a Windows system.

- When building a graphical Go application, always pass switch `-ldflags "-H windowsgui"` so that
  the application does not create a console window when it is launched.

- When using GCC to build a graphical application, always pass switch `-Wl,--subsystem,windows` so
  that the application does not create a console window when it is launched.


# Testing Graphical Applications

- You have no way to see the GUI of a graphical application, which makes it difficult for you to
  test graphical applications.

- Thus, you must test graphical applications as follows:

  1. Tell the user to test the graphical application's GUI before you launch it.

  2. Launch the graphical application, and wait for it to terminate.  If it produces any error
     messages on standard error or standard output, you will see them just as you see the output of
     a non-graphical application.

  3. After the app terminates, ask the user for their testing feedback, then wait for the user to
     enter their feedback.

  4. Take the appropriate actions based on the user's feedback and any output produced by the
     application.


# Mathematical Caclulations

- When you need to perform mathematical calculations while thinking or planning, always use Python
  to do the math.  This guarantees mathematical correctness in your reasoning and responses.
