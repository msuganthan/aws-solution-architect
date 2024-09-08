### Lambda, SNS & SQS

<img src="../../images/solutions/event-processing/lambda-sns-sqs.png" alt="Lambda, SNS & SQS">


### Fan Out Pattern: deliver to multiple SQS

<img src="../../images/solutions/event-processing/fan-out-patterns.png" alt="Fan out Patterns">

### S3 Event Notifications

* S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication...
* Object name filtering possible(*.jpg)
* Use case: generate thumbnails of images uploaded to S3
* Can create as many "S3 events" as desired
* S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer

<img src="../../images/solutions/event-processing/s3-event-notifications.png" alt="S3 Event notifications">

### S3 Event Notification - with Amazon EventBridge

<img src="../../images/solutions/event-processing/s3-notification-with-amazon-event-bridge.png" alt="S3 notification with Amazon EventBridge">

* Advanced filtering options with JSON rules(metadata, object size, name...)
* Multiple Destinations - ex Step Functions, Kinesis Stream / Firehose
* EventBridge Capabilities - Archive, Replay Events, Reliable delivery

### Amazon EventBridge - Intercept API Calls

<img src="../../images/solutions/event-processing/event-bridge-intercept-api-calls.png" alt="EventBridge - Intercept API calls">

### API Gateway - AWS Service Integration - Kinesis Data Streams example

<img src="../../images/solutions/event-processing/api-gateway-kinesis-data-stream-example.png" alt="API Gateway - Kinesis Data Stream Example">