---
title: PHP-Laravel核心思想
abbrlink: f0d02832
date: 2020-12-21 11:32:54
tags:
---
控制反转（Inversion of Control，缩写为IoC），是面向对象编程中的一种设计原则，可以用来减低计算机代码之间的耦合度。其中最常见的方式叫做依赖注入（Dependency Injection，简称DI），还有一种方式叫“依赖查找”（Dependency Lookup）。通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体，将其所依赖的对象的引用传递(注入)给它。
<!-- more -->

## index.php

### 容器核心思想
bind – 每次通过make实例化新的对象  
singleton – 注册单例, 加载后注册至 instances 中  
instance – 通过已经实例化的对象注册单例  

通过反射将所有的依赖注类递归进行实例化

### bootstrap/autoload.php

> 类自动加载

```
<!-- 核心函数 -->
spl_autoload_register
```

### bootstrap/app.php

> 实例化 Illuminate\Foundation\Application 类

1. 设置应用程序的基础路径
```
$this->basePath = rtrim($basePath, '\/');
$this->instance('path', $this->path());
$this->instance('path.base', $this->basePath());
$this->instance('path.lang', $this->langPath());
$this->instance('path.config', $this->configPath());
$this->instance('path.public', $this->publicPath());
$this->instance('path.storage', $this->storagePath());
$this->instance('path.database', $this->databasePath());
$this->instance('path.resources', $this->resourcePath());
$this->instance('path.bootstrap', $this->bootstrapPath());
```
2. 设置应用程序的基础绑定
```
static::setInstance($this);
$this->instance('app', $this);
$this->instance(Container::class, $this);
```
3. 注册基础服务提供商 & 绑定基础依赖
```
$this->register(new EventServiceProvider($this));
$this->register(new LogServiceProvider($this));
$this->register(new RoutingServiceProvider($this));

// 注册基础服务提供商
$this->serviceProviders = [
    '\Illuminate\Events\EventServiceProvider',
    '\Illuminate\Log\LogServiceProvider',
    '\Illuminate\Routing\RoutingServiceProvider',
]

// 绑定基础依赖
$this->bindings = [
	'events',
	'log',
	'router',
	'url',
	'redirect',
	'Psr\Http\Message\ServerRequestInterface',
	'Psr\Http\Message\ResponseInterface',
	'Illuminate\Contracts\Routing\ResponseFactory',
]
```

4. 注册服务别名
```
$this->aliases = [
    Illuminate\Foundation\Application => "app",
    Illuminate\Contracts\Container\Container => "app",
    Illuminate\Contracts\Foundation\Application => "app",
    Illuminate\Auth\AuthManager => "auth",
    Illuminate\Contracts\Auth\Factory => "auth",
    Illuminate\Contracts\Auth\Guard => "auth.driver",
    Illuminate\View\Compilers\BladeCompiler => "blade.compiler",
    Illuminate\Cache\CacheManager => "cache",
    Illuminate\Contracts\Cache\Factory => "cache",
    Illuminate\Cache\Repository => "cache.store",
    Illuminate\Contracts\Cache\Repository => "cache.store",
    Illuminate\Config\Repository => "config",
    Illuminate\Contracts\Config\Repository => "config",
    Illuminate\Cookie\CookieJar => "cookie",
    Illuminate\Contracts\Cookie\Factory => "cookie",
    Illuminate\Contracts\Cookie\QueueingFactory => "cookie",
    Illuminate\Encryption\Encrypter => "encrypter",
    Illuminate\Contracts\Encryption\Encrypter => "encrypter",
    Illuminate\Database\DatabaseManager => "db",
    Illuminate\Database\Connection => "db.connection",
    Illuminate\Database\ConnectionInterface => "db.connection",
    Illuminate\Events\Dispatcher => "events",
    Illuminate\Contracts\Events\Dispatcher => "events",
    Illuminate\Filesystem\Filesystem => "files",
    Illuminate\Filesystem\FilesystemManager => "filesystem",
    Illuminate\Contracts\Filesystem\Factory => "filesystem",
    Illuminate\Contracts\Filesystem\Filesystem => "filesystem.disk",
    Illuminate\Contracts\Filesystem\Cloud => "filesystem.cloud",
    Illuminate\Contracts\Hashing\Hasher => "hash",
    Illuminate\Translation\Translator => "translator",
    Illuminate\Contracts\Translation\Translator => "translator",
    Illuminate\Log\Writer => "log",
    Illuminate\Contracts\Logging\Log => "log",
    Psr\Log\LoggerInterface => "log",
    Illuminate\Mail\Mailer => "mailer",
    Illuminate\Contracts\Mail\Mailer => "mailer",
    Illuminate\Contracts\Mail\MailQueue => "mailer",
    Illuminate\Auth\Passwords\PasswordBrokerManager => "auth.password",
    Illuminate\Contracts\Auth\PasswordBrokerFactory => "auth.password",
    Illuminate\Auth\Passwords\PasswordBroker => "auth.password.broker",
    Illuminate\Contracts\Auth\PasswordBroker => "auth.password.broker",
    Illuminate\Queue\QueueManager => "queue",
    Illuminate\Contracts\Queue\Factory => "queue",
    Illuminate\Contracts\Queue\Monitor => "queue",
    Illuminate\Contracts\Queue\Queue => "queue.connection",
    Illuminate\Queue\Failed\FailedJobProviderInterface => "queue.failer",
    Illuminate\Routing\Redirector => "redirect",
    Illuminate\Redis\RedisManager => "redis",
    Illuminate\Contracts\Redis\Factory => "redis",
    Illuminate\Http\Request => "request",
    Symfony\Component\HttpFoundation\Request => "request",
    Illuminate\Routing\Router => "router",
    Illuminate\Contracts\Routing\Registrar => "router",
    Illuminate\Contracts\Routing\BindingRegistrar => "router",
    Illuminate\Session\SessionManager => "session",
    Illuminate\Session\Store => "session.store",
    Illuminate\Contracts\Session\Session => "session.store",
    Illuminate\Routing\UrlGenerator => "url",
    Illuminate\Contracts\Routing\UrlGenerator => "url",
    Illuminate\Validation\Factory => "validator",
    Illuminate\Contracts\Validation\Factory => "validator",
    Illuminate\View\Factory => "view",
    Illuminate\Contracts\View\Factory => "view",
]
```
5. 继续绑定核心依赖
```
$this->bindings = [
	'events',
	'log',
	'router',
	'url',
	'redirect',
	'Psr\Http\Message\ServerRequestInterface',
	'Psr\Http\Message\ResponseInterface',
	'Illuminate\Contracts\Routing\ResponseFactory',

	'Illuminate\Contracts\Http\Kernel',
	'Illuminate\Contracts\Console\Kernel',
	'Illuminate\Contracts\Debug\ExceptionHandler',
]
```

6. 实例化App\Http\Kernel
```
// 设置路由中间件(优先)
middlewarePriority = [
	"Illuminate\Session\Middleware\StartSession",
	"Illuminate\View\Middleware\ShareErrorsFromSession",
	"Illuminate\Auth\Middleware\Authenticate",
	"Illuminate\Session\Middleware\AuthenticateSession",
	"Illuminate\Routing\Middleware\SubstituteBindings",
	"Illuminate\Auth\Middleware\Authorize",
]

// 设置路由中间件分组
middlewareGroups = [
	web = [
		"App\Http\Middleware\EncryptCookies",
		"Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse",
		"Illuminate\Session\Middleware\StartSession",
		"Illuminate\View\Middleware\ShareErrorsFromSession",
		"App\Http\Middleware\VerifyCsrfToken",
		"Illuminate\Routing\Middleware\SubstituteBindings",
	]
	api = [
		"throttle:60,1",
		"bindings",
	]
]

// 设置中间件
middleware = [
	"Illuminate\Auth\Middleware\Authenticate",
	"Illuminate\Auth\Middleware\AuthenticateWithBasicAuth",
	"Illuminate\Routing\Middleware\SubstituteBindings",
	"Illuminate\Auth\Middleware\Authorize",
	"App\Http\Middleware\RedirectIfAuthenticated",
	"Illuminate\Routing\Middleware\ThrottleRequests",
	"Laravel\Passport\Http\Middleware\CheckScopes",
	"Laravel\Passport\Http\Middleware\CheckForAnyScope",
]

```

1. 开启对_method请求参数的支持  
2. $request是经过Symfony封装的请求对象 
3. 注册request实例到容器 ($app[‘request’]->Illuminate\Http\Request)
4. 清空之前容器中的request实例
5. 调用bootstrap方法，启动一系列启动类的bootstrap方法：
6. 
```
    - Illuminate\Foundation\Bootstrap\DetectEnvironment 环境配置（$app[‘env’]）
    - Illuminate\Foundation\Bootstrap\LoadConfiguration  基本配置（$app[‘config’]）
    - Illuminate\Foundation\Bootstrap\ConfigureLogging   日志文件（$app[‘log’]）
    - Illuminate\Foundation\Bootstrap\HandleExceptions   错误&异常处理
    - Illuminate\Foundation\Bootstrap\RegisterFacades    清除已解析的Facade并重新启动，注册config文件中alias定义的所有Facade类到容器
    - Illuminate\Foundation\Bootstrap\RegisterProviders  注册config中providers定义的所有Providers类到容器
    - Illuminate\Foundation\Bootstrap\BootProviders      调用所有已注册Providers的boot方法
```

7. 通过Pipeline发送请求，经过中间件，再由路由转发，最终返回响应