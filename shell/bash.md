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

**`ln`**
```shell
ln -s <real_file> <future_link>
```

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

## see also

[http://tldp.org/LDP/abs/html/index.html](http://tldp.org/LDP/abs/html/index.html)
[https://stackoverflow.com/documentation/bash/topics](https://stackoverflow.com/documentation/bash/topics)
[https://dev.to/thiht/shell-scripts-matter](https://dev.to/thiht/shell-scripts-matter)
