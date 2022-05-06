# Math API Web Service

This repository is a golang web service that exposes REST HTTP APIs to perform math operations like min, max, average, median, percentile against the list of numbers. It uses Gorilla Mux package to implement a request router and dispatcher for matching incoming requests to their handlers. The web service can be connected through port `8080` on `localhost`.

## API endpoints

#### Get min number(s) based on the quantifier and the list of numbers
+ **Method**: `POST`
+ **URI**: `/min`
+  **Content-Type**: `application/json`
+ **Request Body**:
| Paramter     | Description        |
| :----------- | :----------------- |
| `numList`    | Array of numbers   |
| `quantifier` | Number, (how many) |

#### Get max number(s) based on the quantifier and the list of numbers
+ **Method**: `POST`
+ **URI**: `/max`
+  **Content-Type**: `application/json`
+ **Request Body**:
| Paramter     | Description        |
| :----------- | :----------------- |
| `numList`    | Array of numbers   |
| `quantifier` | Number, (how many) |

#### Get average on the list of numbers
+ **Method**: `POST`
+ **URI**: `/avg`
+  **Content-Type**: `application/json`
+ **Request Body**:
| Paramter     | Description        |
| :----------- | :----------------- |
| `numList`    | Array of numbers   |

#### Get median on the list of numbers
+ **Method**: `POST`
+ **URI**: `/median`
+  **Content-Type**: `application/json`
+ **Request Body**:
| Paramter     | Description        |
| :----------- | :----------------- |
| `numList`    | Array of numbers   |

#### Get qth percentile based on the quantifier 'q' and the list of numbers
+ **Method**: `POST`
+ **URI**: `/percentile`
+  **Content-Type**: `application/json`
+ **Request Body**:
| Paramter     | Description        |
| :----------- | :----------------- |
| `numList`    | Array of numbers   |
| `quantifier` | Number, (q) |

## Assumptions

This utility assumes that it contains enough memory space to store all the resources. The percentile is calculated using [Nearest-Rank method](https://en.wikipedia.org/wiki/Percentile#The_nearest-rank_method).

## Prerequisites

Before you start, please make sure you have Go installed in your system. If not, please use the following link to install Golang:
https://golang.org/doc/install

## Installation

Clone the git repository in your system and then cd into project root directory

```bash
$ git clone https://github.com/RashikaAggarwal/math-api-webservice.git
$ cd math-api-webservice
```

Build your tool by executing the following steps
```bash
$ cd application
$ go run .
```
The server runs on http://localhost:8080 and can be hit through any client like Postman.

## Examples

This utility takes input request in the form of json body. See below examples
### Get min number(s) based on the quantifier and the list of numbers
```http
http://localhost:8080/min
```
#### Request Body
```bash
{
    "numList": [1,5,9,3,7.9],
    "quantifier": 2
}
```
#### Success Response
```bash
[1 3]
```
#### Failure Response
For example, In case of Quantifier passes greater than the length of array
```bash
Quantifier must be less than equal to the length of list of numbers.
```

### Get max number(s) based on the quantifier and the list of numbers
```http
http://localhost:8080/max
```
#### Request Body
```bash
{
    "numList": [1,5,9,3,7.9],
    "quantifier": 2
}
```
#### Success Response
```bash
[9 7.9]
```
#### Failure Response
For example, In case of empty quantifier
```bash
Incomplete input request parameters.
```

### Get average against the list of numbers
```http
http://localhost:8080/avg
```
#### Request Body
```bash
{
    "numList": [1,5,9,3,7.9]
}
```
#### Success Response
```bash
5.18
```
#### Failure Response
For example, In case of empty array of numbers
```bash
Incomplete input request parameters.
```

### Get median against the list of numbers
```http
http://localhost:8080/median
```
#### Request Body
```bash
{
    "numList": [1,5,9,3,7.9]
}
```
#### Success Response
```bash
5
```
#### Failure Response
For example, In case of invalid numbers of array
```bash
Invalid input request.
```

### Get qth percentile based on the quantifier(q) and the list of numbers
```http
http://localhost:8080/percentile
```
#### Request Body
```bash
{
    "numList": [1,5,9,3,7.9],
    "quantifier": 75
}
```
#### Success Response
```bash
7.9
```
#### Failure Response
For example, In case of quantifier greater than 100th percentile
```bash
Quantifier must be less than equal to the 100th percentile.
```

## Unit Testing

This repository contains unit test cases which provide the industry standard code coverage. The test cases can be executed using following commands:
```bash
$ cd math-api-webservice/application
$ go test
```

To get the code coverage, use the following command:
```bash
$ go test -cover
```

## RoadMap

- Handling of invalid routing requests.
- Response Body in a formatted structure
- Response codes and messages maintained in configuration file for success and failure scenarios.
- Include Logging mechanism using packages like "log" to trace the entire API call for monitoring and debugging purposes.
- Secure the APIs through tokens and authorization purposes.
- Handling of concurrent user requests.
- Deploy the application using Nginx server to balance the load and handle server crashing.
- Include Docker to deploy the application on Kubernetes cluster to handle vertical and horizontal scaling effectively.
