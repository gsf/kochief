#summary How to get Kochief up and running.
#labels Phase-Deploy

# Requirements #
  * [Subversion](http://subversion.tigris.org/) client
  * Java JDK 1.5 or higher - **Note**: You need a JDK or SDK to run Solr, not just a JRE.
  * [Solr](http://lucene.apache.org/solr/) - tested successfully with Solr versions 1.1 and 1.2, though a Subversion checkout of the trunk is recommended.
  * Python - tested successfully with Python versions 2.4, 2.5, and (if you're willing to give up I18N support) 2.3
  * [Django](http://djangoproject.org) - use the Subversion trunk or the latest release.

# Details #
  1. [Check out](http://code.google.com/p/kochief/source) the latest version of Kochief from the repository.  On my machine, it looks like this:
```
gsf@manheim:~/svn$ svn co http://kochief.googlecode.com/svn/trunk kochief
```
  1. Run `ant example` in your Solr source to build the example directory.
```
manheim:/usr/local/src/solr# ant example
```
  1. Symlink the `solr` directory from within the Kochief checkout to a directory in `example/multicore` in Solr.
```
manheim:~# ln -s /home/gsf/svn/kochief/kochief/solr/ \
/usr/local/src/solr/example/multicore/test
```
  1. Edit `example/multicore/solr.xml` in your Solr source to include the symlinked directory.  I added `<core name="test" instanceDir="test" />`.
  1. Start Solr by running the script included in Kochief at `solr/solr-init`.  You may need to `chmod 755` it first.  Also, check the variables at the top of that file to make sure they're correct for your implementation.  At some point you may want to copy solr-init to `/etc/init.d/solr` and update your init script links to run it as a system service.
```
manheim:/home/gsf/svn/kochief/kochief# chmod 755 solr/solr-init 
manheim:/home/gsf/svn/kochief/kochief# solr/solr-init start
Starting Solr....
```
  1. Index your records.  It's a good idea to index in chunks of 50,000 records or less to minimize disk and memory use and to correct any errors.  To index a dump of MARC records at `/home/gsf/marcdump.mrc`, for example, I run the indexer like so:
```
gsf@manheim:~/svn/kochief/kochief$ python manage.py index /home/gsf/marcdump.mrc
```
  1. The indexer recognizes the MARC filetype by the "mrc" extension, but you can also specify it on the command line.  For a list of options, run `python manage.py help index`.  We're in the process of developing parsers for many types of records.
  1. To test the install out, sync the Django database (for session and admin data) and start the built-in Django web server. It defaults to `127.0.0.1:8000`, so it will only listen to requests from a browser running on the same machine.
```
gsf@manheim:~/svn/kochief/kochief$ python manage.py syncdb
gsf@manheim:~/svn/kochief/kochief$ python manage.py runserver
```
  1. Test the search interface by connecting to the catalog at http://127.0.0.1:8000/. Type in a search term and you should see a list of facets on the right and results in the main section. Facets can be changed in `settings.py`. Templates are in `templates/`, with the template for the initial search screen at `templates/discovery/index.html` and the results screen at `templates/discovery/search.html`. Everything is organized according to Django conventions -- see the [Django docs](http://docs.djangoproject.com) for help.

# Screencast Install #
Mike Beccaria made a screencast install for Kochief on Windows XP. Note that some steps have changed since this screencast was made. To see it, [download](http://kochief.googlecode.com/files/fbowindowsinstall.zip), unzip, and open fbowindowsinstall.html.