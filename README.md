# sailthru-php5-client

[![Build Status](https://travis-ci.org/sailthru/sailthru-php5-client.svg?branch=master)](https://travis-ci.org/sailthru/sailthru-php5-client)
[![Coverage Status](https://coveralls.io/repos/github/sailthru/sailthru-php5-client/badge.svg?branch=master)](https://coveralls.io/github/sailthru/sailthru-php5-client?branch=master)


## About
A simple client library to remotely access the `Sailthru REST API`. By default, it will make requests in `JSON` format.

### Documentation

[REST API Documentation](https://getstarted.sailthru.com/developers/api-basics/introduction/)
[PHP5 Client Documentation](https://getstarted.sailthru.com/developers/api-client/php5/)

###  Installation

You can clone via GitHub or install via composer.
```shell
git clone git@github.com:sailthru/sailthru-php5-client.git
composer require sailthru/sailthru-php5-client
```

## Examples 

### Default initialization
For basic usage, you can initialize with just API Key and Secret
```php
    $client = new Sailthru_Client($api_key, $api_secret);
```

### Exception-handling
As of 2.0.0, the client library will throw a `Sailthru_Client_Exception` on API and IO errors, which should be properly handled. 

Error codes 1000, 1001, 10002 are client and IO-related, while 0-99 and XX are API errors. [Learn more about our API Errors](https://getstarted.sailthru.com/developers/api-basics/responses/).
```php
try { 
    $client->apiPost('user', [..]);
} catch (Sailthru_Client_Exception $e) {
    $code = $e->getCode();
    $message = $->getMessage();
    // process error
}
```

### Optional parameters for connection/read timeout settings
Increase timeout from 10 (default) to 30 seconds.
```php
$client = new Sailthru_Client($this->api_key, $this->secret, $this->api_url, array('timeout' => 30000, 'connect_timeout' => 30000));
```

### API Rate Limiting

Here is an example how to check rate limiting and throttle API calls based on that. For more information about Rate Limiting, see [Sailthru Documentation](https://getstarted.sailthru.com/new-for-developers-overview/api/api-technical-details/#Rate_Limiting)

```php
// get last rate limit info
$rate_limit_info = $sailthru_client->getLastRateLimitInfo("user", "POST");

// getRateLimitInfo returns null if given endpoint/method wasn't triggered previously
if ($rate_limit_info) {
    $limit = $rate_limit_info['limit'];
    $remaining = $rate_limit_info['remaining'];
    $reset_timestamp = $rate_limit_info['reset'];

    // throttle api calls based on last rate limit info
    if ($remaining <= 0) {
        $seconds_till_reset = $reset_timestamp - time();

        // sleep or perform other business logic before next user api call
        sleep($seconds_till_reset);
    }
}
```

## Tests

You can run the tests locally with composer and phpunit:

```shell
cd sailthru-php5-client
composer install
vendor/bin/phpunit
```


