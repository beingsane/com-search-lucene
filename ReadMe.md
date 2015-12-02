# Introduction #

_com\_search\_lucene_ is a Joomla 1.5 extension set, consisting of a search component, some search plugins and an indexing application. com-search-lucene brings Apache Lucene's indexing and searching capabilities to the Joomla! world. Native Joomla! com\_search component uses plugins that directly uses databases search capabilities. This brings the database feature lock-in. For example some search plugins may use Mysql Full-Text Search ability that's available with the MyISAM storage engine but lacking with InnoDB storage engine. So sites depending on these search plugins are locked in to MyISAM and can not use innodb or any other storage engine which lacks full text search.

com\_search\_lucene can be used with plain php via Zend Framework's Zend-lucene library or with native java lucene via php-java-bridge. For the moment, only indexer can use native lucene library.


# Structure of com\_search\_lucene #

_com\_search\_lucene_ has full Joomla application integration. There is a component part to replace _com\_search_ and there are plugins to change Joomla! _com\_search's_ search plugins.
_com\_search\_lucene_ plugins use **SearchLucene** plugin type name. These plugins describe how indexing occurs and and makes searches in these indexes. A SearchLucene Plugin means a new index will be created by the indexer for the plugin.

_com\_search\_lucene_ Indexer is a self contained Joomla! Application like the xml-rpc app distributed with the Joomla! package. This application is run by a scheduler like linux/unix cron or Windows Scheduled Tasks. At the moment every index, that is described with enabled SearchLucene Pluginsi is deleted and recreated everytime the indexer runs. So re-indexing is just a new index creation.

There are two Indexer Applications. All written in php.
  * One using Zend Framework's ZendLucene php library, not needing any java components. - Caution: This is really a slow indexer.
  * One using php-java-bridge and native Java Lucene library, depending on php-java-lucene, a java application server, a java vm and Apache Lucene library.

Indexer applications are console Joomla! applications. And can be run from Cron daemon.

# Install #

> ## Installing The indexer ##
> > Preliminary Steps
      * Download _com\_search\_lucene indexer package_
      * Download _ZendFramework Minimal Package_
      * Extract _com\_search\_lucene indexer package_ into your Joomla! root directory. A directory named **search\_lucene** should appear.
      * Extract _ZendFramework Minimal Package_ into **search\_lucene** directory. This will create a directory like **ZendFramework-1.9.3PL1-minimal** (note: your Zend Library version may be different. It's not advised to use a ZendFramework version lower than 1.9.3)
      * Move or softlink **ZendFramework-1.9.3PL1-minimal/library/Zend** directory to **search\_lucene/Zend** directory.


> Installing Indexer with ZendFramework SearchLucene Library
    * If you would like to use native php and no java, After doing preliminary steps no extra steps are required. You can jump to section named "Configuring com\_search\_lucene" to learn more about using the indexer.

> Installing Indexer with php-java-bridge
    * If you would like to use Java Lucene, you have to install php-java-bridge. It's installation is not a subject of this document. You have to consult php-java-bridge documentation. These documents can be found at their site.
    * After installing php-java-bridge you have to install Apache Lucene into php-java-bridge applications **WEB-INF/lib** directory, for my debian/tomcat5.5/JavaBridge553 installation this directory is **/var/lib/tomcat5.5/webapps/JavaBridgeTemplate553/WEB-INF/lib/**.
    * After redeployment of the application you must change two define lines located in the **com\_search\_lucene\_java\_indexer.php**,
```
          define("JAVA_HOSTS", "localhost:8080");
          define("JAVA_SERVLET", "/JavaBridgeTemplate553/JavaBridge.phpjavabridge");
```
> > > for your setup. JAVA\_HOSTS constant is your application server's url and port, JAVA\_SERVLET constant is your php-java-bridge applications path on the application server.



> ## Installing Component ##
> Installing the component is a normal component installation procedure.