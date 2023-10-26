# Using Model API

## Prereqs
- Publish a model

## Accessing Published Models

## Creating API Key

## Choosing Payment Strategy
Choose between pay per request and monthly quota payment options. 

*You cannot swap between the two payment strategies within a single payment period. If you want to swap from monthly quota to pay per request, and you do so on October 16, the change won't be in the system until the November billing cycle. If you want to change it and it's more urgent that that, please reach out!*

### Pay Per Request

### Monthly Quota
During the course of the month, you can always increase the quota, but decreasing it will effect the number of requests you have available in a month. During the billing cycle, if you are under the threshold of usage in the lower tier, we 

## Choosing API Configuration

## Calling the API

### cURL

### Java

### Python

### NodeJS

### AutessaScript

## Understanding Response

### Managing Quota and Throttle Headers
* View the headers to make sure you understand how many requests you have left in a period
* If you hit the monthly quota, the API will stop working until the quota is increased
* Usage is not counted if there is an error with prediction
