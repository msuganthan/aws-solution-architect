### Mobile application: MyTodoList

* We want to create a mobile application with the following requirements

* Expose as REST API with HTTPS
* Serverless architecture
* Users should be able to directly interact with their own folder in S3
* Users should authenticate through a managed serverless service
* The users can write and read to-dos, but they mostly read them
* The database should scale, and have some high read throughput

#### User authentication

<img src="../../images/solutions/my-todo-list/user-authentication.png" alt="Serverless user authentication">

#### Giving User acceess to S3

<img src="../../images/solutions/my-todo-list/giving-user-access-to-s3.png" alt="Giving users access to S3">

#### High read throughput

<img src="../../images/solutions/my-todo-list/high-read-throughput.png" alt="High read throughput using DAX">

#### Caching at the API Gateway

<img src="../../images/solutions/my-todo-list/caching-at-the-api-gateway.png" alt="Caching at the API Gateway">

* Serverless REST API: HTTPS, API Gateway, Lambda, DynamoDB
* Using Cognito to generate temporary credentials to access S3 bucket with restricted policy. App users can directly access S3 bucket with restricted policy. App users can directly access AWS resource this way. Pattern can be applied to DynamoDB, Lambda...
* Caching the reads on DynamoDB using DAX
* Caching the REST requests at the API Gateway level
* Security for authentication and authorization with Cognito.