# schema.org
[schema.org](http://schema.org) provides well structured data model for linked data. They also tried to provide the model in RDF format [here](http://schema.rdfs.org), but now it's no longer maintained. Instead, TopQuadrant seems to take over the role.

TopQuadrant proposed some modifications in perspective of conventions which will be suited for many RDF/OWL tools([link](http://topbraid.org/schema/)). But there is one remaining problem with [inverseOf](http://meta.0.3-2f.schemaorgae.appspot.com/inverseOf) property which seems to have duplicated meaning with [owl:inverseOf](http://meta.0.3-2f.schemaorgae.appspot.com/inverseOf). This may cause confusion when the reasoning is needed following 'inverseOf' relation.

One more thing may cause confusion is about [supersededBy](http://meta.0.3-2f.schemaorgae.appspot.com/supersededBy) property which indicates some of the properties are not recommended to use. 

Following these considerations, I revised the model again based on TopQuadrant proposed, as follows: 
 * replace 'schema:inverseOf' to 'owl:inverseOf'
 * remove all the properties which includes 'supersededBy' restriction.
 * remove two annotation properties: 'schema:inverseOf' and 'schema:supersededBy'
  
Additionally, errors regarding of the declarations of some of the properties as follows: 

 * Illegal redeclarations of some of the properties are fixed (declared as ObjectProperty and DataProperty): `schema:commentTime`, `schema:seasonNumber`, `schema:startDate`, `schema:pageStart`, `schema:position`, `schema:query`, `schema:dateModified`, `schema:photo`, `schema:estimatedFlightDuration`, `schema:episodeNumber`, `schema:artEdition`, `schema:endDate`, `schema:mainEntityOfPage`, `schema:clipNumber`, `schema:dateCreated`, `schema:volumeNumber`, `schema:pageEnd`, `schema:issueNumber`, `schema:petsAllowed`
  
  ```XML
  <!-- before -->
  <owl:ObjectProperty rdf:about="http://schema.org/commentTime">
    <rdfs:domain rdf:resource="http://schema.org/UserComments"/>
    <rdfs:range>
        <owl:Class>
            <owl:unionOf rdf:parseType="Collection">
                <rdf:Description rdf:about="http://www.w3.org/2001/XMLSchema#date"/>
                <rdf:Description rdf:about="http://www.w3.org/2001/XMLSchema#dateTime"/>
            </owl:unionOf>
        </owl:Class>
    </rdfs:range>
  </owl:ObjectProperty>
  
  <owl:DatatypeProperty rdf:about="http://schema.org/commentTime"/>
  
  <!-- after -->
  <owl:DatatypeProperty rdf:about="http://schema.org/commentTime">
    <rdfs:domain rdf:resource="http://schema.org/UserComments"/>
    <rdfs:range>
        <rdfs:Datatype>
            <owl:unionOf rdf:parseType="Collection">
                <rdf:Description rdf:about="http://www.w3.org/2001/XMLSchema#date"/>
                <rdf:Description rdf:about="http://www.w3.org/2001/XMLSchema#dateTime"/>
            </owl:unionOf>
        </rdfs:Datatype>
    </rdfs:range>
  </owl:DatatypeProperty>
  ```

 * schema:loanTerm is modified to follow its superProperty, `schema:duration`
  
  ```XML
  <!-- before -->
  <owl:ObjectProperty rdf:about="http://schema.org/loanTerm">
    <rdfs:domain rdf:resource="http://schema.org/LoanOrCredit"/>
    <rdfs:range rdf:resource="http://schema.org/QuantitativeValue"/>
  </owl:ObjectProperty>

  <owl:DatatypeProperty rdf:about="http://schema.org/loanTerm">
    <rdfs:subPropertyOf rdf:resource="http://schema.org/duration"/>
  </owl:DatatypeProperty>
  
  <!-- after -->
  <owl:DatatypeProperty rdf:about="http://schema.org/loanTerm">
    <rdfs:domain rdf:resource="http://schema.org/LoanOrCredit"/>
    <rdfs:range rdf:resource="http://www.w3.org/2001/XMLSchema#duration"/>
    <rdfs:subPropertyOf rdf:resource="http://schema.org/duration"/>
  </owl:DatatypeProperty>
  ```
 
 * Misuses of `schema:sameAs` are fixed
 ```XML
 <!-- before -->
 <owl:DatatypeProperty rdf:about="http://schema.org/sameAs">
    <rdfs:domain rdf:resource="http://www.w3.org/2002/07/owl#Thing"/>
    <rdfs:range rdf:resource="http://www.w3.org/2001/XMLSchema#anyURI"/>
 </owl:DatatypeProperty>
 
 <!-- after -->
 <owl:AnnotationProperty rdf:about="http://schema.org/sameAs">
    <rdfs:domain rdf:resource="http://www.w3.org/2002/07/owl#Thing"/>
    <rdfs:range rdf:resource="http://www.w3.org/2001/XMLSchema#anyURI"/>
 </owl:AnnotationProperty>
 ```
 

# License
Licensed under the terms of service documented [here](http://schema.org/docs/terms.html), which follows [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/).
