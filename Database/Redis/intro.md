# Redis

## Overview
- NoSQL database
- no tables/document, all key-value pairs
- run inside working memory (RAM) instead of disk
- used for caching
- quick!
- lies between traditional database and program
- speed query up by 10-15 times

## Install & start
use homebrew to install in terminal

To run the server:
```
$ redis-server
```
It is running on port 6379 (default). 

Open a new terminal and start cli:
```
$ redis-cli
```
Type `quit` to exit cli. 

## CLI
| Command | Example | Output | Note |
| -------- | ------- | ------- | ------- |
| `SET key value` | `$ SET name karin` | <pre>OK</pre> | lowercase/uppercase does not matter |
| `GET key` | `$ GET name` | <pre>"karin"</pre> | Everything stores in type `string` | 
| `DEL key` | `$ DEL name` | <pre>(interger) 1</pre> |  |
| `EXISTS key` | `$ EXISTS name` | <pre>(integer) 0</pre> |  |
| `KEYS *` | `$ KEYS *` | <pre>1) "name" <br>2) "gender"</pre> |  |
| `flushall` | `$ flushall` |  | empty everything |
| `ttl key` | `$ ttl name` | <pre>(integer) -1</pre> | time to live, -1: forever |
| `expire key time` | `$expire name 10` | <pre>(integer) 1</pre> | set ttl (in seconds) |
| `setex key time value` | `setex name 10 karin` | <pre>OK</pre> | set ttl when creating new pair |
|  |  |  | arrays/list |
| `lpush key value` | `$ lpush friends john` | <pre>(integer) 1</pre> | push to left |
| `lrange key start stop` | `$ lrange friends 0 -1` | <pre>1) "sally" <br>2) "john"</pre> |  |
| `rpush key value` | `$ rpush friends mike` | <pre>1) "sally" <br>2) "john" <br>3) "mike"</pre> |  |
| `LPOP key` | `LPOP friends` | <pre>1) "john" <br>2) "mike"</pre> |  |
| `RPOP key` | `RPOP friends` | <pre>1) "john"</pre> |  |
|  |  |  | sets |
| `SADD key member` | `$ SADD hobbies "weight lifting"` | <pre>(integer) 1</pre> | Set items are unique. `(integer) 0 ` if adding a duplicated value |
| `SMEMBERS key` | `$ SMEMBERS hobbies` | <pre>1) "weight lifting"</pre> |  |
| `SREM key member` | `$ SREM hobbies "weight lifting"` | <pre>(integer) 1</pre> | `(empty list/set)` |
|  |  |  | hashs |
| `HSET key field value` | `$ HSET person name karin` | <pre>(integer) 1</pre> |  |
| `HGET key field` | `$ HGET person name` | <pre>"karin"</pre> |  |
| `HGETALL key` | `$ HGETALL person` | <pre>1) "name" <br>2) "karin"<br>3) "age" <br>4) "18"</pre> | 1) key1 2) value1 3) key2 4) value2 ... |
| `HDEL key field` | `$ HDEL person age` | <pre>(integer) 1</pre> |  |
| `HEXISTS key field` | `HEXISTS person age` | <pre>(integer) 0</pre> |  |

## Example use case
```
const DEFAULT_EXPIRATION = 3600
const Redis = require('redis')
const redisClient = Redis.createClient()  // localhost port by default


redisClient.get('photos', (error, photos) => {
  if (error) ...
  if (photos != null) {
    return res.json(JSON.parse(photos))  // parse string to JSON
  } else {
    const { data } = await ...  // query from database if nothing found in cache (Redis)
    redisClient.setex('photos', DEFAULT_EXPIRATION, JSON.stringify(data))  // cast to string to store
  }
  res.json(data)
})
```



