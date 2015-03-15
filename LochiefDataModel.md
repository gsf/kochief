# Introduction #

While ideally you can store any assertion in lochief, the reality is that the UI will need to have some expectation of what it might be getting in order to not have a total interface nightmare for the user.

There are basically two approaches to this:
  1. A generic "Document" format -- whether something like Dublin Core or proprietary
    * This is, of course, basically how current OPACs and OPAC replacements currently work
    * Talis Prism also uses this method, currently, having cooked up a schema called "BibRDF"  [see an example](http://skywalk.talis.com/bib-demo-2/catalog/items/175)
  1. A collection of domain-specific vocabularies that lochief recognizes out of the box, with a simple way to extend these and add new ones.

For this project, I propose we focus on the latter.  The former seems very "weak tea" (although, arguably, easier) and wouldn't do much to:
  * Enhance the user experience/expectation
  * Leverage the advantages RDF gives us
  * Do anything to set Kochief apart in a crowd of OPAC replacement projects

For now, this will only focus an OPAC replacement scenario, so the source data would be MARC bibliographic records and any enrichment data that seems valid (cover images, reviews, maps, ERMS/Link Resolver data -- possibly).  Preferably there will be nothing lochief-specific here.

# Background #

For a completely unrelated project, I had created a [branch of the ruby-marc parser](http://marc.rubyforge.org/svn/branches/typed_records/) that attempted to define distinct attributes of a MARC record and provide different interfaces depending on the kind of record it was (BKS vs. MAP vs. SCO, for example).  It is incomplete (it doesn't begin to address the data in the 007 field, for example), but can give a more detailed overview of what the record is actually describing.

In an effort to see what kind of resources would actually be found in library catalogs, I took a handful of MARC record dumps of libraries found in the Internet Archive and ran them in a little script to get an idea of what they contained.

The libraries I used were:
  * Western Washington University
  * Georgia Tech (this was not from the IA, but from a stash of records I had when I worked there)
  * Laurentian University
  * Ithaca College
  * The California College of the Arts

More data points would be greatly welcome, however the interface at IA for downloading large sets of MARC records (such as Boston Public Library or Miami University) is unwieldy, which is why they were passed over.  Also, I left a few things out (tests for festschrifts and archival records, for example or gleaning the literary form), but I don't think there was anything that would dramatically change the model.

A very "basic" overview of the major record types looked like this:

| |BKS|SER|MAP|COM|MIX|REC|SCO|VIS|unknown|
|:|:--|:--|:--|:--|:--|:--|:--|:--|:------|
|WWU|1019652|47758|18298|1616|339|34809|30124|9915|429|
|GT|639239|51047|19142|2084|157|69|17|1641|11604|
|LU|697540|76718|71|3908|6 |5946|2422|4126|16263|
|IC|224511|1659|62|13|54|35|18456|189|21|
|CCA|49778|990|22|41|2 |34|7 |2113|13|

This overview doesn't tell much, however, so here is a much more detailed breakout.  Note there can be multiple "attributes" assigned to each record type (for instance, it could be a conference AND a govdoc), so the totals could be far greater than the actual record count.

## Books ##
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Manuscripts |  857 | 5231 | 7 | 850 | 3 |
| Govdocs |  204960 | 172776 | 511896 | 20909 | 2006 |
| Conferences |  32417 | 60720 | 4770 | 5027 | 258 |

### Nature of work ###
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Filmography | 403 | 141 | 88| 427 | 250 |
| Index |  1781 | 668 | 202 | 558 | 63 |
| Encyclopedia | 1627 | 808 | 470 | 614 | 115 |
| Thesis |  200 | 73 | 1181 | 17 | 1 |
| Handbook |  5242 | 5323 | 881 | 844 | 138 |
| Bibliography | 457014 | 328435 | 71859 | 142659 | 25453 |
| Catalog | 8052 | 2708 | 655 | 2775 | 7610 |
| nil | 33 | 181 | 302828 | 3 | 52 |
| Dictionary |  7669 | 3127 | 1061 | 1385 | 407 |
| Technical report | 1828 | 2260 | 803 | 3 | 1 |
| Programmed text | 218 | 89 | 13 | 36 | 1 |
| Legislation | 488 | 1958 | 144 | 25 | 1 |
| Legal case | 18 | 16 | 26 | 11 |  |
| Standard/specification | 4 | 3 | 7296 |  |  |
| Review | 40 | 15 | 14 | 24 | 5 |
| Abstracts | 580 | 389 | 89 | 72 | 18 |
| Directory | 1754 | 1210 | 127 | 203 | 64 |
| Law report | 23 | 20 | 32 | 6 |  |
| Statistics | 15358 | 8303 | 2072 | 1684 | 13 |
| Patent document | 1 | 1 |  | 1 |  |
| Biography | 13 | 215 | 1 | 4 |  |
| Legal article | 25 | 16 | 62 | 7 |  |
| Treaty | 152 | 34 | 99 | 1 |  |
| Literature survey | 56 | 35 | 17 | 15 | 5 |
| Discography | 703 | 47 | 70 | 706 | 32 |
| Other report |  | 2 | 4 |  |  |

## Serials ##
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Govdocs | 16185 | 16561 | 13014 | 208 | 18 |
| Conferences | 1671 | 1886 | 184 | 47 | 9 |

### Type of resource ###
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Monographic Series | 1946 | 316 | 185 | 159 | 11 |
| Database | 4 |  | 11 |  |  |
| Other | 21998 | 19856 | 22081 | 1227 | 453 |
| Newspaper | 1095 | 23 | 153 |  | 2 |
| Website | 3 | 1 | 3 | 1 |  |
| Periodical | 22708 | 22115 | 52845 | 275 | 558 |

### Nature of Work ###
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Encyclopedia | 2 | 1 | 1 |  |  |
| Index | 166 | 137 | 19 | 13 | 14 |
| Handbook | 55 | 62 | 11 | 2 |  |
| Bibliography | 1762 | 1134 | 811 | 59 | 15 |
| Catalog | 177 | 432 | 11 | 7 | 13 |
| nil | 68 | 68 | 1068 | 14 |  |
| Dictionary | 17 | 17 | 9 | 2 |  |
| Programmed Text | 2 | 3 |  |  |  |
| Technical Report | 2 | 6 | 3 | 1 |  |
| Legal case | 45 | 15 | 20 |  |  |
| Legislation | 132 |  | 22 | 4 |  |
| Directory | 284 | 335 | 32 | 2 | 15 |
| Abstracts | 214 | 513 | 73 | 5 | 12 |
| Review | 1122 | 439 | 517 | 11 | 20 |
| Law Report | 76 | 41 | 42 |  |  |
| Statistics | 2229 | 2162 | 535 | 92 | 3 |
| Biography | 150 | 47 | 21 | 15 | 8 |
| Legal article | 139 | 63 | 155 | 2 | 4 |
| Literature survey | 6 | 15 | 11 |  | 1 |
| Discography | 19 |  |  |  |  |
| Standard/specification |  | 3 | 3 |  |  |
| Thesis |  |  | 2 |  |  |
| Other report |  |  | 1 |  |  |
| Patent document |  |  | 5 |  |  |

## Maps ##
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Govdocs | 17850 | 5834 | 16 | 2 | 1 |

### Type ###
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Serial | 535 | 79 |  | 1 |  |
| Atlas | 236 | 176 | 30 | 19 | 22 |
| Other | 1 | 1 |  |  |  |
| Series | 1618 | 340 | 1 |  |  |
| Globe | 2 | 7 |  |  |  |
| Map | 15767 | 8943 | 11| | 1 |  |
| Unknown |  |  | 13 |  |  |

## Computer ##
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Govdocs | 1339 | 1672 | 346 |  | 1 |

### Type ###
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Numeric Data | 1 | 2 |  |  |  |
| Online | 1 | 4 |  |  |  |
| Combination | 18 | 18 |  |  |  |
| Representational | 1 | 1 |  |  |  |
| Computer program | 2 | 9 | 4 |  |  |
| Interactive multimedia | 3 | 3 |  |  |  |
| Document | 14 | 24 | 235 | 1 |  |
| Unknown | 43 | 65 | 6 | 4 | 3 |
| Bibliographic data |  | 1 |  |  |  |
| Other |  | 10 |  |  |  |

## Recording ##
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Conferences | 41 |  |  |  |  |

### Type ###
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Motion picture music | 60 |  | 1 | 1 |  |
| Catata | 428 |  | 3 |  |  |
| Motet | 139 |  | 5 |  |  |
| Part-song | 102 |  |  |  |  |
| Country music | 18 |  |  |  |  |
| Toccata | 14 |  | 1 |  |  |
| Musical revue/comedy | 213 |  |  |  |  |
| Christian chant | 86 |  |  |  |  |
| Prelude | 63 |  | 2 |  |  |
| Rhapsody | 1 |  |  |  |  |
| Symphonic poem | 185 |  |  |  |  |
| Fantasia | 57 | 1 |  |  |  |
| Sonata | 1201 |  | 9 |  |  |
| Rock music | 42 |  | 4 |  |  |
| Requiem | 125 |  | 3 |  |  |
| Hymn | 44 |  |  |  |  |
| Folk music | 673 |  | 8 | 1 |  |
| Polonaises | 20 |  |  |  |  |
| Mass | 290 |  | 4 |  |  |
| Study/exercise | 52 |  |  |  |  |
| Gospel music | 24 |  |  |  |  |
| Nocturne | 28 |  |  |  |  |
| Jazz | 1833 | 1 | 31 |  | 1 |
| Ballet | 281 |  | 1 |  |  |
| Popular music | 322 |  | 6 |  |  |
| Unknown | 495 | 7 | 1130 |  | 3 |
| Fugue | 31 |  |  |  |  |
| Rondo | 9 |  |  | 1 |  |
| March | 94 |  |  |  |  |
| Concerti grossi | 119 |  | 1 |  |  |
| Blues | 52 |  |  |  |  |
| Canzona | 4 |  |  |  |  |
| Madrigal | 63 |  | 2 |  |  |
| Overture | 176 |  | 1 |  |  |
| Bluegrass music | 3 |  |  |  |  |
| Ragtime music | 20 |  |  |  |  |
| Chorale | 33 |  |  |  |  |
| Divertimento/serenade/cassation/divertissement/notturni | 86 |  |  |  |  |
| Multiple forms | 8738 | 1 | 79 | 10 | 2 |
| Oratoria | 280 |  |  |  |  |
| Passion music | 39 |  |  |  |  |
| Ricercars | 1 |  |  |  |  |
| Ballad | 1 |  |  |  |  |
| Song | 2140 | 4 | 22 | 1 |  |
| Variation | 155 |  |  |  |  |
| Other | 3555 | 17 | 27 | 2 | 3 |
| Dance form | 166 |  | 1 |  |  |
| Waltz | 86 |  |  |  |  |
| Chance composition | 34 | 2 |  |  |  |
| Anthem | 37 |  | 1 |  |  |
| Opera | 3038 | 1 | 11 |  |  |
| Pavanes | 2 |  |  |  |  |
| Trio-sonata | 58 |  |  |  |  |
| Passacaglia | 7 |  |  |  |  |
| Not a musical recording | 1458 | 17 | 9 | 10 | 13 |
| Chorale prelude | 12 |  |  |  |  |
| Chaconne | 9 |  |  |  |  |
| Minuet | 2 |  |  |  |  |
| Polyphonic chanson | 8 |  | 3 |  |  |
| Mazurka | 21 |  |  |  |  |
| Canon or round | 29 |  |  |  |  |
| Symphony | 1712 |  | 13 |  |  |
| Program music | 21 |  |  |  |  |
| Carol | 84 |  |  |  |  |
| Chant | 17 |  |  |  |  |
| Concerto | 2168 |  | 22 |  |  |
| Suite | 734 |  | 3 |  |  |
| Sonata |  | 1 |  |  |  |

## Scores ##

### Type ###
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Rhapsody | 3 |  |  | 2 |  |
| Chaconne | 23 |  |  | 10 |  |
| Pavanes | 13 |  |  | 7 |  |
| Minuet | 41 |  |  | 9 |  |
| Madrigal | 149 |  | 2 | 79 |  |
| Popular music | 42 |  | 1 | 139 | 1 |
| Chance composition | 50 |  |  | 44 |  |
| Opera | 1281 | 1 | 14 | 928 |  |
| Waltz | 143 |  |  | 70 |  |
| Polonaises | 15 |  |  | 6 |  |
| Program music | 16 |  |  | 5 |  |
| Concerti grossi | 116 |  | 1 | 67 |  |
| Canzona | 64 |  |  | 20 |  |
| Concerto | 1896 | 1 |  | 894 |  |
| Prelude | 143 |  |  | 97 |  |
| Suite | 1197 |  |  | 720 |  |
| Chorale prelude | 59 |  |  | 63 |  |
| Multiple forms | 1615 | 1 |  | 1122 | 2 |
| Blues | 11 |  |  | 8 |  |
| Ragtime music | 30 |  |  | 15 |  |
| March | 68 |  |  | 64 |  |
| Ballet | 141 |  | 1 | 93 |  |
| Not a musical recording | 24 |  |  | 53 |  |
| Passacaglia | 27 |  |  | 8 |  |
| Symphonic poem | 158 |  |  | 102 |  |
| Fantasia | 215 |  | 1 | 138 |  |
| Part-song | 249 |  |  | 187 |  |
| Jazz | 110 |  | 3 | 32 |  |
| Musical revue/comedy | 215 |  | 1 | 413 |  |
| Divertimento/serenade/cassation/divertissement/notturni | 226 |  |  | 65 |  |
| Trio-sonata | 294 |  |  | 111 |  |
| Symphony | 1043 | 1 | 3 | 667 |  |
| Christian chant | 27 |  |  | 52 |  |
| Motet | 306 |  | 1 | 138 |  |
| Other | 7231 |  | 16 | 3126 |  |
| Gospel music | 1 |  |  | 1 |  |
| Rondo | 72 |  |  | 35 |  |
| Nocturne | 74 |  | 1 | 32 |  |
| Ricercars | 17 |  |  | 16 |  |
| Canon or round | 82 |  |  | 37 |  |
| Passion music | 18 |  |  | 13 |  |
| Catata | 880 |  |  | 418 |  |
| Folk music | 311 | 6 | 4 | 138 | 2 |
| Motion picture music | 10 |  |  | 47 |  |
| Mazurka | 19 |  |  | 8 |  |
| Hymn | 115 |  | 1 | 90 |  |
| Polyphonic chanson | 37 |  |  | 31 |  |
| Chorale | 51 |  | 1 | 30 |  |
| Toccata | 48 |  |  | 29 |  |
| Fugue | 81 |  |  | 40 |  |
| Study/exercise | 1169 |  | 18 | 638 |  |
| Requiem | 95 |  | 1 | 57 |  |
| Variation | 346 |  |  | 217 |  |
| Overture | 351 |  | 1 | 219 |  |
| Country music | 4 |  |  | 4 |  |
| Anthem | 82 | 1 |  | 50 |  |
| Oratorio | 202 |  | 6 | 112 |  |
| Mass | 359 |  |  | 242 |  |
| Rock music | 3 |  | 2 | 14 |  |
| Carol | 67 |  | 1 | 23 |  |
| Unknown | 710 | 2 | 577 | 887 |  |
| Dance form | 324 |  |  | 203 |  |
| Song | 2491 | 3 | 7 | 1885 | 2 |
| Sonata | 1932 |  | 6 | 921 |  |

## Visual ##
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Govdocs | 816 | 269 | 471 | 1 | 10 |

### Type ###
| | WWU | GT | LU | IC | CCA |
|:|:----|:---|:---|:---|:----|
| Flash card | 9 | 2 |  |  | 2 |
| Kit | 77 | 19 | 8 | 7 | 4 |
| Videorecording | 8809 | 1400 | 947 | 91 | 2137 |
| Graphic | 9 | 1 |  |  |  |
| Chart | 64 | 15 |  |  | 1 |
| Picture | 508 | 158 | 1 | 1 | 4 |
| Realia | 232 | 2 |  |  |  |
| Other | 32 | 10 | 1894 |  | 1 |
| Art original | 1 |  |  |  |  |
| Game | 7 |  | 1 |  | 1 |
| Toy | 1 |  |  |  |  |
| Art reproduction | 5 | 1 |  |  | 1 |
| Motion picture | 1 | 3 | 20 | 35 | 6 |
| Filmstrip | 66 | 1 |  |  |  |
| Slide | 37 | 12 | 11 |  | 3 |
| Diorama |  |  | 1 |  |  |
| Technical Drawing |  |  | 1 |  |  |


---


# Classes Present in the Data #

From that, it seems pretty clear that there is more than just "Book", "Serial", "Map", "Score", "Sound", "Computer" and "Visual" items.

Any MARC record already would likely have:

**foaf:Agent** - these could be foaf:Person or foaf:Organization, but these would be the creators, the collaborators, the publishers, etc.

**skos:Concept** - for the subjects

**frbr:Work** - Theoretically this would exist, although it may be difficult to determine from the MARC record.

**frbr:Expression** - This, in my mind, is the hardest to glean from the MARC, but others may know an obvious way.

**frbr:Manifestation** - In theory, this is where the MARC record lives.  Are there ways to know for sure?

**frbr:Place** - This could be in subjects, publisher locations, conference locations.

**dcterms:ProvenanceStatement** - There may be more appropriate ways to express this, but I'll throw this out as a proposal.  A MARC record contains data not only about the resource it's describing, but the record as a whole (date created/modified, source, etc.).

The bib data itself shows some of the following types of resources (although this is not exhaustive):

**bibo:Book** - Sometimes, after all, a book is just a book.

**bibo:Manuscript** - And sometimes it's never actually published.

**bibo:Proceedings**

which would also give us, at the same time

**bibo:Conference** - which would be useful to engineering schools, definitely.

**bibo:Thesis**

**bibo:Report** - For technical reports.  Law reports?

**bibo:Legislation**

**bibo:LegalCaseDocument**

**bibo:Standard**

**bibo:Patent**

**bibo:ReferenceSource** - covers encyclopedias, dictionaries and, I suppose, directories.  Indexes and abstracts, as well, possibly.  Catalogs?  Seems overly broad.

The only "book" concept that I couldn't find any real possible representation in bibo was "treaty".

Also there is no real concept of "Monographic Series".  It would probably be useful to know how many of these are conference proceedings (although not all could possibly be by the numbers).

The other thing missing is something to cover 'database' (in paper form, in this case).  Although there wouldn't be a whole lot of these to mess with, hopefully.

I don't know of an effective way to isolate magazines from journals, so most of these would be lumped as:

**bibo:Periodical** - at least until more data was available.

**bibo:Newspaper**

**bibo:Website**

**bibo:Map**

and

**map:Map**

No way to specify globe, atlas, etc.  Perhaps there are RDA elements that can help here.

Computer is also tough.

**bibo:Document** - for COM Document, maybe BKS/SER Programmed Text

Other than that, there is nothing really that describes the others well.  Numeric data seems useful as its own class.  "Computer program" and "interactive multimedia" seem less useful.

**mo:MusicalRecording** - for most things in REC

**mo:Recording** - for 'not a musical recording', 'other', 'unknown' and probably 'various forms'

Many of these could also be modeled with:
http://www.kanzaki.com/ns/music, although this might be overkill

**mo:Score**

The visual resources are also difficult to model.  VRACore?  Bibo describes a few:

**bibo:Film**

**bibo:Slide**

Flash cards, realia, art, technical drawings, games, toys, kits, graphics, charts and dioramas not represented.