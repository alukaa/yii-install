### yii 安装过程
- 仅以 advance 为例,basic差不多

#### 新建项目

- advance
```php
composer create-project --prefer-dist yiisoft/yii2-app-advanced yii2-app
```

- basic
```php
composer create-project --prefer-dist --stability=dev yiisoft/yii2-app-basic basic
```

##$# 问题

- 执行 ```composer install```的时候很慢,使用```composer -vvv```查看的时候包下载指向了github

#### 解决方法
根据官方描述 ```https://www.yiiframework.com/doc/guide/2.0/zh-cn/start-installation```在```composer.json```添加
```php
"replace": {
    "bower-asset/jquery": ">=1.11.0",
    "bower-asset/inputmask": ">=3.2.0",
    "bower-asset/punycode": ">=1.3.0",
    "bower-asset/yii2-pjax": ">=2.0.0", //前面四个来自https://github.com/yiisoft/yii2-framework/blob/master/composer.json
    "bower-asset/bootstrap": ">=3.4.0" //来自https://github.com/yiisoft/yii2-composer/blob/master/composer.json
}
```

同时修改config和repositories
```php
 "config": {
        "process-timeout": 1800,
        "fxp-asset": {
            "enabled": false
        },
        "preferred-install": "dist",
        "optimize-autoloader": true
    },
    "repositories": {
        "packagist": {
            "type": "composer",
            "url": "https://mirrors.aliyun.com/composer/"
        }
    }
```
阿里云提供了composer镜像，里面的操作应当进行一遍```https://developer.aliyun.com/composer```

#### 最后
1. 复制```https://github.com/yiisoft/yii2-composer``` 里面的```package.json```,把上面的relace包加上,最后见
```composer.json``` 和```package.json```
2. 修改 ```common/config/main.php```如下
```php
<?php
return [
    'aliases' => [
        '@bower' => dirname(dirname(__DIR__)) . '/node_modules',
        '@npm'   => dirname(dirname(__DIR__)) . '/node_modules',
    ],
    'vendorPath' => dirname(dirname(__DIR__)) . '/vendor',
    'components' => [
        'cache' => [
            'class' => 'yii\caching\FileCache',
        ],
    ],
];

```
3. ```package.json```里面的包可以在docker另外的容器安装，不管什么方式，安装好即可
4. ```composer install```，速度快非常多
