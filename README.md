
This package makes it easy to send notifications using Raygan Sms with Laravel 5.3+.

# RayganSms notifications channel for Laravel 5.3+

[![Latest Version on Packagist](https://img.shields.io/packagist/v/trez/raygan-sms-notification-channel.svg?style=flat-square)](https://packagist.org/packages/trez/raygan-sms-notification-channel)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)
[![Build Status](https://img.shields.io/travis/trez/raygan-sms-notification-channel/master.svg?style=flat-square)](https://travis-ci.org/trez/raygan-sms-notification-channel)
[![StyleCI](https://styleci.io/repos/65589451/shield)](https://styleci.io/repos/65589451)
[![SensioLabsInsight](https://img.shields.io/sensiolabs/i/aceefe27-ba5a-49d7-9064-bc3abea0abeb.svg?style=flat-square)](https://insight.sensiolabs.com/projects/aceefe27-ba5a-49d7-9064-bc3abea0abeb)
[![Quality Score](https://img.shields.io/scrutinizer/g/trez/raygan-sms-notification-channel.svg?style=flat-square)](https://scrutinizer-ci.com/g/trez/raygan-sms-notification-channel)
[![Code Coverage](https://img.shields.io/scrutinizer/coverage/g/trez/raygan-sms-notification-channel/master.svg?style=flat-square)](https://scrutinizer-ci.com/g/trez/raygan-sms-notification-channel/?branch=master)
[![Total Downloads](https://img.shields.io/packagist/dt/trez/raygan-sms-notification-channel.svg?style=flat-square)](https://packagist.org/packages/trez/raygan-sms-notification-channel)
<div dir="rtl">
این پکیج امکان ارسال اعلانات (notifications) با استفاده از RayganSms API  را فراهم می کند.

## محتوا

- [Installation](#installation)
    - [نصب و پیکره بندی سرویس](#setting-up-the-RayganSms-service)
- [نحوه استفاده](#usage)
    - [نحوه استفاده از سرویس در ارسال](#available-message-methods)

## Installation

با استفاده از composer  قادر به نصب این سرویس می باشید:
</div>

```bash
composer require trez/raygan-sms-notification-channel
```
<div dir="rtl">
چنانچه شما از نسخه پایین تر از 5.5 لاراول استفاده می نمایید serviseprovider  زیر را به پروژه خود اضافه نمایید:
</div>

```php
// config/app.php
'providers' => [
    ...
    NotificationChannels\SmscRu\SmscRuServiceProvider::class,
],
```

### Setting up the SmscRu service

Add your SmscRu login, secret key (hashed password) and default sender name (or phone number) to your `config/services.php`:

```php
// config/services.php
...
'smscru' => [
    'login'  => env('SMSCRU_LOGIN'),
    'secret' => env('SMSCRU_SECRET'),
    'sender' => 'John_Doe'
],
...
```

> If you want use other host than `smsc.ru`, you MUST set custom host WITH trailing slash.

```
// .env
...
SMSCRU_HOST=http://www1.smsc.kz/
...
```

```php
// config/services.php
...
'smscru' => [
    ...
    'host' => env('SMSCRU_HOST'),
    ...
],
...
```
<div dir="rtl">
## نحوه استفاده

با استفاده از متد `via()` این کانال را به notefication  خود اضاقه نمایید:
</div>

```php
use Illuminate\Notifications\Notification;
use NotificationChannels\SmscRu\SmscRuMessage;
use NotificationChannels\SmscRu\SmscRuChannel;

class AccountApproved extends Notification
{
    public function via($notifiable)
    {
        return [TelegramChannel::class];
    }

    public function toRayganSms($notifiable)
    {
        return (new RayganSmsMessage())
            ->content("Task #{$notifiable->id} is complete!");
    }
}
```
<div dir="rtl">
 متد `routeNotificationForSmscru()` جهت اطمینان در دریافت شماره تملس کاربر اضافه می نماییم : مورد نظر خود متد 
</div>

```php
public function routeNotificationForSmscru()
{
    return $this->phone;
}
```
<div dir="rtl">
### متدها

`content()`: متن ارسالی به دریافت کننده.
</div>

## Credits

- [Farhad Mirzapour](https://github.com/farhadmirzapour)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

