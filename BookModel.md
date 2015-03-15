The basis for the 'book' resource should probably be built on [the Bibo Book class](http://bibotools.googlecode.com/svn/bibo-ontology/trunk/doc/classes/Book.html).

I'm going to also recommend liberal use of literals at this stage to simplify the indexing process.  If there are huge objections to this, it can be abandoned.  For these, I'll use Dublin Core Elements (dc) for literals and Dublin Core Terms (dcterms) for resources.

There will also be a sort of 'catch-all' Chief vocabulary that we can build as we find gaps in the current array of vocabularies.

We might also consider a 'record meta' type vocabulary to capture information about graph describing the resource.
```
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/TR/rdf-schema/> .
@prefix dc: <http://purl.org/dc/elements/1.1/> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix bibo: <http://purl.org/ontology/bibo/> .
@prefix chief: <http://kochief.org/vocab/> .
@prefix frbr: <http://purl.org/vocab/frbr/core#> .
<http://example.kochief.org/books/123456>
  rdf:type bibo:Book;
  rdf:type frbr:Manifestation;
  dc:title "The tragedy of Romeo and Juliet";
  dcterms:alternate "Romeo and Juliet";
  dcterms:creator <http://example.kochief.org/identities/Shakespeare,-William>;
  dc:creator "William Shakespeare";
  dcterms:publisher <http://example.kochief.org/identities/Yale-University-Press>;
  bibo:editor <http://example.kochief.org/identities/Hosley,-Richard>;
  dcterms:issued "1958";
  bibo:oclcnum "252782";
  dcterms:isPartOf <http://example.kochief.org/series/The-Yale-Shakespeare>;
  dc:language "en";
  dcterms:subject <http://id.loc.gov/authorities/sh2008111035#concept>;
  dc:subject "Romeo (Fictitious character)--Drama";
  dcterms:subject <http://id.loc.gov/authorities/sh2001009136#concept>;
  dc:subject "Tragedies";
  dcterms:isVersionOf <http://example.kochief.org/expressions/Romeo-and-Juliet>;
  dcterms:extent "174 p.";
  dc:identifier "info:oclcnum/252782";
  rdfs:seeAlso <http://example.kochief.org/record/123456> .
```