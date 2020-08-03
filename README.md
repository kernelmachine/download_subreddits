# Download Subreddit Data

install `gnu-parallel`

`sudo apt-get install parallel`

Use `*.txt` files as follows:

# Function to get metadata from each json

```
get_urls_xz() {   
    curl https://files.pushshift.io/reddit/submissions/$1 | unxz |  parallel --pipe -q jq -rc '{"url": .url, "subreddit":.subreddit, "created_utc": .created_utc}' | pigz > $2;
}
export -f get_urls_xz
cat xz_files.txt | parallel --ungroup get_urls_xz {} {}.gz
```

To get other fields of subreddit data, just run 

```
curl -s https://files.pushshift.io/reddit/submissions/./RS_2017-07.xz | unxz | head -n 1
```

to check out the available fields, and change the output json in the `jq` part of the command above.