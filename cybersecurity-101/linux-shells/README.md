# TryHackMe: Cybersecurity 101/Linux Shells Write-up

# Problem:
Was given a broken batch script on an Ubuntu machine that searches for a flag (thm-flag01-script) in .log files inside a specific directory (/var/log). 
The script had empty variables and wouldn't function until it was fixed. 

# What I did to fix it:

* The user I connected to the machine on didn't have permissions to search system directories, so I used sudo su to become root user for full access.
* I edited the broken script located at /home/user/flag_hunt.sh

# Previous Script with errors:
```
#!/bin/bash

directory=" "

flag=" "

echo "Flag search in directory: $directory in progress..."

for file in " "/*.log; do

    if grep -q "$flag" "$file"; then

        echo "Flag found in: $(basename "$file")"

    fi

done
```

# Commands and Fixes:

| Command / Action                                                  | What it does                         | Why it was needed                                    |
| ----------------------------------------------------------------- | ------------------------------------ | ---------------------------------------------------- |
| `sudo su`                                                         | Switches to root user                | Needed root permissions to search system directories |
| Edited `directory=" "` → `directory="/var/log"`                   | Defined the directory to search logs | Fixed empty variable with no search path             |
| Edited `flag=" "` → `flag="thm-flag01-script"`                    | Defined the correct flag keyword     | Allowed grep to search for correct the term          |
| Edited `for file in " "/*.log` → `for file in "$directory"/*.log` | Fixed undefined file path            | Made the loop search in the intended directory       |


# Updated Script:
```
#!/bin/bash

directory="/var/log"

flag="thm-flag01-script"

echo "Flag search in directory: $directory in progress..."

for file in "$directory"/*.log; do

if grep -q "$flag" "$file"; then
    
        echo "Flag found in: $(basename "$file")"
    fi
done
```
# Flag Found:
```
Flag found in authentication.log
```



# Key Takeaways:
* Check for empty variables in Bash scripts when they fail.

* Directory paths in loops must be correctly referenced with variables.

* sudo su is necessary for tasks that need root level access

