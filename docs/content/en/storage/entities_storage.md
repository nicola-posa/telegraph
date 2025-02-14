---
title: 'Telegraph Entities Storage'
menuTitle: 'Entities Storage'
description: ''
category: 'Storage'
fullscreen: false 
position: 70
---

**Telegraph** offers a quick, multi driver, data storage solution to save and retrieve information about Bots, Chats and Users. A fine grained configuration is available in the `storage` section of Telegraph [configuration file](installation#configuration) 

## Overview

### Bot Storage

[TelegraphBots](models/telegraph-bot) data storage is available through its `->storage()` method  

```php
/** @var \DefStudio\Telegraph\Models\TelegraphBot $telegraphBot */
$telegraphBot->storage()->set('last_chat', '1254824');

$lastChatId = $telegraphBot->storage()->get('last_chat'); // 1254824

$telegraphBot->forget('last_chat') //stored data is deleted
```

### Chat Storage

[TelegraphChats](models/telegraph-chat) data storage is available through its `->storage()` method  

```php
/** @var \DefStudio\Telegraph\Models\TelegraphChat $telegraphChat */
$telegraphChat->storage()->set('last_message', 'hello!');

$lastMessage = $telegraphChat->storage()->get('last_message'); // 1254824

$lastMessage->forget('last_message') //stored data is deleted
```

### User DTO Storage

[User DTO](webhooks/dto#defstudio-telegraph-dto-user) data storage is available through its `->storage()` method  

```php
/** @var \App\Models\User $user */
/** @var \DefStudio\Telegraph\DTO\User $telegraphUser */
$telegraphUser->storage()->set('laravel_user_id', $user->id);

$userId = $telegraphUser->storage()->get('laravel_user_id'); // 99

$userId->forget('laravel_user_id') //stored data is deleted
```

## Using different Storage drivers

Depending on the application needs, multiple drivers can be [configured](installation#configuration) and used. 

### Default driver

A default storage driver can be set in `telegraph.storage.default` config and it will be used when `->storage()` is invoked without arguments:

```php
// /config/telegraph.php

return [
    // ...
    
    'storage' => [
        'default' => 'file'
    ]
]
```

```php
$telegraphUser->storage() // \DefStudio\Telegraph\Storage\FileStorageDriver
```

### File Storage Driver

A file storage driver saves data in json files inside the disk/folder defined in Telegraph [configuration file](installation#configuration)

```php
// /config/telegraph.php

return [
    // ...
    
    'storage' => [
        'stores' => [
            'file' => [
                'driver' => \DefStudio\Telegraph\Storage\FileStorageDriver::class,
                'disk' => 'local',
                'root' => 'telegraph',
            ]
        ]       
    ]
]
```

with the above configuration this code will store the `name`
value in a `/[laravel local disk path]/telegraph/User/<telegraph_user_id>.json` file

```php
$telegraphUser->storage('file')->put('name', 'Daniele');
```


### Cache Storage Driver

A cache storage driver saves data using Laravel Cache system following Telegraph [configuration file](installation#configuration)

```php
// /config/telegraph.php

return [
    // ...
    
    'storage' => [
        'stores' => [
            'cache' => [
                'driver' => \DefStudio\Telegraph\Storage\CacheStorageDriver::class,
                'store' => 'redis',
                'key_prefix' => 'tgph',
            ]
        ]       
    ]
]
```

with the above configuration this code will store the `name`
value inside Laravel Redis Cache

```php
$telegraphUser->storage('cache')->put('name', 'Daniele');
```
