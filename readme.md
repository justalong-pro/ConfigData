# Money Account Transactions Application Back-end

### 1. Introduction

The Application is used to collect every transaction of every account.And it also implement a reusable **REST API** for returning the paginated list of money account transactions created in an arbitrary calendar month for a given customer who is logged-on in the portal. And it should make sure the security of the user.

### 2. Development technology

- Java
- Springboot: web, jpa, mybatis, security, jwt, kafka, log4j, swagger
- Mysql
- Kubemetes

### 3. The function

#### Function 1: sign up 

For make sure the security, so i developed a reusable REST API for user to sign up.

#### Function 2: collect every transaction of every account function

I developed a reusable REST API to get the transactions and save the transactions to the database for user to query.

Use the kafka to fullfill this task, user the producer of kafka to get the transactions, and the use consumer to automatically 

to save the data to the mysql.

#### Function 3: query money account transactions by month

Firstly the user signed up the account, and the imformation ot the account has been saved to the database.

Secondly, the transacitons have been saved to the database.

So i can dveloped a api for querying the data by month, not only it would return the transactions of the month,it would also return the total debit and total credit.(The query i will let the user to input password every time for security, and i can let them not do so every time. In reality when I use the bank app, it let me input password for every function. )

### 4. About the structure of the code

I mainly devide the code into three layers(controller,service,dao) ,others are common, kafka config, security, the mapper.xml and the property files. And the model is the data object for request ,response,persistence.

![image-20220524011602891](https://tva1.sinaimg.cn/large/e6c9d24egy1h2iudc668nj20af0bsq3f.jpg)



### 5. How i develop the applicaiton

#### Step 1:  Build my springboot project demo with the start of web, mysql, jpa.

#### Step 2: Read the file of kafka offical websit to learn and build a kafka enviroment and a demo

Dufficlties: I think a lot how to get the messages from the kafka. At last, I decided to save the data to database.

#### Step 3:  Combine the kafka demo to my project and test the first function.

Dufficlties: How to combine the code, what is useful for me and how to make the structure of the code clearly.

#### Step 4: Learn the Spring security and build a demo

Dufficlties: Understand how it works, and should know how to save the password of the user to the database.

#### Step 5: Combine the Spring security demo with my project

Dufficlties: I think a lot how to get the username from the token, maybe i can save the token to the mysql and get username,

maybe i can let the user needn't input password for every time.At last, i temporarily let the user input password for query every time.

#### Step 6: Combine the swagger with my project

#### Step 7: Combine the log4j with my project

#### Step 8: Write the testing case of my project and the junit test of the function

#### Step 9: Find the docker demo to learn how to deploy and use

Dufficlties:Install the docker. Find the docker demo to learn how to deploy springboot demo to it.

#### Step 10: Find the Kubemetes demo to learn how to deploy and use

Dufficlties:Install the Kubemetes. Find the Kubemetes demo to learn how to deploy springboot demo to it. There is something problem with the enviroment.

### 6. About the the architecture

#### 6.1 About the sign up function

Beacuse we don't has user in db, so i just develop this function to save the user to db. And this function we use the spring scurity.

The diagram of my function as below.

![image-20220524143703808](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1hrzv2zj20nz06m752.jpg)

#### 6.2 About the kafka to get the transaction

This function i just use kafka to get transations and save the transactions to db.(I didn't use security here.The diagram of my function as below.

![image-20220524223929579](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1hx0orwj20q6070q3r.jpg)

#### 6.3 About the user query by month.

The request will be checked by the spring security to query. And the request should include the password and username.It can make sure the security by let the user to input password to query.(The user must input password every time.Maybe i can save the token to the database, and find the JWT, but i think is not a good way, so i still want to find the best way.)

![image-20220524154227729](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1i09s1vj21410cen01.jpg)

#### 6.4 Kubemetes(I'm learning.Havn't done.)



### 7. Testing case

#### 7.1 API Test

I use swagger to test my api, the testing cases are as below.

The sit of swagger: http://localhost:9000/swagger-ui/index.html#/.

And then search :" /bank-openapi"as below

![image-20220524180347573](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1i3fhntj20w00gtzm2.jpg)"

#### Case 1

1-1 api: http://localhost:9000/bank/signup

1-2 parameter:

{

​    "username": "jack",

​    "email": "jack@gmail.com",

​    "password": "123456"

}

1-3 Result:

![image-20220524174712826](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1i6cxwgj214a0dx0u5.jpg)

#### Case 2

2-1 api: http://localhost:9000/bank/publish

2-2 parameter:(transferType should be debit or credit)

{

"transactionId" : "122222",

"username":"zhang",

"account":"12345678",

"amount":200,

"currency":"CNY"

"transferType":"debit",

"time":"2021-02-05"

}

2-3 result:

![image-20220524232340421](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1i9ix92j21240h9gnj.jpg)

#### Case 3

3-1 api: http://localhost:9000/bank/queryTransaction

3-2 parameter:

{

"username":"zhang",

"password":"123456",

"account":"12345678",

"year":2021,

"month":2

}

3-3 Result:

![image-20220524232512612](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1ict5u6j212f0kmmyz.jpg)

3-4 Special description

Each transaction 'page' return the total credit and debit values. 

In order to satisfy this need, i deal the total in the service layer, return as below:

​        "account": "total",

​        "totalDebit": 1708.5,

​        "totalCredit": 5050.3

I set the value of account as "total", and set the value of total debit to  "totalDebit", and also set the value of total credit to  "totalCredit".

![image-20220524233233259](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1ighyftj20c3078aag.jpg)

#### 7.2 Unit Test

![image-20220524223822483](https://tva1.sinaimg.cn/large/e6c9d24egy1h2l1ijhosfj20di03z3yl.jpg)



### 8. How it can be better

Q1: The user must input password every time to query the trasaction.

(用户必须每次输入密码去查询账单，我这边觉得可以改进是第一次查询需要输入，然后可以让用户在3分钟之内的查询不需要重新输入密码，这个是我可以改进的)

Q2: When kfakaconcumer consume the data from the topic, i send the data to mysql, i want to find a more better way to do this.

Q3: I just return the total of the credit and debit when the user search, maybe i can record the total amount by month for the history transacitons.

### 9. Conclusion

I have introduced my project and how did I develop it above. I use kafka to get data from the topic and send the data to the mysql.Then the user who have the authucation can input password to seach the trasactions by month. Basically, i think i have fullfilled the need, but as a programmer i can continue find a better way to make it better.

At last, thank you for giving me this chanllenge,  i like this chanllenge and learned a lot from it. I hope i can overcome more chanllenge in the future, and to be a really good programmer.

























