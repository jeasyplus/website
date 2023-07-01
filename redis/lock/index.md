## Redis分布式锁实现

**释放锁的lua脚本**

```java
// 解锁脚本
String UN_LOCK_SCRIPT = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
//解锁成功的返回值        
Long SUCCESS = 1L;
```

**保存锁信息的容器：**

```java
ThreadLocal<String> lockHolder = new ThreadLocal<>();
```

**Redis客户端：**

```java
@Autowired
RedisTemplate<Object, Object> redisTemplate;
```
**锁定实现：**
```java
public boolean lock(String key,long lockExpireTime){
    try {
        String prefix = lockKeyPrefix(); //锁的前缀
        String lockKey = prefix + key;
        String value = UUID.randomUUID().toString().replace("-", ""); //唯一值
        lockHolder.set(value); //将本次锁的唯一性值放入当前线程
        //将值存入redis
        return redisTemplate.opsForValue().setIfAbsent(lockKey, value, lockExpireTime, TimeUnit.MINUTES);
    } catch (Exception e) {
        log.warn("获取锁定失败,{}","......",e);
    }
    return false;
}
```
**解锁实现：**
```java
public boolean unlock(String key) {
    try {
        String prefix = lockKeyPrefix(); //锁的前缀
        String lockKey = prefix + key;
        String value = lockHolder.get();  //从当前线程获取唯一值
        //将key和value送入redis，使用lua脚本先做value比对，equals则解锁
        Object result = redisTemplate.execute(new DefaultRedisScript<Long>(UN_LOCK_SCRIPT, Long.class), Collections.singletonList(lockKey), value);
        //解锁成功
        if (SUCCESS.equals(result)) {
            return true;
        }
    } catch (Exception e) {
        log.warn("解除锁定失败:{}", "......", e);
    } finally {
        lockHolder.remove();
    }
    return false;
}
```
**锁前缀：**
```java
public String lockKeyPrefix(){
    return "a:b:c:";
}
```