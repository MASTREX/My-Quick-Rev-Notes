shell: bourne shell
Bash: bourne again shell (enhanced version of shell)

---------------------------
## Essential Commands:
- `cd`: Change directory.
- `ls`: List directory contents.
- `pwd`: Print working directory.
- `mkdir`: Make directory.
- `rm`: Remove files or directories.
- `cp`: Copy files or directories.
- `mv`: Move or rename files or directories.
- `touch`: Create an empty file.
- `cat`: Concatenate and display files.
- `grep`: Search text patterns.
- `find`: to find with matching a pattern or criteria

  syntax: `find <path> [options value [options value]...]`
  ```bash
  find ~/Documents/ -iname "*txt" -type f,l -maxdepth 1 -perm 0600 -exec grep -Hi penguin {} \;
  ```
- `chmod`: Change file permissions.
- `chown`: Change file owner and group.
- `xargs`: Construct an argument list from stdin and run a command

  syntax: `...stdin... | xargs -n 100 -P 2 chmod 775`
- `read`: Take user input
- `basename` and `dirname`: basename gives the filename and dirname give the dir path from complete file path
  ```bash
  PATHNAME=/etc/system/info.config
  > basename $PATHNAME
  info.config

  > dirname $PATHNAME
  /etc/system

  ```
- `man <>`: help doc for non builtins binaries
- `help <>`: help doc for builtin system binaries

## System Management:
- `top`: Display Linux processes.
- `ps`: Show currently running processes.
- `kill`: Terminate processes.
- `sudo`: Execute a command as another user.
- `df`: Display disk space usage.
- `du`: Estimate file space usage.
- `free`: Display amount of free and used memory.
- `reboot`: Reboot the system.
- `shutdown`: Shutdown the system.
- `hostname`: Print or set system name.
- `ifconfig` (or `ip`): Configure network interfaces.
- `ping`: Test network connectivity.
## Package Management:
- `apt-get` (or `apt`): Package manager for Debian-based systems.
- `yum`: Package manager for Red Hat-based systems.
- `pip`: Package installer for Python.
- `npm`: Package manager for Node.js.
- `composer`: Dependency manager for PHP.
## Version Control:
- `git`: Distributed version control system.
- `svn`: Subversion version control system.
## Automation and Scripting:
- `cron`: Job scheduler.
- ***bash scripts***: Writing and executing bash scripts for automation.
## Networking:
- `ssh`: Secure shell for remote access.
- `scp`: Securely copy files between hosts.
- `rsync`: Remote file synchronization.
- `wget`: Retrieve files from the web.
## Monitoring and Logging:
- `tail`: Output the last part of files.
- `grep`: Search text patterns in files.
- `sed`: Stream editor for filtering and transforming text.
## Text Processing:
- `awk`: Text processing tool.
- `cut`: Remove sections from each line of files.
  - `-b` specifiy byte like -b 1-3,7
  - `-c` select specific characters rather than byte. Good for multi byte encodings
  - `-d` delimiter used as field separator, default is `TAB`
  - `-f` field list ex -f 1-2,4
  - `-complement` print not selected fields/characters
  - `--output-delimiter=%` delemiter used in output, by default it is same as input delimiter described in -d

- `sed`: stream editor
  `sed 's/match-patter/replace-pattern' filename`
- `sort`: Sort lines of text files.
- `uniq`: Report or omit repeated lines.
- `tr`: Translate or delete characters.
## Docker and Containers:
- `docker`: Containerization platform.
- `docker-compose`: Define and run multi-container Docker applications.
## Deployment and Configuration Management:
- `ansible`: Configuration management and deployment tool.
- `chef`: Infrastructure automation framework.
- `puppet`: Configuration management tool.
- `terraform`: Infrastructure as code tool.
## Continuous Integration/Continuous Deployment (CI/CD):
- `jenkins`: Automation server for CI/CD pipelines.
- `gitlab-ci`: CI/CD integrated with GitLab.
- `travis-ci`: CI service for GitHub projects.
- `circle-ci`: CI/CD platform for continuous integration and delivery.
## Cloud Computing:
- `aws-cli`: AWS Command Line Interface.
- `gcloud`: Google Cloud SDK.
- `az-cli`: Azure Command-Line Interface.
## Logging and Monitoring:
- `prometheus`: Monitoring and alerting toolkit.
- `grafana`: Metrics dashboard and graph editor.
- `elasticsearch`: Distributed search and analytics engine.
- `kibana`: Data visualization dashboard for Elasticsearch.
## Security:
- `nmap`: Network exploration tool and security scanner.
- `fail2ban`: Intrusion prevention software.
## Miscellaneous:
- `curl`: Transfer data from or to a server.
- `tmux`: Terminal multiplexer.
- `jq`: Command-line JSON processor.
----------------------

# Important points
- shebang: #!/bin/bash. This is honoured by operating system if script is executed by itself like `./myscript.sh`
- if a variable is passed like this `ENV_VAR=value executable param1 param2` then it is defined for the period of that command execution only.
- when we defined a env variable using `export` then it is pass to all sub or child process called from that environment



# Variables
```bash
# you can not have spaces before and after the = sign.
AGE=25
echo $AGE
# OR
echo ${AGE}		# good practice standard

readonly AGE		# marks the variable as readonly
unset AGE

func() {
  # local variable is only applicable in the function scope
	local AGE=25		# local variable (by default variables are global)
}
```

if parameter are passed to the command like this `./myscript.sh para1 para2!` then they can be referenced like by $\<parameter no\>
- $1: for first parameter
- $2: for second parameter
- $@: for all parameter
- $0: reference the script itself (can be used to self destruct by `rm -f $0` in the script)

# take input with 'read'
`-p` is use for prompt msg
`-r` disable escape backslash
`-s` Does not echo user's input
`-t` timeout in seconds

```bash
#!/bin/bash
read -p "What is your name? " name
echo "Hi there $name"
echo "Welcome to DevDojo!"
```


| Variable Type    | Syntax              | Description                                          |
|------------------|---------------------|------------------------------------------------------|
| Array            | declare -a variable | declare an indexedarray variable that stores strings |
| Associated Array | declare -A variable | Associated Array (key value pair)                                     |
| Integer          | declare -i variable | numeric value to store in variable                   |
| Readonly         | declare -r variable | readonly variable, cannot be changed or unset        |
| Export           | declare -x variable | export the variable and used by all child process    |


`printenv` or `env` for getting environment variables. Variables are case sensitive

# Special Characters
#### Wildcards
   - `?` single character
   - `*` mulitiple character
   - `[]` character set

#### Command seperator
   - `;` executes each even if previous one fails
   - `&&` stops if previous one fails

#### Background Process
   - `command &` executes the command in background and return the process ID

#### Input redirection
   - To take file as input through stream use left-angle bracket `<`
   - ex: `sort < words.txt`

#### Output redirection
   - Use the right-angle bracket `>` to redirect the output from a command (typically, into a file);
   - ex: `ls > files.txt`
   - use digit 2 to redirect errors. `2>/dev/null` directs error to null

#### Pipe
   - `|` chains commands together. It takes the output from one command and feeds it to the next as input.

> To use special character as it is use single quotes arround them or use backslash
> <br>example: `echo 'Today is $(date)'`
> <br>example: `echo "Today is \$(date)"`


# Comments
```bash
# single line

:'
multiline1
multiline2
'
```

# Array
```bash
array=(one two three)
# in case of array you have to use curly braces
echo ${array[0]}	# first index
echo ${array[-1]}	# last index
array[key1]=one		# for associated or key-vaule type
echo ${array:0:2}	# slicing, 0 inclusive, 2 exclusive
echo ${array::5}	# slicing, defaults to 0, 5 exclusive
echo ${array:3}		# slicing, start from index 3 to end

echo {1..5}
# Outputs to {1 2 3 4 5}


# for loop on array
nums=(1 3 12)
for i in "${nums[@]}"
do
  echo "$i"
done

${array[@]}		Get All elements
${array[*]}		Get All elements
${!array[!]}	Get All indexes
${#array[!]}	Array length
${#array[@]}	Array length
```

# Conditionals

> - conditon is an expression that evaluates to `true` or `false`
> - Space is required before and after `[` and `]`
> - A semicolon before then is required

```bash
# True if file exists. (deprecated)
[[ -a ${file} ]]

# True if file exists.
# preferred in modern scripts
[[ -e ${file} ]]

# True if file exists and is a block special file.
[[ -b ${file} ]]

# True if file exists and is a character special file.
[[ -c ${file} ]]

# True if file exists and is a directory.
[[ -d ${file} ]]

# True if file exists and is a regular file.
[[ -f ${file} ]]

# True if file exists and is a symbolic link.
[[ -h ${file} ]]
# True if file exists and is a symbolic link.
[[ -L ${file} ]]

# True if file exists and is readable.
[[ -r ${file} ]]

# True if file exists and has a size greater than zero.
[[ -s ${file} ]]

# True if file exists and is writable.
[[ -w ${file} ]]

# True if file exists and is executable.
[[ -x ${file} ]]

# True if the shell variable varname is set (has been assigned a value).
[[ -v ${varname} ]]

# True if the length of the string is zero.
[[ -z ${string} ]]

# True if the length of the string is non-zero.
[[ -n ${string} ]]

# String comparison true if equal
[[ ${string1} == ${string2} ]]
# True if the strings are not equal.
[[ ${string1} != ${string2} ]]
# True if string1 sorts before string2 lexicographically.
[[ ${string1} < ${string2} ]]
# True if string1 sorts after string2 lexicographically.
[[ ${string1} > ${string2} ]]

# Number comparison
[[ ${arg1} -eq ${arg2} ]]  # ==
[[ ${arg1} -ne ${arg2} ]]  # !=
[[ ${arg1} -lt ${arg2} ]]  # <
[[ ${arg1} -le ${arg2} ]]  # <=
[[ ${arg1} -gt ${arg2} ]]  # >
[[ ${arg1} -ge ${arg2} ]]  # >=

# AND / OR / NOT
[[ test_case_1 && test_case_2 ]] # And
[[ test_case_1 || test_case_2 ]] # Or
[[ ! test_condition ]] # Not

# returns true if the command was successful without any errors
[[ $? -eq 0 ]]
# returns true if the command was not successful or had errors
[[ $? -gt 0 ]]


```
> `$?` stores the last executed command reuslt status like 0 for successful execution and greater than 0 for some error

## if elif else
```bash
if [[ condition1 ]]; then
	# true code
elif condition2
then
	# condition1 is false and condition2 is true
else
	# none of above condition are true
fi
```

example1: check your current UserID and would not allow you to run the script as the root user
```bash
#!/bin/bash
if (( $EUID == 0 )); then
 echo "Please do not run as root"
 exit
fi

```

## case
```bash
case $some_variable in
 pattern_1)		# ) is important at the end
 commands
 ;;				# ;; to end the block
 pattern_2| pattern_3)		# | to separate multiple cases
 commands
 ;;
 *)				# default case start
 default commands
 ;;
esac

```

# Loops
## for
```bash
for element in ${list}
# ex for element in 1 2 3 4 5
do
	echo $element
done

# index loop
for (( assignment; condition; iteration )); do
	# code block
done

for ((i=0; i<5; i++)); do
	echo $i
done
```

## while
```bash
while [ condition ]; do
	# code block
done

i=0
while [[ i -lt 100 ]]; do
	echo "$i"
	i=$((i+1))
done
```

example1:
```bash
#!/bin/bash
counter=1
while [[ $counter -le 10 ]]
do
 echo $counter
 ((counter++))
done
```

## until
The difference between `until` and `while` loops is that the `until` loop will run the commands within the loop until the condition becomes `true`
```bash
until [[ your_condition ]]
do
 your_commands
done
```

## `continue` and `break`
syntax: `continue [n]` and `break[n]`
> here the [n] argument is optional and can be greater than or equal to 1. When [n] is given, the n-th enclosing loop is affected.

> For two nested loop, if continue 2 is used then instead of resuming inner loop it will go to outer loop

```bash
until [[ condition until true ]]
do
	while [[ condition ]]
	do
		# do something
		continue 2
	done
done
```

# Function
```bash
function function_name() {
 your_commands
}
# OR
function_name() {
 your_commands
}

```
- `function` keyword is optional
- You should not add the parenthesis when you call the function

Example1:
```bash
#!/bin/bash

#######################################
# Description: Hello function
# Globals:
# None
# Arguments:
# Single input argument
# Outputs:
# Value of input argument
# Returns:
# 0 if successful, non-zero on error.
#######################################
function hello() {
 echo "Hello $1!"
}
hello DevDojo
```


------------
# Scripting
## Set command
Use to change shell properties and behaviour
- `-e`: errexit : fast-failing : Exit immediately if a commands fail with non zero status
- `-u`: nounset : Treat unset variable as an error and exit
- `-o <option>` : enable option   `+o <option>` : disable option
  - `pipefail`: return the exit status of last failed command
  - `allexport`: Automatically export all variables as soon as they are defined

```bash
set -euo pipefail;

# OR
set -o allexport
source .env
set +o allexport
```