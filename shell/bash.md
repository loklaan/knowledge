# bash

## script template

```shell
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'

#/ Usage:
#/ Description:
#/ Examples:
#/ Options:
#/   --help: Display this help message
usage() { grep '^#/' "$0" | cut -c4- ; exit 0 ; }
expr "$*" : ".*--help" > /dev/null && usage

readonly LOG_FILE="/tmp/$(basename "$0").log"
info()    { echo "[INFO]    $@" | tee -a "$LOG_FILE" >&2 ; }
warning() { echo "[WARNING] $@" | tee -a "$LOG_FILE" >&2 ; }
error()   { echo "[ERROR]   $@" | tee -a "$LOG_FILE" >&2 ; }
fatal()   { echo "[FATAL]   $@" | tee -a "$LOG_FILE" >&2 ; exit 1 ; }

cleanup() {
  # Remove temporary files
  # Restart services
  # ...
}

if [[ "${BASH_SOURCE[0]}" = "$0" ]]; then
  trap cleanup EXIT
  # Script goes here
  # ...
fi
```

## builtins

**`for`**
```shell
for i in ( ls some-directory ); do
  echo $i
done
```

**`case`**
```shell
case "$variable" in
  abc)  echo "\$variable = abc" ;;
  xyz)  echo "\$variable = xyz" ;;
esac
```

**`ln`**
```shell
ln -s <real_file> <future_link>
```

**`select`**
```shell
select result in Yes No Cancel
do
  echo $result
done
```

**`tput`**
```shell
tput cuu1 # Move cursor up by one line
tput el   # Clear the line
```

## tips

Ternary expressions.

```shell
<expression> && <on-true expression> || <on-false expression>
# See below example for file/stdin to variable.
```

Assign either a file or stdin (piped input) to a variable, with fallbacks.

```shell
# Use a filepath from args when available, or use stdin
[ $# -ge 1 -a -f "$1" ] && input="$1" || input="-"
content=$(cat $input)

# Use stdin if pipe is occupied, otherwise use a file
(test -s /dev/stdin) && input="-" || input="./config.json"
content=(cat $input)
```

## see also

http://tldp.org/LDP/abs/html/index.html  
https://stackoverflow.com/documentation/bash/topics  
https://dev.to/thiht/shell-scripts-matter  
