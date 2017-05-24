# schema.org
[schema.org](http://schema.org) provides well structured data model for linked data. They also tried to provide the model in RDF format [here](http://schema.rdfs.org), but now it's no longer maintained. Instead, TopQuadrant seems to take over the role.

TopQuadrant proposed some modifications in perspective of conventions which will be suited for many RDF/OWL tools([link](http://topbraid.org/schema/)). But there is one remaining problem with [inverseOf](http://meta.0.3-2f.schemaorgae.appspot.com/inverseOf) property which seems to have duplicated meaning with [owl:inverseOf](http://meta.0.3-2f.schemaorgae.appspot.com/inverseOf). This may cause confusion when the reasoning is needed following 'inverseOf' relation.

One more thing may cause confusion is about [supersededBy](http://meta.0.3-2f.schemaorgae.appspot.com/supersededBy) property which indicates some of the properties are not recommended to use. 

Following these considerations, I revised the model again based on TopQuadrant proposed, as follows: 
  * replace 'schema:inverseOf' to 'owl:inverseOf'
  * remove all the properties which includes 'supersededBy' restriction.
  * remove two annotation properties: 'schema:inverseOf' and 'schema:supersededBy'

# License
Licensed under the terms of service documented [here](http://schema.org/docs/terms.html), which follows [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/).
