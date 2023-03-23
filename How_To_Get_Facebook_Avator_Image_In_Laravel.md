# How to get user avatar image from facebook by token in Laravel

```php
$facebookUser = Socialite::driver('facebook')->stateless()->user();
$avator = $facebookUser->getAvator($facebookUser);

function getAvator($facebookUser) {
    $url = $facebookUser->getAvatar() . '&access_token=' . $facebookUser->token;
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
    curl_exec($ch);

    return curl_getinfo($ch, CURLINFO_EFFECTIVE_URL);
}
```
