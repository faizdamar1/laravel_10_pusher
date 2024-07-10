## .env 
- BROADCAST_DRIVER=pusher
- PUSHER_APP_ID=
- PUSHER_APP_KEY=
- PUSHER_APP_SECRET=
- PUSHER_APP_CLUSTER=

## AuthServiceProvider.php
```php
Auth::viaRequest('custom-auth', function ($request) {
    // Any custom user-lookup logic here. For example:
    if ($request->token === 'randomtoken') {

        $user = new User();
        $user->id = 1;
        $user->email = 'admin@mail.com';
        $user->name = 'admin';
        $user->token = 'randomtoken';
        return $user;
    }

    Log::debug("broadcast auth failed");
    return null;
});
```

### auth.php
```php
'guards' => [
    'broadcasting' => [
        'driver' => 'custom-auth',
        'provider' => 'users'
    ],
],
```

## BroadcastServiceProvider.php
```php
//comment Broadcast::routes();
Broadcast::routes(['middleware' => ['auth:broadcasting']]);
```
