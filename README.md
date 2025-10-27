# Nextcloud talk notification channel

This package makes it easy to send notifications using [NextcloudTalk](https://nextcloud.com/) with Laravel 10.x, 11.x and 12.x.

## Contents

-   [Installation](#installation)
    -   [Setting up the NextcloudTalk service](#setting-up-the-NextcloudTalk-service)
-   [Usage](#usage)
    -   [Available Message methods](#available-message-methods)
-   [Changelog](#changelog)
-   [Testing](#testing)
-   [Security](#security)
-   [Contributing](#contributing)
-   [Credits](#credits)
-   [License](#license)

## Installation

You can install this package via composer:

```bash
composer require molnix/nextcloud-talk
```

### Setting up the NextcloudTalk service

Add your NextcloudTalk config to config/services.php:

```php
// config/services.php
...
    'nextcloudtalk' => [
        'url' => env('NEXTCLOUD_URL'),
        'username' => env('NEXTCLOUD_USERNAME'),
        'password' => env('NEXTCLOUD_PASSWORD'),
        'default_channel' => env('NEXTCLOUD_DEFAULT_CHANNEL'),
        'one_to_one_channel_name' => env('NEXTCLOUD_ONE_TO_ONE_CHANNEL_NAME'),
    ],
...
```

## Usage

You can use the channel in your via() method inside the notification:

```php
use Illuminate\Notifications\Notification;
use Molnix\Channels\NextcloudTalkChannel;
use Molnix\Channels\NextcloudTalkMessage;

class NextcloudNotification extends Notification
{

    public function via(object $notifiable): array
    {
        return [NextcloudTalkChannel::class];
    }

    public function toNextcloudTalk()
    {
        return NextcloudTalkMessage::create('Hello');
    }

}
```

If you want the bot to make a channel with the user and send message (one to one), add `routeNotificationForNextcloudTalk` to your notifiable model:

```php
public function routeNotificationForNextcloudTalk($notification): string
    {
        return $this->username;
    }
```

Channel selection order:

1. Channel specified using `NextcloudTalkMessage::to()`.
2. `routeNotificationForNextcloudTalk()`
3. `default_channel` specified in the config

### Available Message methods

`to()`: To specify a nextcloud channel token, example groups etc.

`content()`: Sets a content of the notification message.

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Testing

```bash
$ composer test
```

## Security

If you discover any security related issues, please email vishnu@koothattu.com instead of using the issue tracker.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## Credits

-   [Vishnu Koothattu](https://github.com/vish404)
-   [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
