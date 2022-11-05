# flashbots-relay-data

Quick explorations of data from Flashbots Boost Relay.

### Links

- [Flashbots Boost Relay - Data](https://flashbots-boost-relay-public.s3.us-east-2.amazonaws.com/index.html)
- [Flashbots Boost Relay - Mainnet](https://boost-relay.flashbots.net)

### Notebook

- [data.ipynb](./data.ipynb)

### Getting block submission data locally

Easiest thing for me was using the `awscli`

```
$ mkdir data && cd data
$ aws s3 cp s3://flashbots-boost-relay-public . --recursive --no-sign-request --exclude "*" --include "*.json.gz"
```

The above `cp` command narrows down the files to json compressed zips, but you can omit that or change to get the zipped csv files instead.

Unzipping everything 

```
for file in data/data/2_builder-submissions/*; do 
    if [ -f "$file" ]; then 
        gzip -d "$file"
    fi 
done
```

### Uploading data to a DB

TBD but I ran a few scripts to load up a fraction of it into a MongoDB cluster and for that `mongoimport` is best since it can open parallel connections and uploads a 300MB JSON in ~5min.

```
mongoimport --uri <CONNECTION_STRING> --collection='submissions' --file='builder-submissions_slot-4629154-to-4650000.json' --jsonArray
```
