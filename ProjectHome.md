Kochief is a discovery interface for local and remote collections.  Records are indexed in Solr and presented with the Django web framework.  Tools are in development for the management of a local catalog.

## History ##
Kochief began with Casey Durfee's **Open Source Endeca in 250 lines or less** presentation at the code4lib 2007 conference, where he claimed to provide faceted features similar to the Endeca and AquaBrowser experiences using [Solr](http://lucene.apache.org/solr) and [Django](http://djangoproject.org).

Casey's work focused strictly on the Dewey-based Horizon system he works with at the [Seattle Public Library](http://spl.org). After downloading his code, Dan Scott was able to tweak the code to work with his LC-based Unicorn system at the [Laurentian University Library](http://laurentian.ca/library). He wanted to demonstrate a faceted search interface to my colleagues, and he also needed a backup catalogue because the production ILS had a nasty habit of dying at inconvenient times. He called the project "Fac-Back-OPAC".

Casey gave his blessing to do whatever Dan wanted with the code, so he made it available under the Apache 2.0 license, started with Casey's original code in the project SVN repository, and began checking in changes. Mike Beccaria, who also attended Casey's presentation, quickly joined the project as he had been working on his own fork of Casey's code and has added several enhancements. He works at Paul Smith's College and for a time used Kochief for their [book catalog](http://library.paulsmiths.edu/catalog). You're welcome to join us too!

## News ##
  * 2012/01/18: A fork of Kochief (https://github.com/jermnelson/Discover-Aristotle) is live at http://discovery.coloradocollege.edu/.
  * 2009/04/04: Project moved from Fac-Back-OPAC to Kochief.
  * 2008/01/31: A screencast of fbo being installed from start to finish in windows is available. [Download](http://fac-back-opac.googlecode.com/files/fbowidowsinstall.zip), unzip, and open fbowindowsinstall.html.
  * 2007/10/02: Casey's code for Helios has been committed to the **Helios** branch of the Subversion repository.
  * 2007/10/02: Gabe Farrell has been added as another Fac-Back-OPAC developer. Gabe plans to clean up some of the non-Pythonic code and improve the Django project structure, for starters. Welcome Gabe!
  * 2007/10/01: "[Fac-Back-OPAC: An Open Source Interface to Your Library System](http://www.infotoday.com/cilmag/oct07/Beccaria_Scott.shtml)", an article about Fac-Back-OPAC written by Mike Beccaria and Dan Scott, has been published online by Computers in Libraries.
  * 2007/08/23: Casey has joined the project, and plans to contribute Helios (the most up-to-date version of his original code, featuring tag clouds and other goodness). Hurray!

## Examples ##
  * [Drexel Libraries Collections](http://sets.library.drexel.edu/)

## Changes ##
Casey's code was pretty good to start with, but we're trying to make it even better:
  * The code is now i18n-compatible, so you can offer the catalog with support for multiple languages. I have included a French translation of the interface.
  * Atom and RSS feeds are available for searches.
  * A detailed item view within FacBackOPAC is an option if you don't want to link to your existing catalogue for a detailed item display.

## Details ##
At the moment, the code needs lots more work, but the following leaps out at me:
  * [Document](ProjectDocumentation.md) the steps you need to follow to get a FacBackOPAC instance running. The high-level details have been written, but some pointers on localizing the interface would make sense.
  * Find some reasonable media format icons to bundle with the code. Pointers to good sources of freely redistributable icons would be appreciated!
  * Build a working implementation of tracking cataloging activity (in Unicorn, the plan is to scan the transaction logs) to incrementally add or delete items.
  * Add support for other integrated library systems. We'll need to depend on support from others for this.