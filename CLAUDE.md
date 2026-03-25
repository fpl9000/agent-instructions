# OS Environment

- This computer is a Windows 11 system with Cygwin installed.

- Cygwin's `/bin` directory is in the `PATH` environment variable.

- When you execute Bash commands, they are executed by the Cygwin Bash shell.

- Most Linux commands are available in the Cygwin Bash shell.

- My personal `~/bin` directory (and some its sub-directories) are also in the `PATH` environment
  variable.

  - They contain scripts and tools that I regularly use, but you are free to use or copy them.
  - Most of them support switch `--help` to display usage, but please read any that you plan to use
    before using them.

- Cygwin symlinks corresponding to each Windows drive letter have been created, as follows:

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

- In a Cygwin Bash shell and all Cygwin apps spawned from it, the value of environment variable
  `HOME` is `/cygdrive/c/franl`.

- In all other processes, including native Windows apps, the value of `HOME` is `C:\franl`, even
  though that is not my windows home directory. This causes most Windows apps to use my Cygwin home
  directory, though some still use `C:\Users\flitt\`.

## Pathnames in Bash Commands

- Relative pathnames in Bash commands should have one of two forms, depending on whether they are being
  given to a Cygwin app or a native Windows app:

  1. When invoking Cygwin apps, relative pathnames should use forward slashes. Example: `COMMAND
     path/to/file`.

  2. When invoking native Windows apps, relative pathnames should use backslashes and be
     single-quoted to escape the backslashes. Example: `COMMAND 'path\to\file'`.

- Absolute pathnames in Bash commands should have one of two forms, depending on whether they are
  being given to Cygwin apps or native Windows apps:

  1. When invoking Cygwin apps, absolute pathnames should start with a slash, followed by a Windows
     drive letter. Example: `COMMAND /c/path/to/file`.

  2. When invoking native Windows apps, absolute pathnames should contain backslashes, should be
     single-quoted to escape the backslashes, and should have a leading drive letter. Example:
     `COMMAND 'C:\path\to\file'`.

- In all cases, if a pathname contains whitespace or Bash metacharacters, the entire pathname must
  be single-quoted, regardless of whether it is being given to a Cygwin app or a native Windows app.

- If single-quotes are not an option for any reason, escape each backslash with another backslash,
  as follows: `C:\\path\\to\\file`. Do this as a last resort, because it hinders readability.

## Installed Compilers and Tools

- The following compilers and tools are installed and available in the Bash shell: `gcc`, `g++`,
  `go`, `rustc`, `cargo`, `python`, `uv`, `uvx`, `npm`, `npx`, `git`, and `gh`.

- Node.js is installed and can be executed using command `node`.

- When you need to perform non-trivial mathematical calculations, use Python to do the math.

- If you need additional compilers or tools installed, confirm with the user before installing them.

# Creating and Editing Files

## File Encoding

- When you create new files containing source code or text, always use UTF-8 text encoding.

- When you modify existing files, use the same text encoding as the rest of the file.

## Newline Conventions

- When you create new files, you should use UNIX-style newlines (a single line-feed character).

- When you modify existing files, you must use the same newline convention as the rest of the file.

- Never convert an existing file from one newline convention to the other. If you have a compelling
  reason to do this, confirm with the user first, even if confirmation-for-edits is enabled.

## Writing Source Code

- Keep lines of source code less than 100 columns wide.

- Avoid single-character identfiers.

- In loops, use meaningful identifiers, such as `index`, `counter`, and `loopCount`, instead of
  single-character identifiers.

- Prefer Python and Bash as scripting languages. Avoid Windows batch scripts and Powershell
  scripts, unless absolutely necessary.

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

- When running commands to build executables, always make the executable name end with `.exe`,
  because this is a Windows system.

- When building a graphical Go application, always pass switch `-ldflags "-H windowsgui"` so that
  the application does not create a console window when it is launched.

- When using GCC to build a graphical application, always pass switch `-Wl,--subsystem,windows` so
  that the application does not create a console window when it is launched.

# Accessing GitHub

- You have read access and write access to my GitHub repositories.

  - My GitHub user name is `fpl9000`.

  - My GitHub profile is located at `https://github.com/fpl9000`.

  - The `git` command can be used to access my GitHub repositories. No credentials are needed
    because SSH access to GitHub is already configured.

- The GitHub CLI app `gh` is also available for accessing GitHub from the command line, though you
  may have an MCP connector or skill that works better for you.
