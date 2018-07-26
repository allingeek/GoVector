[![Build Status](https://travis-ci.com/DistributedClocks/GoVector.svg?branch=master)](https://travis-ci.com/DistributedClocks/GoVector)

GoVector
========

This library can be added to a Go project to
generate a [ShiViz](http://bestchai.bitbucket.io/shiviz/)-compatible
vector-clock timestamped log of events in a concurrent or distributed system.
GoVec is compatible with Go 1.4+ 

* govec/    : Contains the Library and all its dependencies
* example/  : Contains some examples instrumented with different features of GoVector

### Usage

To use GoVec you must have a correctly configured go development
environment, see [How to write Go
Code](https://golang.org/doc/code.html)

Once you set up your environment, GoVec can be installed with the go
tool command:

> go get github.com/DistributedClocks/GoVector

*gofmt* will automatically add imports for GoVec. If you do not have a
working version of *gofmt* GoVec can be imported by adding:

See GoVectors library documentation [here](https://godoc.org/github.com/DistributedClocks/GoVector/govec).

###   Examples

The following is a basic example of how this library can be used 
```go
	package main

	import "./govec"

	func main() {
		Logger := govec.InitGoVector("MyProcess", "LogFile")
		
		//In Sending Process
		
		//Prepare a Message
		messagepayload := []byte("samplepayload")
		finalsend := Logger.PrepareSend("Sending Message", messagepayload)
		
		//send message
		connection.Write(finalsend)

		//In Receiving Process
		
		//receive message
		recbuf := Logger.UnpackReceive("Receiving Message", finalsend)

		//Can be called at any point 
		Logger.LogLocalEvent("Example Complete")
		
		Logger.DisableLogging()
		//No further events will be written to log file
	}
```

This produces the log "LogFile.txt" :

	MyProcess {"MyProcess":1}
	Initialization Complete
	MyProcess {"MyProcess":2}
	Sending Message
	MyProcess {"MyProcess":3}
	Receiving Message
	MyProcess {"MyProcess":4}
	Example Complete

An executable example of a similar program can be found in
[Examples/ClientServer.go](example/ClientServer/ClientServer.go)

An executable example of a RPC Client-Server program can be found in 
[Examples/RpcClientServer.go](example/RpcClientServer/RpcClientServer.go)


Here is a sample output of the priority logger

![Examples/Output/PriorityLoggerOutput.png](example/output/PriorityLoggerOutput.PNG)
<!-- July 2017: Brokers are no longer supported, maybe they will come back.

### VectorBroker

type VectorBroker
   * func Init(logfilename string, pubport string, subport string)

### Usage

    A simple stand-alone program can be found in server/broker/runbroker.go 
    which will setup a broker with command line parameters.
   	Usage is: 
    "go run ./runbroker (-logpath logpath) -pubport pubport -subport subport"

    Tests can be run via GoVector/test/broker_test.go and "go test" with the 
    Go-Check package (https://labix.org/gocheck). To get this package use 
    "go get gopkg.in/check.v1".
    
Detailed Setup:

Step 1:

    Create a Global Variable of type brokervec.VectorBroker and Initialize 
    it like this =

    broker.Init(logpath, pubport, subport)
    
    Where:
    - the logpath is the path and name of the log file you want created, or 
    "" if no log file is wanted. E.g. "C:/temp/test" will result in the file 
    "C:/temp/test-log.txt" being created.
    - the pubport is the port you want to be open for publishers to send
    messages to the broker.
    - the subport is the port you want to be open for subscribers to receive 
    messages from the broker.

Step 2:

    Setup your GoVec so that the real-time boolean is set to true and the correct
    brokeraddr and brokerpubport values are set in the Initialize method you
    intend to use.

Step 3 (optional):

    Setup a Subscriber to connect to the broker via a WebSocket over the correct
    subport. For example, setup a web browser running JavaScript to connect and
    display messages as they are received. Make RPC calls by sending a JSON 
    object of the form:
            var msg = {
            method: "SubManager.AddFilter", 
            params: [{"Nonce":nonce, "Regex":regex}], 
            id: 0
            }
            var text = JSON.stringify(msg)

####   RPC Calls

    Publisher RPC calls are made automatically from the GoVec library if the 
    broker is enabled.
    
    Subscriber RPC calls:
    * AddNetworkFilter(nonce string, reply *string)
        Filters messages so that only network messages are sent to the 
        subscriber.      
    * RemoveNetworkFilter(nonce string, reply *string)
        Filters messages so that both network and local messages are sent to the 
        subscriber.
    * SendOldMessages(nonce string, reply *string)
        Sends any messages received before the requesting subscriber subscribed.
  -->
