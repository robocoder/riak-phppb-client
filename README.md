# Riak PB Client for PHP

* [![Packagist](https://img.shields.io/packagist/v/basho/riak-pb.svg?maxAge=2592000)](https://packagist.org/packages/basho/riak-pb)
* Build status: [![Build Status](https://secure.travis-ci.org/basho/riak-phppb-client.png?branch=master)](http://travis-ci.org/basho/riak-phppb-client)

**Riak PHP PB Client** is a client which makes it easy to communicate with [Riak](http://basho.com/riak/), an open source, distributed database that focuses on high availability, horizontal scalability, and *predictable* latency. Both Riak and this code is maintained by [Basho](http://www.basho.com/).

To see other clients available for use with Riak visit our [Documentation Site](http://docs.basho.com/riak/latest/dev/using/libraries)

## Installation

### Dependencies

This library requires PHP 5.4+ and allegro/protobuf PHP extension (instructions below).

### Composer Install

Run the following `composer` command:

```console
$ composer require "basho/riak-pb": "1.2.*"
```

Alternately, manually add the following to your `composer.json`, in the `require` section:

```javascript
"require": {
    "basho/riak-pb": "1.2.*"
}
```

And then run `composer update` to ensure the module is installed and all dependencies retrieved.

Next, you need to run the following command to install the needed extension for PB support:

```sh
make install-protobuf
```

Finally, you need to make sure the newly installed extension is being included with your PHP environment via `php.ini`
or a php configuration file (`*.conf`).

```text
extension=protobuf.so
```

You can confirm that it is enabled for the command line environment with the following command.

```sh
php -m | fgrep protobuf
```

## Documentation

A fully traversable version of the API documentation for this library can be found on [Github Pages](http://basho.github.io/riak-phppb-client).

### Example Usage
Below is a short example of using the client. More substantial sample code is available [in examples](/examples).
```php
// lib classes are included via the Composer autoloader files
use Basho\Riak;
use Basho\Riak\Api\Pb;
use Basho\Riak\Node;
use Basho\Riak\Command;

// define the connection info to our Riak nodes
$nodes = (new Node\Builder)
    ->onPort(10018)
    ->buildCluster(['riak1.company.com', 'riak2.company.com', 'riak3.company.com',]);

// instantiate the Riak client
$riak = new Riak($nodes, [], new Pb());

// build a command to be executed against Riak
$command = (new Command\Builder\StoreObject($riak))
    ->buildObject('some_data')
    ->buildBucket('users')
    ->build();

// Receive a response object
$response = $command->execute($command);

// Retrieve the Location of our newly stored object from the Response object
$object_location = $response->getLocation();
```

## Contributing

This repo's maintainers are engineers at Basho and we welcome your contribution to the project! You can start by reviewing [CONTRIBUTING.md](CONTRIBUTING.md) for information on everything from testing to coding standards.

### An honest disclaimer

Due to our obsession with stability and our rich ecosystem of users, community updates on this repo may take a little longer to review.

The most helpful way to contribute is by reporting your experience through issues. Issues may not be updated while we review internally, but they're still incredibly appreciated.

Thank you for being part of the community! We love you for it.

## License and Authors

* Author: Christopher Mancini (https://github.com/christophermancini)
* Author: Alex Moore (https://github.com/alexmoore)
* Author: Luke Bakken (https://github.com/lukebakken)

Copyright (c) 2015 Basho Technologies, Inc. Licensed under the Apache License, Version 2.0 (the "License"). For more details, see [License](License).
