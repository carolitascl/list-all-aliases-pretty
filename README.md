## List all aliases pretty

This zsh/bash script runs on macOS Tahoe and shows your aliases in a cleaner, more readable format than the alias command

<br>
<img width="359" height="232" alt="image" src="https://github.com/user-attachments/assets/a6480588-b5c0-4c81-a54b-0d1ee3b62bbe" />
<br>

### How to use it

#### In zsh
> 1. Open the terminal
> 2. Copy the script once at the end of the ``` ~/.zshrc ``` file
> 3. Run the following command once: ``` source ~/.zshrc ```
> 4. Run `alias-alist`

#### In bash
> 1. Open the terminal
> 2. Copy the script once at the end of the ``` ~/.bashrc ``` file
> 3. Run the following command once: ``` source ~/.bashrc ```
> 4. Run `alias-alist`

<br>

```bash
alias-list() {
    {
        printf "\e[1;36m=== List Of All Aliases ===\e[0m\n"
        
        local term_width=${COLUMNS:-100}
        local wrap_width=$((term_width - 30))
        
        alias | sort | while IFS='=' read -r name value; do
            printf "\e[0;32m%-25s\e[0m \e[1;90m=>\e[0m " "$name"
            
            if [[ "$value" == *'\\n'* ]] || [[ "$value" == *'function '* ]]; then
                printf "\e[0;35m[function]\e[0m\n"
                echo "$value" | sed "s/^\\$'//" | sed "s/'$//" | sed 's/\\n/\n/g' |
                sed 's/^/    /' | sed 's/\\t/    /g'
                echo ""
            else
                local clean_value=$(echo "$value" | sed "s/^'//" | sed "s/'$//")
                
                printf "\e[0;33m%s\e[0m\n" "$clean_value" |
                fold -w "$wrap_width" -s |
                awk 'NR==1 {print; next} {print "                           ↳ " $0}'
            fi
        done
    } | less -r
}
```
