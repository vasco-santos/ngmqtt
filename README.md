# Angular MQTT

Angular service to handle an MQTT connection in multiple controllers. It is used the Observer pattern in this implementation, in order to allow the use of the same connection in multiple controllers, each one with its own subscribed topics.

Particularly, this provider is tremendous useful for combining notifications in a navbar, as well as in multiple controllers, without establishing new connections over the time.

It is used the [mqtt.js](https://github.com/mqttjs/MQTT.js) with browserify for browser (browserMqtt.js) for the JS MQTT implementation.

## Installation

------------
1. Download the package:
   1. download using npm: `npm install ngmqtt`
   1. download using bower: `bower install ngmqtt`
1. Modify your application to include `ngmqtt` in your application dependencies.

Example:

'
var app = angular.module('<your-app>', ['ngmqtt']).run(function(){

	//Angular application started
});


/*
Test Controller 1
*/
app.controller('testController1', ['$scope', '$interval', 'ngmqtt', function($scope, $interval, ngmqtt){

	var options = {
            clientId: "test",
            protocolId: 'MQTT',
            protocolVersion: 4
        };
	ngmqtt.connect('ws://127.0.0.1:8888', options);
	
	ngmqtt.listenConnection("testController1", function(){
		console.log("connected");
		ngmqtt.subscribe('topic4test');

		var i = 0;
		$interval(function() {
			ngmqtt.publish('topic4test', i.toString());
			i++;
		}, 300);
	});

	ngmqtt.listenMessage("testController1", function(topic, message){
		console.log("message received (Controller 1)");
		console.log(topic + ": " + message);
	});
}]);

'

It is provided a broker implementation [hbmqtt broker](https://github.com/beerfactory/hbmqtt/) in python.

## Using

### Methods
------------
1. `ngmqtt.connect(configuration)`    - connect to an MQTT broker (configuration structure in [mqtt.js](https://github.com/mqttjs/MQTT.js)).
1. `ngmqtt.listenConnection(controllerName, callback)`    - bind controller to an MQTT connection.
1. `ngmqtt.subscribe(topic)`    - subscribe an MQTT topic.
1. `ngmqtt.publish(topic, data)`    - publish data to an MQTT topic.
1. `ngmqtt.listenMessage(controllerName, callback)`    - bind controller to the MQTT connection, in order to listen MQTT messages in the controller.

## License

This project is released over [MIT license](http://opensource.org/licenses/MIT).