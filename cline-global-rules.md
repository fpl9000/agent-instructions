# Command Execution

- This computer is a Windows 11 system with Cygwin Bash installed.

- The tool you use to execute commands executes them using the Cygwin Bash shell.  Give Bash commands to this tool.  You do not need to invoke Bash in your commands.

- Most Linux commands are available in the Bash shell, such as `cd`, `ls`, `grep`, `find`, `wc`, `cp`, `mv`, `rm`, `sed`, `awk`, etc.


## Pathnames in Commands

- When giving pathnames to commands within the Bash shell, always use forward slashes as the directory separator, even though this is a Windows system.

- Absolute pathnames in Bash commands should start with a Windows drive letter and a forward slash. For example: `C:/path/to/file`.


## Executing Python Code

- If you need to execute Python code, Python version 3.13.3 is available by running command `python`.

- If you write Python code that has pathnames embedded in the code, the pathnames should follow the same rules as above for pathnames in Bash commands.


## Executing MinGW/MSYS2 Applications

- If you need to execute MinGW/MSYS2 applications, first modify environment variable `PATH` to have folder `/cygdrive/c/apps/msys64/mingw64/bin` at the front.  Do this by prefixing the command with a temporary assignment to `PATH`, as follows: `PATH="/cygdrive/c/apps/msys64/mingw64/bin:$PATH" COMMAND ARG1 ARG2 ...`.  Do not make persistent changes to `PATH`.

- If modifying `PATH` as described in the previous item does not work, next you should try launching the MSYS2 Bash shell by executing `/cygdrive/c/apps/msys64/usr/bin/bash.exe --login` and running the MinGW/MSYS2 application in that shell.  In this case, the `--login` switch is necessary, because it makes Bash source my `~/.bash_profile` script, which sets `PATH` correctly.


# Editing Files

## Newline Conventions

- All new files you create should use Unix-style newlines (a single line-feed character) instead of Windows-style newlines (a carriage return and line-feed pair).

- When editing existing files, always use the same newline convention as the rest of the file.


## Writing Source Code

- Keep lines of source code less than 100 columns wide.

- Avoid single-character identfiers.  In loops, use identifiers such as `index`, `counter`, and `loopCount` instead of single-character identifiers.

- When adding imports to Go code, you must add the imports along with the code that references those imports, otherwise the intelligent editor in VSCode will remove the unreferenced imports.

- All scripts that you create must be Bash scripts.  Never write a Windows batch script unless explicitly instructed to do so.

- In Bash scripts, all variable names must be fully uppercase.  For example: `COUNT=0`, `FILENAME="file.txt"`, etc.

- In Bash scripts, local variables in Bash functions must start with a leading underscore to avoid accidentally shadowing global variables.  For example: `local _COUNTER=0`.

- In Bash scripts use the new test command (`[[ ... ]]`) instead of the traditional test command (`[ ... ]`).  Be careful to use the proper argument syntax for the new test command, such as using `&&` instead of `-a` to indicate Boolean AND operations, and `||` instead of `-o` to indicate Boolean OR operations.


## Comments in Source Code

- Always write well-commented source code.

- Comments should be complete sentences that start with a capital letter and end with a period.

- Sentences in comments should be separated by two spaces for readability.

- Put comments on the line above the code they reference, rather than on the same line.

- Comments can appear on the same line as code only if the comment is very short.  In this case, the comment is exempt from the rule that it must be a complete sentence.

- Comments should explain the purpose and rationale of the code and not simply restate what the code does.

- Do not talk to the user through comments.

- Aim to have nearly the same number of lines of comments as lines of code.  This is a guideline not a hard and fast rule.

- Do not comment trivial code, such as Python and Go `import` statements or the initialization of local variables, unless the comment explains something important for a developer to understand.


# Building Applications

- When running commands to build executables, always make the executable name end with `.exe`, because this is a Windows system.

- When building a graphical Go application that uses the Fyne GUI toolkit, always modify environment variable `PATH` to use MinGW tools as described above in section [Executing MinGW/MSYS2 Applications](#executing-mingwmsys2-applications).

- When building a graphical Go application, always pass switch `-ldflags "-H windowsgui"` so that the application does not create a console window when it is launched.

- When using GCC to build a graphical application, always pass switch `-Wl,--subsystem,windows` so that the application does not create a console window when it is launched.


# Testing Graphical Applications

- You have no way to see the GUI of a graphical application, which makes it difficult for you to test graphical applications.

- Thus, you must test graphical applications as follows:

  1. Tell the user to test the graphical application's GUI before you launch it.

  2. Launch the graphical application, and wait for it to terminate.  If it produces any error messages on standard error or standard output, you will see them just as you see the output of a non-graphical application.

  3. After the app terminates, ask the user for their testing feedback, then wait for the user to enter their feedback.

  4. Take the appropriate actions based on the user's feedback and any output produced by the application.


# Mathematical Caclulations

- When you need to perform mathematical calculations while thinking or planning, always use Python to do the math.  This guarantees mathematical correctness in your reasoning and responses.
