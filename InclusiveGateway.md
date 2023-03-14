# Implement a BPMN Service Task in Camunda
The article contains a step-by-step guide on how to implement a BPMN Inclusive Gateway in Camunda making use of a Spring Boot Application.

**Java Nibble Article:** [https://www.javanibble.com/implement-bpmn-inclusive-gateway-in-camunda/](https://www.javanibble.com/implement-bpmn-inclusive-gateway-in-camunda/)

## Pre-Requisites
The following is required to run the Spring Boot example:
* [curl](https://www.javanibble.com/how-to-install-curl-on-macos-using-homebrew/)
* jq
* [maven](https://www.javanibble.com/how-to-install-maven-on-macos-using-homebrew/)

## BPMN Inclusive Gateway
A diverging Inclusive Gateway (Inclusive Decision) can be used to create alternative but also parallel paths within a Process flow. A default path can optionally be identified, to be taken in the event that none of the conditional Expressions evaluate to true. A converging Inclusive Gateway is used to merge a combination of alternative and parallel paths. A control flow token arriving at an Inclusive Gateway MAY be synchronized with some other tokens that arrive later at this Gateway.

Use Camunda Modeller to model the process. The process model is composed of five tasks and two inclusive gateways:

![BPMN Inclusive Gateway](https://www.javanibble.com/assets/images/posts/bpmn-inclusive-gateway/bpmn-inclusive-gateway.png)

* Place Order: Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.
* Order Placed: Is an `Inclusive Gateway` with three sequence flows.
* Coffee Ordered: Is a `Sequence Flow` with an expression `${orderedCoffee==true}`.
* Muffin Ordered: Is a `Sequence Flow` with an expression `${orderedMuffin==true}`.
* Default: Is the default `Sequence Flow`.
* Make Coffee: Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.
* Warm up Muffin: Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.
* Give Free Sample: Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.
* Deliver Order:Is a `Service Task` linked to a Delegate Expressions with the name `${logger}`.

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

Scenario 1 is starting the Place Order process and passing in the process instance variable called `orderedCoffee` with a value of `true` and the `orderedMuffin` variable with a value of `false`. The inclusive gateway evaluates the variables and directs the token to the applicable service task.

```shell
$ ./start_parallel_gw_01.sh
```
The script performs the following commands:

```shell
$ curl --location --request POST 'http://localhost:8080/engine-rest/process-definition/key/place-order-process/start' --header 'Content-Type: application/json' --data-raw '{
  "variables": {
    "orderedCoffee": {
      "value": "true",
      "type": "boolean"
    },
      "orderedMuffin": {
        "value": "false",
        "type": "boolean"
    }
  }
}'
```
The following is the output to the console after running the above command.

![Console](https://www.javanibble.com/assets/images/posts/bpmn-inclusive-gateway/console-camunda-bpmn-inclusive-gateway-scenario1.png)


**Run the command: Scenario 2**

Scenario 2 is starting the Place Order process and passing in the process instance variable called `orderedCoffee` with a value of `false` and the `orderedMuffin` variable with a value of `true`. The inclusive gateway evaluates the variables and directs the token to the applicable service task.

```shell
$ ./start_exclusive_gw_02.sh
```
The script performs the following commands:

```shell
$ curl --location --request POST 'http://localhost:8080/engine-rest/process-definition/key/place-order-process/start' --header 'Content-Type: application/json' --data-raw '{
  "variables": {
    "orderedCoffee": {
      "value": "false",
      "type": "boolean"
    },
      "orderedMuffin": {
        "value": "true",
        "type": "boolean"
    }
  }
}'
```
The following is the output to the console after running the above command.

![Console](https://www.javanibble.com/assets/images/posts/bpmn-inclusive-gateway/console-camunda-bpmn-inclusive-gateway-scenario2.png)

**Run the command: Scenario 3**

Scenario 3 is starting the Place Order process and passing in the process instance variable called `orderedCoffee` with a value of `true` and the `orderedMuffin` variable with a value of `true`. The inclusive gateway evaluates the variables and directs the token to the applicable service task.

```shell
$ ./start_exclusive_gw_03.sh
```
The script performs the following commands:

```shell
$ curl --location --request POST 'http://localhost:8080/engine-rest/process-definition/key/place-order-process/start' --header 'Content-Type: application/json' --data-raw '{
  "variables": {
    "orderedCoffee": {
      "value": "true",
      "type": "boolean"
    },
      "orderedMuffin": {
        "value": "true",
        "type": "boolean"
    }
  }
}'
```
The following is the output to the console after running the above command.

![Console](https://www.javanibble.com/assets/images/posts/bpmn-inclusive-gateway/console-camunda-bpmn-inclusive-gateway-scenario3.png)

**Run the command: Scenario 4**

Scenario 4 is starting the Place Order process and passing in the process instance variable called `orderedCoffee` with a value of `false` and the `orderedMuffin` variable with a value of `false`. The inclusive gateway evaluates the variables and directs the token to the applicable service task.
```shell
$ ./start_process_scenario_04.sh
```
The script performs the following commands:
```shell
$ curl --location --request POST 'http://localhost:8080/engine-rest/process-definition/key/place-order-process/start' --header 'Content-Type: application/json' --data-raw '{
  "variables": {
    "orderedCoffee": {
      "value": "false",
      "type": "boolean"
    },
      "orderedMuffin": {
        "value": "false",
        "type": "boolean"
    }
  }
}'
```
The following is the output to the console after running the above command.

![Console](https://www.javanibble.com/assets/images/posts/bpmn-inclusive-gateway/console-camunda-bpmn-inclusive-gateway-scenario4.png)


## View Camunda Admin Console
To view the Camunda Admin Console, type the following url in your browser while the application is running. You will be prompted with the login screen.

* [http://localhost:8080/](http://localhost:8080/)

After you have typed the above URL in a browser while the application is running, you will be prompted with the login screen. Type the Username and Password you set within the application properties file.


## View the H2 Console
To view the H2 Console, type the following url in your browser while the application is running. You will be prompted with the login screen.

* [http://localhost:8080/h2-console](http://localhost:8080/h2-console)

After you have typed the above URL in a browser while the application is running, you will be prompted with the login screen. Press the connect button since there is no password specified.