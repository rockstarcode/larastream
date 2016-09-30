# RockstarCode StreamLine (previously 82rules/streamline)

## A simple alternative to 3rd party push services and realtime libraries.

### Requirements
	* PHP ^5.6
	* Redis
	* Apache/Nginx

### Features

This PHP libraries adds a simple interface to create realtime apps using Redis Pub/Sub as the distribution engine.

It allows for User -> Users and Server -> User(s) communication streams
Allowing features such as

	* Message Notifications
	* Chat
	* Status Notifications
	* Dynamic Content Push
   	* Dead simple integration
	* Long Polling Javascript included
	* Auto reconnects on dropped connections
	* No need for external services



### Usage

``` composer require rockstarcode/streamline ```

#### Laravel
```
// routes file

Route::get('/subscribe/{channel}',function($channel){
    use RockstarCode\StreamLine\Subscribe;

    $subscribe = new Subscribe($channel, function(){ /// optional custom connection
        $client = new Redis();
        $client->connect('127.0.0.1');
        return $client;
    });

    $subscribe->stream(function($message){ /// function that handles each message as it's received in the channel
        echo json_encode(['status'=>true,'message'=>$message]);
    });
});

Route::post('/subscribe/{channel}',function($channel, Request $request){
    use RockstarCode\StreamLine\Publish;

    $pub = new Publish($channel);

    $subscribe->send($request->only('message'));
});
```

#### Included Example
Included in this repo you will find a very simple chat example in ./example
```
    ./example
        - .htaccess
        - frontend.php
        - index.php
```

The example expects for you to use apache or nginx and configure a virtualhost with a documentroot pointed to the examples path

```
    <VirtualHost *:80>
            ServerName myexample.app (or whichever domain you choose)
            DocumentRoot /path/to/streamline/example
    </VirtualHost>
```

simply navigate to http://<your host>/ on two different browser tabs, you should be able to enter a name in each tab and send messages between each window.



### Common Issues

#### Blocking Requests
    If you are using Laravel's Valet, or PHP's built in web server, they do not handle concurrent connections well and block open streams. The only way to test this app is using a web server
    capable of threading PHP executions.

#### Questions/Comments/Commits

This is an ongoing project, I welcome feedback and contribution. If you have any questions feel free to email me at rulian.estivalletti@gmail.com



