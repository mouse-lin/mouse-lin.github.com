---
layout: post
title: "Rails cache fetch"
description: ""
category: Rails
tags: [Rails]
---

---
### ActiveSupport::Cache::Store(Rails cache)

* fetch

According to rails api: Fetches data from the cache, using the given key. If there is data in the cache with the given key, then that data is returned.

If there is no such data in the cache (a cache miss), then nil will be returned. However, if a block has been passed, that block will be passed the key and executed in the event of a cache miss. The return value of the block will be written to the cache under the given cache key, and that return value will be returned.

    cache.write('today', 'Monday')
    cache.fetch('today')  # => "Monday"
    
    cache.fetch('city')   # => nil
    cache.fetch('city') do
    		'Duckburgh'
    end
    
    cache.fetch('city')   # => "Duckburgh"

You may also specify additional options via the options argument. Setting force: true will force a cache miss:

    cache.write('today', 'Monday')
    cache.fetch('today', force: true)  # => nil

Setting :compress will store a large cache entry set by the call in a compressed format.

Setting :expires_in will set an expiration time on the cache. All caches support auto-expiring content after a specified number of seconds. This value can be specified as an option to the constructor (in which case all entries will be affected), or it can be supplied to the fetch or write method to effect just one entry.

    cache = ActiveSupport::Cache::MemoryStore.new(expires_in: 5.minutes)
    cache.write(key, value, expires_in: 1.minute) # Set a lower value for one entry

Setting :race_condition_ttl is very useful in situations where a cache entry is used very frequently and is under heavy load. If a cache expires and due to heavy load seven different processes will try to read data natively and then they all will try to write to cache. To avoid that case the first process to find an expired cache entry will bump the cache expiration time by the value set in :race_condition_ttl. Yes, this process is extending the time for a stale value by another few seconds. Because of extended life of the previous cache, other processes will continue to use slightly stale data for a just a big longer. In the meantime that first process will go ahead and will write into cache the new value. After that all the processes will start getting new value. The key is to keep :race_condition_ttl small.

If the process regenerating the entry errors out, the entry will be regenerated after the specified number of seconds. Also note that the life of stale cache is extended only if it expired recently. Otherwise a new value is generated and :race_condition_ttl does not play any role.

    # Set all values to expire after one minute.
    cache = ActiveSupport::Cache::MemoryStore.new(expires_in: 1.minute)
    
    cache.write('foo', 'original value')
    val_1 = nil
    val_2 = nil
    sleep 60
    
    Thread.new do
      val_1 = cache.fetch('foo', race_condition_ttl: 10) do
        sleep 1
        'new value 1'
      end
    end
    
    Thread.new do
      val_2 = cache.fetch('foo', race_condition_ttl: 10) do
        'new value 2'
      end
    end
    
    # val_1 => "new value 1"
    # val_2 => "original value"
    # sleep 10 # First thread extend the life of cache by another 10 seconds
    # cache.fetch('foo') => "new value 1"

Other options will be handled by the specific cache store implementation. Internally, fetch calls read_entry, and calls write_entry on a cache miss. options will be passed to the read and write calls.

For example, MemCacheStore’s write method supports the :raw option, which tells the memcached server to store all values as strings. We can use this option with fetch too:

  cache = ActiveSupport::Cache::MemCacheStore.new
  cache.fetch("foo", force: true, raw: true) do
    :bar
  end
  cache.fetch('foo') # => "bar"

**source code:**

    def fetch(name, options = nil)
      if block_given?
      	options = merged_options(options)
      	key = namespaced_key(name, options)
      
      	cached_entry = find_cached_entry(key, name, options) unless options[:force]
      	entry = handle_expired_entry(cached_entry, key, options)
      
      	if entry
        get_entry_value(entry, name, options)
      	else
        save_block_result_to_cache(name, options) { |_name| yield _name }
        end
      else
        read(name, options)
      end
    end

[Read it on Rails API](http://api.rubyonrails.org/)
