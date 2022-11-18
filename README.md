# Tutorial

https://www.alxolr.com/articles/how-to-test-locally-aws-sqs-queues-in-node-js

Note: a bit outdated:

## Correction #1
Had to add these dummy options to the index.ts → new SQS():
accessKeyId: 'key',
secretAccessKey: 'secret',
sessionToken: 'token',

This was needed for both connecting to the docker and JVM based servers.

## Correction #2
The docker image in the tutorial is old. Got the latest instructions and image from the docker repo at Docker Hub . Told me to start elasticmq in docker with:

```
docker run -p 9324:9324 -p 9325:9325 -v $(pwd)/elasticmq.conf:/opt/elasticmq.conf softwaremill/elasticmq
```

elasticmq.conf file changed to have a quote-staging queue name:

***Because we can start the EMQ via docker with just the command above and the .conf file:***

***1) The Dockerfile and the .conf in the repo are unneeded.***

***2) In all reality, the sqs-mock repo is unneeded - it just shows an example of how to hit the MQ using node.***

***Thus, for all intents and purpose, that command and the .conf file is all that is needed - and setting the QUEUE_URL value to:***

***- QUEUE_URL=http://localhost:9324/000000000000/quote-staging***

***In arq-manager***

# Running

## Install and run this project to see a list of queues

```
npm install

npm run build

npm start
```

## Sending to the queue using the aws CLI tool
```
aws sqs send-message --endpoint-url http://localhost:9324 --queue-url http://localhost:9324/000000000000/quote-staging --message-body '{"Subject": "<<<Put Event Name here replacing the surrouding angle brackets too>>>", "Message": "<<<Put Escaped JSON Body Here replacing surrounding angle brackets too >>>"}' --region us-west-2
```

Example:
```
aws sqs send-message --endpoint-url http://localhost:9324 --queue-url http://localhost:9324/000000000000/quote-staging --message-body '{"Subject": "QUOTE_PROCESSING_COMPLETE", "Message": "{"caseId":"6232f74f-5a6a-443e-87ff-8818ab060d03","contractId":42,"numSubmissions":8,"numErrored":3,"numSucceeded":5,"uuid":"6232f74f-5a6a-443e-87ff-8818ab060d03","relatedUUID":null}"}' --region us-west-2
```



