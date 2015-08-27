# **Overview**
The API for managing synonyms works with a list of words, it uses a map, where the value for each entry in the map is a set of synonyms for a term.

Add the following field type definition in schema.xml
>     <fieldType name="managed_en" positionIncrementGap="100">
>     <analyzer>    
>     <tokenizer class="solr.StandardTokenizerFactory"/>    
>     <filter class="solr.ManagedStopFilterFactory"   managed="english" />     
>     <filter class="solr.ManagedSynonymFilterFactory"  managed="english" />   
>     </analyzer>
>     </fieldType>

To get the map of managed synonyms, send a GET request to:
>     curl "http://localhost:8983/solr/mycollection/schema/analysis/synonyms/english"

This request will return a response:
>     { 
>     "responseHeader":{  
>     "status":0,    "QTime":3},
>     "synonymMappings":{
>     "initArgs":{  
>     "ignoreCase":true,
>     "format":"solr"}, 
>     "initializedOn":"2014-12-16T22:44:05.33Z",  
>     "managedMap":{  
>     "GB":
>     ["GiB",         "Gigabyte"],
>     "TV":   
>     ["Television"],
>     "happy":        ["glad",         "joyful"]}}}

Managed synonyms are returned under the managedMap property which contains a JSON Map where the value of each entry is a set of synonyms for a term.

To add a new synonym mapping, you can PUT/POST a single mapping such as
>     curl -X PUT -H 'Content-type:application/json' --data-binary '{"mad":["angry","upset"]}' "http://localhost:8983/solr/mycollection/schema/analysis/synonyms/english"

The API will return status code 200 if the PUT request was successful. 

To determine the synonyms for a specific term, you send a GET request for the child resource, 
>     curl -X GET -H 'Content-type:application/json' --data-binary "http://localhost:8983/solr/mycollection/schema/analysis/synonyms/english/mad"

The response would be
>    {  
>    "responseHeader":{
>    "status":0,    "QTime":3},
>    "synonymMappings":{ 
>    "initArgs":{
>    "ignoreCase":true,      
>    "format":"solr"},
>    "initializedOn":"2014-12-16T22:44:05.33Z",
>    "managedMap":{
>    "mad":
>    ["angry", "upset"]}}}

Lastly, you can delete a mapping by sending a DELETE request to the managed endpoint.
>    curl -X DELETE "http://localhost:8983/solr/mycollection/schema/analysis/stopwords/mad"
