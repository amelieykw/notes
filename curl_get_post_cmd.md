# [command for RESTful api](https://stackoverflow.com/questions/7172784/how-do-i-post-json-data-with-curl-from-a-terminal-commandline-to-test-spring-res)

## GET

curl http://10.22.18.71:3000/prices/

## POST

curl -i -X POST -H "Content-Type: application/json" -X POST --data '[{ "timeStamp": 325, "price": 3243 }, { "timeStamp": 1000,"price": 100 }]' http://10.22.18.71:3000/prices/addNewPrices