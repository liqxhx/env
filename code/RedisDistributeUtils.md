```java

import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import java.util.concurrent.TimeUnit;

/**
 * <p> 类描述: Redis Distribution Lock Util
 *
 * @author liqxhx
 * @version 1.0
 * @date 2019/03/13 9:06
 * @since 2019/03/13 9:06
 */
@Component
@Slf4j
public class RedisDistributeLockUtils {
    private static final long LOCK_TIMEOUT_MS = 5000;

    @Autowired
    private RedisTemplate redisTemplate;

    public void lock(String lockName, DoInLockCallback callback) {


        if(redisTemplate.opsForValue().setIfAbsent(lockName, System.currentTimeMillis()+LOCK_TIMEOUT_MS)) {
            log.debug("{} try lock success. {}", Thread.currentThread().getName(), lockName);
            redisTemplate.expire(lockName, LOCK_TIMEOUT_MS, TimeUnit.MILLISECONDS);

            callback.invoke();

            redisTemplate.delete(lockName);
            log.debug("{} release lock {}", Thread.currentThread().getName(), lockName);
        } else {
            log.debug("{} try lock fail {}", Thread.currentThread().getName(), lockName);
            String timestampStr = String.valueOf(redisTemplate.opsForValue().get(lockName));

            if(StringUtils.isBlank(timestampStr) || System.currentTimeMillis() > Long.parseLong(timestampStr)) {

                String timestampStrAgain = String.valueOf(redisTemplate.opsForValue().getAndSet(lockName, System.currentTimeMillis()+LOCK_TIMEOUT_MS));

                if(timestampStrAgain == null || StringUtils.equals(timestampStr, timestampStrAgain)) {
                    log.info("{} retry lock success {}", Thread.currentThread().getName(), lockName);

                    redisTemplate.expire(lockName, LOCK_TIMEOUT_MS, TimeUnit.MILLISECONDS);

                    callback.invoke();

                    redisTemplate.delete(lockName);
                    log.info("{} lock released. {}", Thread.currentThread().getName(), lockName);

                }else{
                    log.info("{} fail to retry lock {}", Thread.currentThread().getName(), lockName);
                }

            } else {
                log.info("{} lock still in use {}", Thread.currentThread().getName(), lockName);
            }
        }
    }

    @FunctionalInterface
    public interface DoInLockCallback {
        public void invoke();
    }

}
```
