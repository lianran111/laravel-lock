# 环境要求
    
    1.PHP >= 7
    2.composer
    3.redis(必须支持lua)
    4.predis
    
# composer 安装

移步 [composer中文网](https://www.phpcomposer.com/).
# redis 安装

[redis中文网](http://www.redis.net.cn/)

# predis 安装
    composer require predis/predis
# lock 安装     
    第一步, 安装alravel-lock
    composer require lianran111/laravel-lock
    第二步, 生成配置文件
    php artisan vendor:publish --provider="Lock\LockServiceProvider"
# 抢占锁
## lock(callable $callback, string $lock_val)
多进程并发时, 其中某一个进程得到锁后, 其他进程将被拒绝
    
    
    $callback  
                回调函数, 可返回值
    $lock_val
                锁定值
# 多参数抢占锁
## lock(callable $callback, array $lock_vals)
多进程并发时, 其中某一个进程得到锁后, 其他进程将被拒绝
    
    
    $callback  
                回调函数, 可返回值
    $lock_vals
                锁定值(数组)
        
       
# 队列锁

## queueLock($closure, $lock_val, $max_queue_process = 100) 
多进程并发时, 其中某一个进程得到锁后, 其他进程将等待解锁(配置最大等待进程后, 超过等待数量后进程将被拒绝)

    $callback  
                    回调函数, 可返回值
    $lock_val
                    锁定值
    $max_queue_process        
                    队列最大等待进程        
                    
# 多参数队列锁

## queueLock($closure, $lock_vals, $max_queue_process = 100) 
多进程并发时, 其中某一个进程得到锁后, 其他进程将等待解锁(配置最大等待进程后, 超过等待数量后进程将被拒绝)

    $callback  
                    回调函数, 可返回值
    $lock_vals
                    锁定值(数组)
    $max_queue_process        
                    队列最大等待进程       

# 使用
    
    //静态调用
    $lock_val = 'user:pay:1';
    Lock::lock(function($redis){
       echo 'hello world!';
    }, $lock_val);
            
    //实例化调用
    $lock = new Lock();
    $lock_val = 'user:pay:1';
    $lock->lock(function($redis){
        echo 'hello world!';
    }, $lock_val);
    
    //多参数锁
    $lock = new Lock();
    $lock_val[] = 'user:pay:1';
    $lock_val[] = 'user:pay:2';
    $lock->lock(function($redis){
        echo 'hello world!';
    }, $lock_val);
    
    
# 限流

## isActionAllowed($key, $period, $max_count)
    
    $key        限制key
    $period     限制时间(秒)
    $max_count  限制时间内最大数量
    
# config配置

     /*
        |--------------------------------------------------------------------------
        | lock配置文件
        |--------------------------------------------------------------------------
        |
        |drive 锁驱动(默认redis)
        |
        |redis redis驱动配置
        |   host 地址
        |   port 端口
        |
        |params 参数配置
        |   max_queue_process  进程池最大进程
        |   expiration         锁值过期时间
        |
        */
    'drive' =>  'redis',

    'redis' =>  [
        'host'                  =>  '127.0.0.1',
        'port'                  =>  6379,
        'read_write_timeout'    =>  0,
        'persistent'            =>  true,
    ],

    'params' => [
        'max_queue_process' =>  100,
        'expiration'        =>  5
    ]
# laravel-lock
