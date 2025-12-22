# SPKG
This is the smallest and most minimal version of SPKG

## How it works
It's a very basic bash script that uses `curl` to walk an HTTP(S) repository and download the file.
Below are the two curl calls actually used, as written in the script. Everything else is fluff to make it work.
```sh
curl -fsSL "$REPO" \
    | grep -oE "$name[^\"]*\.tar\.[^\" ]+" \
    | head -n 1

curl -fsSL "$REPO/$path" -o "$PKG_DIR/$path"
```
