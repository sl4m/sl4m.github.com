---
layout: post
title: first look at mongodb
date: 2010-07-15 21:00:00 -05:00
categories:
  -- nosql
  -- mongodb
---

Here's what I learned from today's experimentation with MongoDB.  Kudos to [10gen](http://10gen.com/) for the wonderful [documentation](http://www.mongodb.org/display/DOCS/Ruby+Language+Center).  Everything I needed to know about: from installation, to how it works, to what commands to use, were all right there in the docs.  There was even a [SQL to Mongo Mapping Chart](http://www.mongodb.org/display/DOCS/SQL+to+Mongo+Mapping+Chart) that helps SQL users translate MongoDB's commands and query statements.  Here's a SQL to Mongo mapping of the storage structure:

<table>
<tr><th>SQL</th><th>MongoDB</th></tr>
<tr><td>database</td><td>database</td></tr>
<tr class='even'><td>table</td><td>collection</td></tr>
<tr><td>row</td><td>document</td></tr>
<tr class='even'><td>column</td><td>field</td></tr>
<tr><td>primary key</td><td>_id</td></tr>
</table>

Using the mongo [gem](http://rubygems.org/gems/mongo) in an irb session, I learned about the classes in MongoDB.

<table>
<tr><th>MongoDB Class Name</th><th>Description</th></tr>
<tr><td>Mongo::Connection</td><td>Connection object holds a specific connection to the MongoDB server</td></tr>
<tr class='even'><td>Mongo::DB</td><td>Database object holds a specific database</td></tr>
<tr><td>Mongo::Collection</td><td>Collection object holds a specific collection</td></tr>
<tr class='even'><td>Mongo::Cursor</td><td>Cursor object holds a set of documents from a specified query</td></tr>
<tr><td>BSON::OrderedHash</td><td>OrderedHash object holds a document</td></tr>
</table>

<pre><code class="no-highlight">
[~] irb
ruby-1.8.7-p299 > require 'rubygems'
ruby-1.8.7-p299 > require 'mongo'
ruby-1.8.7-p299 > include Mongo
ruby-1.8.7-p299 > connection = Connection.new
ruby-1.8.7-p299 > db = connection.db('test_db')
ruby-1.8.7-p299 > collection = db.collection('test_coll')
ruby-1.8.7-p299 > collection.insert({'name' => 'sam', 'animal' => 'dog'})
ruby-1.8.7-p299 > collection.insert({'name' => 'dixie', 'animal' => 'cat', 'breed' => 'maneki neko'})
ruby-1.8.7-p299 > collection.insert({'name' => 'dixie', 'animal' => 'fish'})
ruby-1.8.7-p299 > cursor = collection.find
ruby-1.8.7-p299 > cursor.to_a[0]["name"]
 => "sam"
</code></pre>

*Note:* I took out the return values for most of the commands in the IRB session.

Once you have a collection object, there are many ways to query the collection.  The basic query command is the *find* command as shown above.  Without any arguments, it returns the entire collection as a Mongo::Collection object.  With arguments, you can specify which documents you want returned in a cursor (Mongo::Cursor).

Find a document object with a 'name' field, and 'dixie' value.

<pre><code class="no-highlight">
ruby-1.8.7-p299 > collection.find({'name' => 'dixie'})
 => &lt;Mongo::Cursor:0x8094bb0c namespace='test_db.test_coll' @selector={"name"=>"dixie"}>
</code></pre>

You can search using any field.

<pre><code class="no-highlight">
ruby-1.8.7-p299 > collection.find({'animal' => 'dog'})
 => &lt;Mongo::Cursor:0x80949488 namespace='test_db.test_coll' @selector={"animal"=>"dog"}>
</code></pre>

You can search against multiple fields to get a more refined search.

<pre><code class="no-highlight">
ruby-1.8.7-p299 > collection.find({'name' => 'dixie', 'animal' => 'fish'})
 => &lt;Mongo::Cursor:0x809414f4 namespace='test_db.test_coll' @selector={"name"=>"dixie", "animal"=>"fish"}>
</code></pre>

If not searching with exact values, you can use regular expressions or [conditional operators](http://www.mongodb.org/display/DOCS/Advanced+Queries).

<pre><code class="no-highlight">
ruby-1.8.7-p299 > collection.find({'name' => /^d/})
 => &lt;Mongo::Cursor:0x8090b048 namespace='test_db.test_coll' @selector={"name"=>/^d/}>
</code></pre>

I plan to write all of the possible moves in a 4x4 Tic Tac Toe game and export the collection as a \*.bson file.

<pre><code class="no-highlight">
{'board' => [], 'best_moves' => []}
</code></pre>

Backup to \*.bson file

<pre><code class="no-highlight">
[~/local/mongodb/backup] mongodump --db test_db --collection test_coll
connected to: 127.0.0.1
DATABASE: test_db	 to 	dump/test_db
	test_db.test_coll to dump/test_db/test_coll.bson
		 3 objects
</code></pre>

