# Download Subreddit Data

install `gnu-parallel`

`sudo apt-get install parallel`

Use `*.txt` files as follows:

# Function to get metadata from each json

get_urls_xz() {   
    curl https://files.pushshift.io/reddit/submissions/$1 | unxz |  parallel --pipe -q jq -rc '{"url": .url, "subreddit":.subreddit, "created_utc": .created_utc}' | pigz > $2;
}

# Export as function

export -f get_urls_xz

cat xz_files.txt | parallel --ungroup get_urls_xz {} {}.gz