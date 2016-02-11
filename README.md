# Twitter plugin

## Installation

### Using Composer

Ensure `require` is present in `composer.json`.

```json
{
    "require": {
        "cvo-technologies/twitter": "0.1.*"
    }
}
```

## Usage

If you want to get information about a specific repository

### Webservice config

Add the following to the ```Webservice``` section of your application config.

```
        'twitter' => [
            'className' => 'Muffin\Webservice\Connection',
            'service' => 'CvoTechnologies\Twitter\Webservice\Driver\Twitter',
            'consumerKey' => '',
            'consumerSecret' => ''
        ]
```

### Controller

```php
<?php

namespace App\Controller;

use Cake\Event\Event;

class StatusesController extends AppController
{

    public function beforeFilter(Event $event)
    {
        $this->loadModel('CvoTechnologies/Twitter.Statuses', 'Endpoint');
    }

    public function index()
    {
        $statuses = $this->Statuses->find()->conditions([
            'screen_name' => 'CakePHP',
        ]);

        $this->set('statuses', $statuses);
    }
}
```

### Streaming example

This is an example of how to implement the Twitter streaming API.

```php
<?php

namespace App\Shell;

use Cake\Console\Shell;

class StreamShell extends Shell
{

    public function beforeFilter(Event $event)
    {
        $this->loadModel('CvoTechnologies/Twitter.Statuses', 'Endpoint');
    }

    public function main()
    {
        $statuses = $this->Statuses
            ->find('filter')
            ->where([
                'word' => 'twitter',
            ]);

        foreach ($statuses as $status) {
            echo $status->text . PHP_EOL;
        }
    }
}
```
