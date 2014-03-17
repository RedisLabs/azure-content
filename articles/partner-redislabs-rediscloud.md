[Redis Cloud](http://redislabs.com/redis-cloud) is a fully-managed cloud service for hosting and running your Redis dataset in a highly-available and scalable manner, with predictable and stable top performance. You can quickly and easily get your apps up and running with Redis Cloud through its App Service add-on at Azure Store - just tell us how much memory you need and get started instantly with your first Redis database.
 
Redis Cloud offers true high-availability with its in-memory dataset replication and instant auto-failover mechanism, combined with its fast storage engine. You can easily import an existing dataset to any of your Redis Cloud databases, from Azure blobs storage, FTP/HTTP server or from any other Redis server. Daily backups are performed automatically and in addition, you can backup your dataset manually at any given time. The service guarantees high performance, as if you were running the strongest cloud instances.


## Creating A Redis Cloud Service

Choose the Redis Cloud add-on from the Azure Store's App Service catalog, select your plan and set a unique name for your resource, approve your purchase and you're done. 

## Getting Your Redis Cloud Connection Info

On your Azure add-ons console, wait for your Redis Cloud service to be started. 
Then, simply click on your service's Connection Info and get your Redis Cloud __host__, __port__ and __password__.

* [Ruby](#ruby)
* [Rails](#rails)
* [Sinatra](#sinatra)
* [Java](#java)
* [Python](#python)
* [Django](#django)
* [PHP](#php)
* [Node.js](#node)


## <a id="ruby"></a>Using Redis from Ruby
The [redis-rb](https://github.com/redis/redis-rb) is a very stable and mature Redis client and is probably the easiest way to access Redis from Ruby. 

Install redis-rb:
	
	sudo gem install redis

### <a id="rails"></a>Configuring Redis from Rails

For Rails 2.3.3 up to Rails 3.0, update the `config/environment.rb` to include the redis gem:
	
	config.gem 'redis' 

For Rails 3.0 and above, update the Gemfile:
	
	gem 'redis'  
	
And then install the gem via Bundler:

	bundle install

Lastly, create a new `redis.rb` initializer in `config/initializers/` and add the following code snippet: 
	
    	$redis = Redis.new(:host => "<your_redis_cloud_host>", :port => <your_redis_cloud_port>, :password => "<your_redis_cloud_password>")

### <a id="sinatra"></a>Configuring Redis on Sinatra

Add this code snippet to your configure block:

	configure do
        . . .
		require 'redis'
		$redis = Redis.new(:host => "<your_redis_cloud_host>", :port => <your_redis_cloud_port>, :password => "<your_redis_cloud_password>")
        . . .
	end

### <a id="java"></a>Using Redis on Unicorn

No special setup is required when using Redis Cloud with a Unicorn server. For Rails apps on Unicorn, follow the instructions in the [Configuring Redis from Rails](#rails) section and for Sinatra apps on Unicorn see [Configuring Redis on Sinatra](#sinatra) section.

### Testing (Ruby)
	
	$redis.set("foo", "bar")
	# => "OK"
	$redis.get("foo")
	# => "bar"
	
## Using Redis from Java

[Jedis](https://github.com/xetorthio/jedis) is a blazingly small, sane and easy to use Redis java client. You can download the latest build from [github](http://github.com/xetorthio/jedis/downloads) or use it as a maven dependency:

	<dependency>
		<groupId>redis.clients</groupId>
		<artifactId>jedis</artifactId>
		<version>2.0.0</version>
		<type>jar</type>
		<scope>compile</scope>
	</dependency>

Configure the connection to your Redis Cloud service using your Connection Info and the following code snippet:

	JedisPool pool = new JedisPool(new JedisPoolConfig(),
	 		"<your_redis_cloud_host>",
	    		<your_redis_cloud_port>,
	    		Protocol.DEFAULT_TIMEOUT,
	    		"<your_redis_cloud_password>");
	
### Testing (Java)

	jedis jedis = pool.getResource();
	jedis.set("foo", "bar");
	String value = jedis.get("foo");
	// return the instance to the pool when you're done
	pool.returnResource(jedis);

(example taken from Jedis docs).

## <a id="python"></a>Using Redis from Python

[redis-py](https://github.com/andymccurdy/redis-py) is the most commonly-used client for accessing Redis from Python.
 
Use pip to install it:
 
	sudo pip install redis

Configure the connection to your Redis Cloud service using your Connection Info and the following code snippet:
	
	import os
	import urlparse
	import redis
	import json
	
	r = redis.Redis(host="<your_redis_cloud_host>", port=<your_redis_cloud_port>, password="<your_redis_cloud_password>")
	
### Testing (Python):
	
	>>> r.set('foo', 'bar')
	True
	>>> r.get('foo')
	'bar'

### <a id="django"></a>[Django-redis-cache](https://github.com/sebleier/django-redis-cache) 

Redis can be used as the back-end cache for Django. To do so, install django-redis-cache:
 
	pip install django-redis-cache

Next, configure your `CACHES` in the `settings.py` file:

	import os
	import urlparse
	import json
	
	CACHES = {
		'default': {
		'BACKEND': 'redis_cache.RedisCache',
		'LOCATION': '%s:%s' % ("<your_redis_cloud_host>", <your_redis_cloud_port>),
		'OPTIONS': {
            'PASSWORD': "<your_redis_cloud_password>",
			'DB': 0,
        }
	  }
	}

### Testing (Django)

	from django.core.cache import cache
	cache.set("foo", "bar")
	print cache.get("foo")
	
## <a id="php"></a>Using Redis from PHP

[Predis](https://github.com/nrk/predis) is a flexible and feature-complete PHP client library for Redis.

Instructions for installing the [Predis](https://github.com/nrk/predis) library can be found [here](https://github.com/nrk/predis#how-to-use-predis).

Loading the library to your project should be straightforward:

	<?php
	// prepend a base path if Predis is not present in your "include_path".
	require 'Predis/Autoloader.php';
	Predis\Autoloader::register();
	
Configure connection to your Redis Cloud service using your Connection Info and the following code snippet:

	$redis = new Predis\Client(array(
		'host' => '<your_redis_cloud_host>', 
		'port' => <your_redis_cloud_port>,
		'password' => '<your_redis_cloud_password>' 
	));
	
### Testing (PHP)

	$redis->set('foo', 'bar');
	$value = $redis->get('foo');
	
## <a id="node"></a>Using Redis from Node.js

[node_redis](https://github.com/mranney/node_redis) is a complete Redis client for node.js. 

You can install it with:

	npm install redis

Configure the connection to your Redis Cloud service using your Connection Info and the following code snippet:

	var redis = require('redis');
	var client = redis.createClient(<your_redis_cloud_port>, "<your_redis_cloud_host>", {no_ready_check: true});
	client.auth("<your_redis_cloud_password>");
	
###Testing (Node.js)

	client.set('foo', 'bar');
	client.get('foo', function (err, reply) {
        console.log(reply.toString()); // Will print `bar`
    });
	
## Dashboard

Our dashboard presents all performance and usage metrics of your Redis Cloud service on a single screen, as shown below:

![Dashboard](https://s3.amazonaws.com/paas-docs/redis-cloud/Redis+Cloud+-+Azure+Store.png)

To access your Redis Cloud dashboard, simply click on your service's 'Manage' button in your Azure add-ons console.

You can then find your dashboard under the `MY DATABASES` menu.

## Support

Any Redis Cloud support issues or product feedbacks are welcome via email at: support@redislabs.com.

## Additional resources

* [Developers Resources](http://redislabs.com/redis-cloud)
* [Redis Documentation](http://redis.io/documentation)