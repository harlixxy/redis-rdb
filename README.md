# redis-rdb #

This library provides a set of modules and classes that make it easy to handle binary
database dumps generated by [Redis](http://redis.io) (.rdb files) in Ruby.

Currently redis-rdb allows developers to read .rdb files in a streamable flashion
with `RDB::Reader` by providing a set of callbacks. Database objects can optionally
be filtered by database / key / type using custom filters.

```ruby
require 'rdb'

class MyCallbacks
  include RDB::ReaderCallbacks

  KEY_SELECTOR = Regexp.compile(/user:\d+/)

  def accept_object?(state)
    state.database == 15 && KEY_SELECTOR.match(state.key)
  end

  def set(key, value, state)
    puts "SET \"#{key}\" \"#{value}\""
  end
end

RDB::Reader.read_file('dump.rdb', callbacks: MyCallbacks.new)
```

For more details about the supported callbacks you can take a look at the source code in
[lib/rdb/callbacks.rb](https://github.com/nrk/redis-rdb/blob/master/lib/rdb/callbacks.rb).

## Additional notes and credits ##

Right now there is still no documentation about the binary format of .rdb files so we used
the code in [rdb.c](https://github.com/antirez/redis/blob/unstable/src/rdb.c) as the reference
implementation. Credit goes also to [sripathikrishnan](https://github.com/sripathikrishnan),
his work on the [redis-rdb-tools](https://github.com/sripathikrishnan/redis-rdb-tools) Python
library was quite an inspiration for the final design of `RDB::Reader` and we also reused the
.rdb files shipped with his project for testing.

## Dependencies ##
- Ruby >= 1.9.0

## Links ##

### Project ###
- [Source code](https://github.com/nrk/redis-rdb/)
- [Issue tracker](https://github.com/nrk/redis-rdb/issues)

### Related ###
- [Redis](http://redis.io/)
- [redis-rdb-tools](https://github.com/sripathikrishnan/redis-rdb-tools) (Python)

## Author ##

- [Daniele Alessandri](mailto:suppakilla@gmail.com) ([twitter](http://twitter.com/JoL1hAHN))

## License ##

The code for redis-rdb is distributed under the terms of the MIT license (see LICENSE).
