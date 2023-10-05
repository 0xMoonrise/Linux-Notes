  
This script processes text data from a file, extracts specific network-related information, and sorts it based on numerical values in the data.
```bash
#!/usr/bin/env bash
#made by jsnunez 04/08/2022
strings=$(grep -P "interface fastethernet|switchport trunk native vlan" "$1")
strings=$(tr '\n' '\t' <<< "$strings" | sed 's/interface/\n&/2g' | grep -vP "/24|/23")

strings="${strings//interface fastethernet/switch }"
strings="${strings//switchport trunk native/}"

if [[ -n $2 ]]; then
    grep "switch $2" <<< "$strings" | sort -k 4 -n
else
    sort -k 4 -n <<< "$strings"
fi
```
------
This script allows you to upload or download files to/from a remote server using SFTP. You can specify the IP address and file path on the server as options. It also provides a help panel for usage instructions.
```bash
#!/bin/bash

FILE_PATH="/path/to/file"
IP="1.1.1.1"
MODE=""
USERNAME=""

function download_file()
{
	echo "Downloading:$1.bak"
	curl -k "sftp://$IP/$FILE_PATH/$1" --user "$USERNAME:$password" -o "./$1.bak"
}

function upload_file()
{
        echo "Uploading:$1"
	curl -k "sftp://$IP/$FILE_PATH/" --user "$USERNAME$:$password" -T "./$1"
	rm "./$1"
}

function help()
{
	echo -e "Welcome to help.
default IP:
    $IP
default PATH:
    $FILE_PATH
default usage:
    upload_files.sh file1 file2 ... fileN
options:
    -i $IP set the IP server to upload and download files
    -p /path/from/server/	set the PATH on the server to upload and download files
    -o upload | download	set mode to only mode
    -h get the help panel"
}
while getopts ":i:p:o:h" opt; do
	case ${opt} in
		i )
			IP="${OPTARG}"
			;;
		p )
			FILE_PATH="${OPTARG}"
			;;
		o )
			MODE="${OPTARG}"
			if [[ "$MODE" =~ ^(upload|download)$ ]]; then
				echo "Mode: only $MODE activated."
			else
				echo "Mode-Error: $MODE is not allowed, aborting."
				exit 1
			fi
			;;
		h )
			help
			exit 0
			;;
		\? )
			echo "Invalid option: $OPTARG" 1>&2
			echo "Use: upload_files.sh -h to get help."
			exit 1
			;;
		: )
			echo "Invalid option: $OPTARG requires an argument" 1>&2
			echo "Use: upload_files.sh -h to get help."
			exit 1
			;;
	esac
done

echo -n "Password:"
read -s password
echo

shift $((OPTIND -1))

for file in "$@"; do

	if [ ! -f $file ]; then echo "File: $file not found!"; continue; fi
	if ! [[ -w $file && -r $file ]]; then chmod +rw $file; fi
	
	if [[ $MODE == "download" || $MODE == "" ]]; then download_file $file; fi
	if [[ $MODE == "upload" || $MODE == "" ]]; then upload_file $file; fi

done
```