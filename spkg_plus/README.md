# spkg
This is the advanced version that will pull urls from a list of mirrors

# Usage
`./spkg <package_name>`

## How it works
This is a more advanced downloader, still not an installer or manager, with advanced 
### Repo List
```sh
while IFS= read -r line; do         # Read file and separate each linebreak and add to line variable
    if [[ "$line" == \#* ]]; then   # Ignore lines that start with # (comments)
        :
    else
        REPOS+=("$line")            # Add every other line to the array
    fi
done < "$REPO_LIST"                 # The file list of repositories
```
### Seach
Near identical to the minimal downloader, wrapped in a for loop and adds the full confirmed valid path to a list for the download function.
```sh
for repo in "${REPOS[@]}"; do
    path=$(curl -fsSL "$repo" --connect-timeout "$TIME_OUT" \
        | grep -oE "$pkg[^\"]*\.tar\.[^\" ]+" \
        | head -n 1)

    if [ -n "$path" ]; then
        SUCCESS_URLS+=("$repo$path")
        echo -e "${SUCCESS_URLS[@]} ... [FOUND]"
    else
        echo -e "${repo}${path} ... [NOT FOUND!]"
    fi
 done
 ```
### Download
```sh
for pkg_url in "${SUCCESS_URLS[@]}"; do
    echo -e "Downloading: $name\n  From: $pkg_url"

    if curl -fsSL "$pkg_url" \
        -o "$PKG_DIR/$name" \
        --max-time "$MAX_TIMEOUT"; then
            
        success "$name"
        return 0
    else
        local exit_code=$?
        echo -e "Download failed with exit code $exit_code"

        # Clean up failed downloads
        clean_up "$name"
        ((idx++))

        if [ "$idx" -eq "$((${#SUCCESS_URLS} - 1))" ]; then
            failure "$name"
        fi
    fi
done
```
