[Memcached Cloud](http://redislabs.com/memcached-cloud) is a fully-managed service for running your Memcached in a reliable and fail-safe manner. Your dataset is constantly replicated, so if a node fails, an auto-switchover mechanism guarantees data is served without interruption. Memcached Cloud provides various data persistence options as well as remote backups for disaster recovery purposes. You can quickly and easily get your apps up and running with Memcached Cloud through its through its App Service add-on at Azure Store - just tell us how much memory you need and get started instantly with your first Memcached bucket.
 
A Memcached bucket is created in seconds and from that moment on, all operations are fully-automated. The service completely frees developers from dealing with nodes, clusters, server lists, scaling and failure recovery, while guaranteeing absolutely no data loss.

## Creating A Memcached Cloud Service

Choose the Memcached Cloud add-on from the Azure Store's App Service catalog, select your plan and set a unique name for your resource, approve your purchase and you're done.

## Getting Your Memcached Cloud Connection Info

On your Azure add-ons console, wait for your Memcached Cloud service to be started. 
Then, simply click on your service's Connection Info and get your Memcached Cloud __host__, __port__, __usernamer__ and __password__.

* [Ruby](#ruby)
* [Rails](#rails)
* [Sinatra](#sinatra)
* [Unicron](#unicorn)
* [Java](#java)
* [Python](#python)
* [Django](#django)
* [PHP](#php)

## <a id="ruby"></a>Using Memcached with Ruby
[Dalli](https://github.com/mperham/dalli) is a high performance pure Ruby client for accessing memcached servers, which uses the binary protocol.

For usage with Rails 3.x, update the Gemfile:
	
	gem 'dalli'  
	
And then install the gem via Bundler:

	bundle install

Lastly, in your `config/environments/production.rb`:
		
    	config.cache_store = :dalli_store, "<your_memcached_cloud_host>:<your_memcached_cloud_port>".split(','), { :username => "<your_memcached_cloud_username>", :password => "<your_memcached_cloud_password>" }

### <a id="sinatra"></a>Configuring Memcached on Sinatra

Add this code snippet to your configure block:

	configure do
        . . .
		require 'dalli'
		$cache = Dalli::Client.new("<your_memcached_cloud_host>:<your_memcached_cloud_port>".split(','), :username => "<your_memcached_cloud_username>", :password => "<your_memcached_cloud_password>")
        . . .
	end

### <a id="unicorn"></a>Using Memcached on Unicorn

No special setup is required when using Memcached Cloud with a Unicorn server. For Rails apps on Unicorn, follow the instructions in the [Configuring Memcached from Rails](#rails) section and for Sinatra apps on Unicorn see [Configuring Memcached on Sinatra](#sinatra) section.

### Testing from Ruby
	
	$cache.set("foo", "bar")
	# => true
	$cache.get("foo")
	# => "bar"
	
## <a id="java"></a>Using Memcached with Java

[spymemcached](https://code.google.com/p/spymemcached/) is a simple, asynchronous, single-threaded memcached client written in Java. You can download the latest build from: https://code.google.com/p/spymemcached/downloads/list.

To use the maven repository, start by specifying the repository:
	
	<repositories>
	    <repository>
	      <id>spy</id>
	      <name>Spy Repository</name>
	      <layout>default</layout>
	      <url>http://files.couchbase.com/maven2/</url>
	      <snapshots>
	        <enabled>false</enabled>
	      </snapshots>
	    </repository>
	</repositories>

And specify the actual artifact as follows: 

	<dependency>
	  <groupId>spy</groupId>
	  <artifactId>spymemcached</artifactId>
	  <version>2.8.9</version>
	  <scope>provided</scope>
	</dependency>

Configure the connection to your Memcached Cloud service using your Connection Info and the following code snippet:

	try {
			// building the memcached client
			AuthDescriptor ad = new AuthDescriptor(new String[] { "PLAIN" },
			        new PlainCallbackHandler("<your_memcached_cloud_username>", "<your_memcached_cloud_password>"));
				
			MemcachedClient mc = new MemcachedClient(
			          new ConnectionFactoryBuilder()
			              .setProtocol(ConnectionFactoryBuilder.Protocol.BINARY)
			              .setAuthDescriptor(ad).build(),
			          AddrUtil.getAddresses("<your_memcached_cloud_host>:<your_memcached_cloud_port>"));
			          
	} catch (IOException ex) {
		// the memcached client could not be initialized. 
	} 
	
### Testing from Java

	mc.set("foo", 0, "bar");
	Object value = mc.get("foo");

## <a id="python"></a>Using Memcached with Python

[bmemcached](https://github.com/jaysonsantos/python-binary-memcached) is a pure, thread safe, python module to access memcached via its binary protocol.
 
Use pip to install it:
 
	pip install python-binary-memcached

Configure the connection to your Memcached Cloud service using your Connection Info and the following code snippet:
	
	import os
	import urlparse
	import bmemcached
	import json
	
	mc = bmemcached.Client("<your_memcached_cloud_host>:<your_memcached_cloud_port>".split(',').split(','), "<your_memcached_cloud_username>", "<your_memcached_cloud_password>")
	
### Testing from Python
	
	mc.set('foo', 'bar')
	print client.get('foo')

### <a id="django"></a>Using Memcached with Django

Memcached can be used as a django cache backend, with [django-bmemcached](https://github.com/jaysonsantos/django-bmemcached). 

To do so, install django-bmemcached:
 
	pip install django-bmemcached

Next, configure your `CACHES` in the `settings.py` file:

	import os
	import urlparse
	import json

	CACHES = {
		'default': {
		'BACKEND': 'django_bmemcached.memcached.BMemcached',
		'LOCATION': "<your_memcached_cloud_host>:<your_memcached_cloud_port>".split(','),
		'OPTIONS': {
            		'username': "<your_memcached_cloud_username>",
            		'password': "<your_memcached_cloud_password>"
			
        }
	  }
	}

### Testing from Django

	from django.core.cache import cache
	cache.set("foo", "bar")
	print cache.get("foo")
	
## <a id="php"></a>Using Memcached with PHP

[PHPMemcacheSASL](https://github.com/ronnywang/PHPMemcacheSASL) is a simple PHP class with SASL support.

Include the class in your project, and configure a connection to your Memcached Cloud service using your Connection Info with the following code snippet:
	
	<?php
	include('MemcacheSASL.php');
	
	$mc = new MemcacheSASL;
	$mc->addServer("<your_memcached_cloud_host>", "<your_memcached_cloud_port>");
	$mc->setSaslAuthData("<your_memcached_cloud_username>", "<your_memcached_cloud_password>");
	
### Testing from PHP

	$mc->add("foo", "bar");
	echo $mc->get("foo");

## Dashboard

Our dashboard presents all performance and usage metrics of your Memcached Cloud service on a single screen, as shown below:

![Dashboard](https://s3.amazonaws.com/paas-docs/memcached-cloud/Memcached+Cloud++Azure+Store.png)

To access your Memcached Cloud dashboard, simply click on your service's 'Manage' button in your Azure add-ons console.

You can then find your dashboard under the `MY DATABASES` menu.

## Support

Any Memcached Cloud support issues or product feedbacks are welcome via email at support@redislabs.com.

## Additional resources

* [Developers Resources](http://redislabs.com/memcached-cloud)
* [Memcached Wiki](https://code.google.com/p/memcached/wiki/NewStart)