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
