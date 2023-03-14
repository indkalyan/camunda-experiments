# Implement a BPMN Exclusive Gatewayin Camunda
The article contains a step-by-step guide on how to implement a BPMN Exclusive Gateway in Camunda making use of a Spring Boot Application. A diverging Exclusive Gateway (Decision) is used to create alternative paths within a Process flow. This is basically the “diversion point in the road” for a Process. For a given instance of the Process, only one of the paths can be taken. A converging Exclusive Gateway is used to merge alternative paths. Each incoming Sequence Flow token is routed to the outgoing Sequence Flow without synchronization.

**Java Nibble Article:** [https://www.javanibble.com/implement-bpmn-exclusive-gateway-in-camunda/](https://www.javanibble.com/implement-bpmn-exclusive-gateway-in-camunda/)

## Pre-Requisites
The following is required to run the Spring Boot example:
* [curl](https://www.javanibble.com/how-to-install-curl-on-macos-using-homebrew/)
* jq
* [maven](https://www.javanibble.com/how-to-install-maven-on-macos-using-homebrew/)

## BPMN Exclusive Gateway
> An exclusive gateway, is used to model a decision in the process. When the execution arrives at this gateway, all outgoing sequence flows are evaluated in the order in which they have been defined. The sequence flow which condition evaluates to ‘true’ is selected for continuing the process.

Use Camunda Modeller to model the process. The process model is composed of five tasks and two exclusive gateways:

![BPMN Exclusive Gateway](https://www.javanibble.com/assets/images/posts/bpmn-exclusive-gateway/bpmn-exclusive-gateway.png)

* Retrieve Coffee Order: Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.
* Choice of Coffee: Is a `Exclusive Gateway` with three sequence flows.
* Espresso: Is a `Sequence Flow` with an expression `#{order == 'Espresso'}`.
* Caffè Mocha: Is a `Sequence Flow` with an expression `#{order == 'Caffe Mocha'}`.
* Default: Is the default `Sequence Flow`.
* Make Espresso: Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.
* Make Caffè Mocha: Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.
* Give Free Sample: Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.
* Deliver Coffee Order:Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.


## Compile & Run The Example

### 1. Compile the application
Use the following command to compile the Spring Boot application making use of maven:

```shell
$ mvn clean install
```

### 2. Run the application
After you have successfully built the Camunda BPM Spring Boot application, the compiled artifact can be found in the
target directory. Use the following command to start the Camunda BPM Spring Boot Application.

```shell
$ mvn spring-boot:run
```

### 3. Execute the example
After the application has started, run the following command in another terminal:

**Run the command: Scenario 1**

Scenario 1 is starting the Order Coffee process and passing in the process instance variable called `order` with a value of `Espresso`. The exclusive gateway evaluates the `order` variable and directs the token to the applicable service task.

```shell
$ ./start_exclusive_gw_01.sh
```

The script performs the following commands:
```shell
$ curl --location --request POST 'http://localhost:8080/engine-rest/process-definition/key/order-coffee/start' --header 'Content-Type: application/json' --data-raw '{
     "variables": {
         "order": {
             "value": "Espresso",
             "type": "String"
        }
    }
}'
```
The following is the output to the console after running the above command.

![Console](https://www.javanibble.com/assets/images/posts/bpmn-exclusive-gateway/console-camunda-bpmn-exclusive-gateway-scenario1.png)

**Run the command: Scenario 2**

Scenario 2 is starting the Order Coffee process and passing in the process instance variable called `order` with a value of `Caffe Mocha`. The exclusive gateway evaluates the `order` variable and directs the token to the applicable service task.
```shell
$ ./start_exclusive_gw_02.sh
```

The script performs the following commands:
```shell
$ curl --location --request POST 'http://localhost:8080/engine-rest/process-definition/key/order-coffee/start' --header 'Content-Type: application/json' --data-raw '{
     "variables": {
         "order": {
             "value": "Caffe Mocha",
             "type": "String"
        }
    }
}'
```
The following is the output to the console after running the above command.

![Console](https://www.javanibble.com/assets/images/posts/bpmn-exclusive-gateway/console-camunda-bpmn-exclusive-gateway-scenario2.png)

**Run the command: Scenario 3**

Scenario 3 is starting the Order Coffee process and passing in the process instance variable called `order` with a value of `Nothing`. The exclusive gateway evaluates the `order` variable and directs the token to the applicable service task.
```shell
$ ./start_exclusive_gw_03.sh
```
The script performs the following commands:
```shell
$ curl --location --request POST 'http://localhost:8080/engine-rest/process-definition/key/order-coffee/start' --header 'Content-Type: application/json' --data-raw '{
     "variables": {
         "order": {
             "value": "Nothing",
             "type": "String"
        }
    }
}'
```
The following is the output to the console after running the above command.

![Console](https://www.javanibble.com/assets/images/posts/bpmn-exclusive-gateway/console-camunda-bpmn-exclusive-gateway-scenario3.png)

## View Camunda Admin Console
To view the Camunda Admin Console, type the following url in your browser while the application is running. You will be prompted with the login screen.

* [http://localhost:8080/](http://localhost:8080/)

After you have typed the above URL in a browser while the application is running, you will be prompted with the login screen. Type the Username and Password you set within the application properties file.


## View the H2 Console
To view the H2 Console, type the following url in your browser while the application is running. You will be prompted with the login screen.

* [http://localhost:8080/h2-console](http://localhost:8080/h2-console)

After you have typed the above URL in a browser while the application is running, you will be prompted with the login screen. Press the connect button since there is no password specified.
