
## Set up required
* Create an environment variable for your API -> $ECE_API_KEY
* Create an environment variable for your API end point -> $COORDINATOR_HOST

## Index Operations

Create an Index
>curl -k -XPUT -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/songs/?pretty" -H "Content-Type: application/x-ndjson" -d '
{
  "mappings": {
    "properties": {
      "Year": {"type": "date"}
    }
  }
}
'

Return all of the indices
>curl -k -XGET -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/_cat/indices?v=true&pretty" -H "kbn-xsrf: reporting"

## Insert data into an index

Create a document in the movies index
>curl -k -XPUT -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/movies/_doc/109499" -H "Content-Type: application/x-ndjson" -d '
{
  "genre": ["Pop", "Rom-Com"],
  "title": "Yesterday",
  "year": 2019
}
'

## Perform query for data in an existing index 

Querying Elastic Cloud to return the documents in the movies index
> curl -k -XGET -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/movies/_search?pretty" -H "kbn-xsrf: reporting"

Return a document if know the index and the document ID
> curl -k -XGET -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/movies/_doc/109487?pretty" -H "kbn-xsrf: reporting"

Perform a partial text query (e.g. you know the movie is 'inter')
>curl -k -XGET -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/movies/_search?q=Star&pretty" -H "kbn-xsrf: reporting"


## Perform a bulk update of data into an existing index 

Use the Bulk API command to upload multiple documents into the movies index:
> curl -k -XPUT -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/_bulk?pretty" -H "Content-Type: application/x-ndjson" --data-binary @movies.json


## Perform an updates 
Update only field
>curl -k -XPOST -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/movies/_update/109487?pretty" -H "Content-Type: application/x-ndjson" -d '
{
    "doc":{
        "title": "Interstellar"
    }
}
'

Overwrite the entire contents of the a document
>curl -k -XPUT -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/movies/_doc/109487?pretty" -H "Content-Type: application/x-ndjson" -d '
{
    "genre": ["IMAX", "Sci-Fi"],
    "title": "Interstellar",
    "year": 2014
}
'

## Perform deletes

Delete a document when you know the index and document ID
> curl -k -XDELETE -H "Authorization: ApiKey $ECE_API_KEY" "https://$COORDINATOR_HOST/movies/_doc/58559?pretty"


