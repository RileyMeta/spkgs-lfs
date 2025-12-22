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
```sh
... # todo: fill this out :3
```
### Download
```sh
... # todo: fill this out :3
```
