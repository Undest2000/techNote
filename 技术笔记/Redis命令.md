# Redis keys 命令

| 序号  | 命令及描述                                                                                                                                                                    |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | [DEL key](https://www.runoob.com/redis/keys-del.html)<br>该命令用于在 key 存在时删除 key。                                                                                           |
| 2   | [DUMP key](https://www.runoob.com/redis/keys-dump.html)<br>序列化给定 key ，并返回被序列化的值。                                                                                         |
| 3   | [EXISTS key](https://www.runoob.com/redis/keys-exists.html)<br>检查给定 key 是否存在。                                                                                            |
| 4   | [EXPIRE key](https://www.runoob.com/redis/keys-expire.html) seconds<br>为给定 key 设置过期时间，以秒计。                                                                               |
| 5   | [EXPIREAT key timestamp](https://www.runoob.com/redis/keys-expireat.html)<br>EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。 |
| 6   | [PEXPIRE key milliseconds](https://www.runoob.com/redis/keys-pexpire.html)<br>设置 key 的过期时间以毫秒计。                                                                          |
| 7   | [PEXPIREAT key milliseconds-timestamp](https://www.runoob.com/redis/keys-pexpireat.html)<br>设置 key 过期时间的时间戳(unix timestamp) 以毫秒计                                         |
| 8   | [KEYS pattern](https://www.runoob.com/redis/keys-keys.html)<br>查找所有符合给定模式( pattern)的 key 。                                                                               |
| 9   | [MOVE key db](https://www.runoob.com/redis/keys-move.html)<br>将当前数据库的 key 移动到给定的数据库 db 当中。                                                                               |
| 10  | [PERSIST key](https://www.runoob.com/redis/keys-persist.html)<br>移除 key 的过期时间，key 将持久保持。                                                                                 |
| 11  | [PTTL key](https://www.runoob.com/redis/keys-pttl.html)<br>以毫秒为单位返回 key 的剩余的过期时间。                                                                                        |
| 12  | [TTL key](https://www.runoob.com/redis/keys-ttl.html)<br>以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。                                                                      |
| 13  | [RANDOMKEY](https://www.runoob.com/redis/keys-randomkey.html)<br>从当前数据库中随机返回一个 key 。                                                                                     |
| 14  | [RENAME key newkey](https://www.runoob.com/redis/keys-rename.html)<br>修改 key 的名称                                                                                         |
| 15  | [RENAMENX key newkey](https://www.runoob.com/redis/keys-renamenx.html)<br>仅当 newkey 不存在时，将 key 改名为 newkey 。                                                              |
| 16  | [SCAN cursor [MATCH pattern] [COUNT count]](https://www.runoob.com/redis/keys-scan.html)<br>迭代数据库中的数据库键。                                                                 |
| 17  | [TYPE key](https://www.runoob.com/redis/keys-type.html)<br>返回 key 所储存的值的类型。                                                                                              |

# Redis 字符串命令

下表列出了常用的 redis 字符串命令：

| 序号  | 命令及描述                                                                                                                                             |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [SET key value](https://www.runoob.com/redis/strings-set.html)<br>设置指定 key 的值。                                                                    |
| 2   | [GET key](https://www.runoob.com/redis/strings-get.html)<br>获取指定 key 的值。                                                                          |
| 3   | [GETRANGE key start end](https://www.runoob.com/redis/strings-getrange.html)<br>返回 key 中字符串值的子字符                                                  |
| 4   | [GETSET key value](https://www.runoob.com/redis/strings-getset.html)<br>将给定 key 的值设为 value ，并返回 key 的旧值(old value)。                               |
| 5   | [GETBIT key offset](https://www.runoob.com/redis/strings-getbit.html)<br>对 key 所储存的字符串值，获取指定偏移量上的位(bit)。                                          |
| 6   | [MGET key1 [key2..]](https://www.runoob.com/redis/strings-mget.html)<br>获取所有(一个或多个)给定 key 的值。                                                     |
| 7   | [SETBIT key offset value](https://www.runoob.com/redis/strings-setbit.html)<br>对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。                                 |
| 8   | [SETEX key seconds value](https://www.runoob.com/redis/strings-setex.html)<br>将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。                   |
| 9   | [SETNX key value](https://www.runoob.com/redis/strings-setnx.html)<br>只有在 key 不存在时设置 key 的值。                                                      |
| 10  | [SETRANGE key offset value](https://www.runoob.com/redis/strings-setrange.html)<br>用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。                    |
| 11  | [STRLEN key](https://www.runoob.com/redis/strings-strlen.html)<br>返回 key 所储存的字符串值的长度。                                                             |
| 12  | [MSET key value [key value ...]](https://www.runoob.com/redis/strings-mset.html)<br>同时设置一个或多个 key-value 对。                                        |
| 13  | [MSETNX key value [key value ...]](https://www.runoob.com/redis/strings-msetnx.html)<br>同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。                  |
| 14  | [PSETEX key milliseconds value](https://www.runoob.com/redis/strings-psetex.html)<br>这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。 |
| 15  | [INCR key](https://www.runoob.com/redis/strings-incr.html)<br>将 key 中储存的数字值增一。                                                                    |
| 16  | [INCRBY key increment](https://www.runoob.com/redis/strings-incrby.html)<br>将 key 所储存的值加上给定的增量值（increment） 。                                      |
| 17  | [INCRBYFLOAT key increment](https://www.runoob.com/redis/strings-incrbyfloat.html)<br>将 key 所储存的值加上给定的浮点增量值（increment） 。                          |
| 18  | [DECR key](https://www.runoob.com/redis/strings-decr.html)<br>将 key 中储存的数字值减一。                                                                    |
| 19  | [DECRBY key decrement](https://www.runoob.com/redis/strings-decrby.html)<br>key 所储存的值减去给定的减量值（decrement） 。                                        |
| 20  | [APPEND key value](https://www.runoob.com/redis/strings-append.html)<br>如果 key 已经存在并且是一个字符串， APPEND 命令将指定的 value 追加到该 key 原来值（value）的末尾。          |

### Redis 列表命令

下表列出了列表相关的基本命令：

| 序号  | 命令及描述                                                                                                                                                         |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [BLPOP key1 [key2 ] timeout](https://www.runoob.com/redis/lists-blpop.html)<br>移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。                                  |
| 2   | [BRPOP key1 [key2 ] timeout](https://www.runoob.com/redis/lists-brpop.html)<br>移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。                                 |
| 3   | [BRPOPLPUSH source destination timeout](https://www.runoob.com/redis/lists-brpoplpush.html)<br>从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| 4   | [LINDEX key index](https://www.runoob.com/redis/lists-lindex.html)<br>通过索引获取列表中的元素                                                                            |
| 5   | [LINSERT key BEFORE\|AFTER pivot value](https://www.runoob.com/redis/lists-linsert.html)<br>在列表的元素前或者后插入元素                                                    |
| 6   | [LLEN key](https://www.runoob.com/redis/lists-llen.html)<br>获取列表长度                                                                                            |
| 7   | [LPOP key](https://www.runoob.com/redis/lists-lpop.html)<br>移出并获取列表的第一个元素                                                                                     |
| 8   | [LPUSH key value1 [value2]](https://www.runoob.com/redis/lists-lpush.html)<br>将一个或多个值插入到列表头部                                                                  |
| 9   | [LPUSHX key value](https://www.runoob.com/redis/lists-lpushx.html)<br>将一个值插入到已存在的列表头部                                                                         |
| 10  | [LRANGE key start stop](https://www.runoob.com/redis/lists-lrange.html)<br>获取列表指定范围内的元素                                                                       |
| 11  | [LREM key count value](https://www.runoob.com/redis/lists-lrem.html)<br>移除列表元素                                                                                |
| 12  | [LSET key index value](https://www.runoob.com/redis/lists-lset.html)<br>通过索引设置列表元素的值                                                                          |
| 13  | [LTRIM key start stop](https://www.runoob.com/redis/lists-ltrim.html)<br>对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。                                 |
| 14  | [RPOP key](https://www.runoob.com/redis/lists-rpop.html)<br>移除列表的最后一个元素，返回值为移除的元素。                                                                            |
| 15  | [RPOPLPUSH source destination](https://www.runoob.com/redis/lists-rpoplpush.html)<br>移除列表的最后一个元素，并将该元素添加到另一个列表并返回                                             |
| 16  | [RPUSH key value1 [value2]](https://www.runoob.com/redis/lists-rpush.html)<br>在列表中添加一个或多个值                                                                    |
| 17  | [RPUSHX key value](https://www.runoob.com/redis/lists-rpushx.html)<br>为已存在的列表添加值                                                                              |

## Redis 集合命令

下表列出了 Redis 集合基本命令：

| 序号  | 命令及描述                                                                                                                       |
| --- | --------------------------------------------------------------------------------------------------------------------------- |
| 1   | [SADD key member1 [member2]](https://www.runoob.com/redis/sets-sadd.html)<br>向集合添加一个或多个成员                                   |
| 2   | [SCARD key](https://www.runoob.com/redis/sets-scard.html)<br>获取集合的成员数                                                       |
| 3   | [SDIFF key1 [key2]](https://www.runoob.com/redis/sets-sdiff.html)<br>返回第一个集合与其他集合之间的差异。                                     |
| 4   | [SDIFFSTORE destination key1 [key2]](https://www.runoob.com/redis/sets-sdiffstore.html)<br>返回给定所有集合的差集并存储在 destination 中    |
| 5   | [SINTER key1 [key2]](https://www.runoob.com/redis/sets-sinter.html)<br>返回给定所有集合的交集                                          |
| 6   | [SINTERSTORE destination key1 [key2]](https://www.runoob.com/redis/sets-sinterstore.html)<br>返回给定所有集合的交集并存储在 destination 中  |
| 7   | [SISMEMBER key member](https://www.runoob.com/redis/sets-sismember.html)<br>判断 member 元素是否是集合 key 的成员                       |
| 8   | [SMEMBERS key](https://www.runoob.com/redis/sets-smembers.html)<br>返回集合中的所有成员                                               |
| 9   | [SMOVE source destination member](https://www.runoob.com/redis/sets-smove.html)<br>将 member 元素从 source 集合移动到 destination 集合 |
| 10  | [SPOP key](https://www.runoob.com/redis/sets-spop.html)<br>移除并返回集合中的一个随机元素                                                  |
| 11  | [SRANDMEMBER key [count]](https://www.runoob.com/redis/sets-srandmember.html)<br>返回集合中一个或多个随机数                              |
| 12  | [SREM key member1 [member2]](https://www.runoob.com/redis/sets-srem.html)<br>移除集合中一个或多个成员                                   |
| 13  | [SUNION key1 [key2]](https://www.runoob.com/redis/sets-sunion.html)<br>返回所有给定集合的并集                                          |
| 14  | [SUNIONSTORE destination key1 [key2]](https://www.runoob.com/redis/sets-sunionstore.html)<br>所有给定集合的并集存储在 destination 集合中   |
| 15  | [SSCAN key cursor [MATCH pattern] [COUNT count]](https://www.runoob.com/redis/sets-sscan.html)<br>迭代集合中的元素                  |

## Redis 有序集合命令

下表列出了 redis 有序集合的基本命令:

| 序号  | 命令及描述                                                                                                                                                      |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | [ZADD key score1 member1 [score2 member2]](https://www.runoob.com/redis/sorted-sets-zadd.html)<br>向有序集合添加一个或多个成员，或者更新已存在成员的分数                              |
| 2   | [ZCARD key](https://www.runoob.com/redis/sorted-sets-zcard.html)<br>获取有序集合的成员数                                                                             |
| 3   | [ZCOUNT key min max](https://www.runoob.com/redis/sorted-sets-zcount.html)<br>计算在有序集合中指定区间分数的成员数                                                           |
| 4   | [ZINCRBY key increment member](https://www.runoob.com/redis/sorted-sets-zincrby.html)<br>有序集合中对指定成员的分数加上增量 increment                                       |
| 5   | [ZINTERSTORE destination numkeys key [key ...]](https://www.runoob.com/redis/sorted-sets-zinterstore.html)<br>计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 destination 中 |
| 6   | [ZLEXCOUNT key min max](https://www.runoob.com/redis/sorted-sets-zlexcount.html)<br>在有序集合中计算指定字典区间内成员数量                                                    |
| 7   | [ZRANGE key start stop [WITHSCORES]](https://www.runoob.com/redis/sorted-sets-zrange.html)<br>通过索引区间返回有序集合指定区间内的成员                                         |
| 8   | [ZRANGEBYLEX key min max [LIMIT offset count]](https://www.runoob.com/redis/sorted-sets-zrangebylex.html)<br>通过字典区间返回有序集合的成员                               |
| 9   | [ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT]](https://www.runoob.com/redis/sorted-sets-zrangebyscore.html)<br>通过分数返回有序集合指定区间内的成员                        |
| 10  | [ZRANK key member](https://www.runoob.com/redis/sorted-sets-zrank.html)<br>返回有序集合中指定成员的索引                                                                  |
| 11  | [ZREM key member [member ...]](https://www.runoob.com/redis/sorted-sets-zrem.html)<br>移除有序集合中的一个或多个成员                                                      |
| 12  | [ZREMRANGEBYLEX key min max](https://www.runoob.com/redis/sorted-sets-zremrangebylex.html)<br>移除有序集合中给定的字典区间的所有成员                                          |
| 13  | [ZREMRANGEBYRANK key start stop](https://www.runoob.com/redis/sorted-sets-zremrangebyrank.html)<br>移除有序集合中给定的排名区间的所有成员                                     |
| 14  | [ZREMRANGEBYSCORE key min max](https://www.runoob.com/redis/sorted-sets-zremrangebyscore.html)<br>移除有序集合中给定的分数区间的所有成员                                      |
| 15  | [ZREVRANGE key start stop [WITHSCORES]](https://www.runoob.com/redis/sorted-sets-zrevrange.html)<br>返回有序集中指定区间内的成员，通过索引，分数从高到低                             |
| 16  | [ZREVRANGEBYSCORE key max min [WITHSCORES]](https://www.runoob.com/redis/sorted-sets-zrevrangebyscore.html)<br>返回有序集中指定分数区间内的成员，分数从高到低排序                   |
| 17  | [ZREVRANK key member](https://www.runoob.com/redis/sorted-sets-zrevrank.html)<br>返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序                                        |
| 18  | [ZSCORE key member](https://www.runoob.com/redis/sorted-sets-zscore.html)<br>返回有序集中，成员的分数值                                                                 |
| 19  | [ZUNIONSTORE destination numkeys key [key ...]](https://www.runoob.com/redis/sorted-sets-zunionstore.html)<br>计算给定的一个或多个有序集的并集，并存储在新的 key 中                |
| 20  | [ZSCAN key cursor [MATCH pattern] [COUNT count]](https://www.runoob.com/redis/sorted-sets-zscan.html)<br>迭代有序集合中的元素（包括元素成员和元素分值）                           |
