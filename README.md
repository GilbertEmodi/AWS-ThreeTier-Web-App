# Build a Three-Tier Web App on AWS

**Author:** Gilbert Emodi  
**Email:** zemodi99@gmail.com

---

![Image](https://github.com/GilbertEmodi/AWS-ThreeTier-Web-App/blob/main/architecture-complete.png?raw=true)

---

## Introducing Today's Project!

In this project I will be building a three-tier Web Application from scratch. In order, I will be creating the presentation-tier, logic-tier, then the data-tier. In the end, I will seamlessly connect them all together.

### Tools and concepts

Services I used were Amazon S3, CloudFront, DynamoDB, Lambda, API Gateway.
Key concepts I learned include Lambda functions, CORS errors, updating the Javascript file with the API Invoke URL, and testing the invoke URL within the browser.

### Project reflection

This project took me approximately 2 hours. The most challenging part was resolving the CORS errors in the API Gateway and Lambda.
It was most rewarding to see the final outcome - data getting returned in our web app.

I did the project today to learn about the three-tier architecture and set up my own web app. This project met my goals as I could see a fully functioning web app by the end.

---

## Presentation tier

For the presentation tier, I will set up how the website will be displayed to our end users. The presentation tier is responsible for stroing our files (Amazon S3) and website distribution (Amazon CloudFront).

I accessed my delivered website by visiting the CloudFront distribution's URL. The URL is working because I set up an Orgin Access Control that lets my S3 bucket restrict access to only my CloudFront Distrubution.

![Image](https://github.com/GilbertEmodi/AWS-ThreeTier-Web-App/blob/main/1-screenshot-of-website.JPG?raw=true)

---

## Logic tier

For the logic tier, I will set up a Lambda function to process requests (e.g. look up "userID" to return some data) and also an API with API Gateway to recieve requests from the user and hand it over to Lambda.

The Lambda function retrieves data by looking up the userID that the user enters over the web app in DynamoDB. The AWS SDK is used in the function code so I can use templates and libraries that let me find the correct DynamoDB table & request data.

![Image](https://github.com/GilbertEmodi/AWS-ThreeTier-Web-App/blob/main/2-screenshot-of-Lambda-function.JPG?raw=true)

---

## Data tier

For the data tier, I will set up a DynamoDB database that stores user data. At the moment, there is no data to return to our web app's users. The data in my database will get returned once I set up DynamoDB and connect it with Lambda.

The partition key for my DynamoDB table is "userID". This means that when the table looks up user data, it will look it up based on "userID". Then, it can return all the values related to the item with that ID.

![Image](https://github.com/GilbertEmodi/AWS-ThreeTier-Web-App/blob/main/3-screenshot-of-DynamoDB-JSON.JPG?raw=true)

---

## Logic and Data tier

Once all three layers of my three-tier architecture are set up, the next step is to connect the "presentation" and "logic" tiers. This is because currently there is no way for the API to catch requests that users make through our distributed site.

To test my API, I visited the Invoke URL of the "PROD" stage API. This let me test if I can use the API and retrieve user data. The results were some user data in JSON when I looked up "userId=1". This proved the logic & data tier connection.

![Image](https://github.com/GilbertEmodi/AWS-ThreeTier-Web-App/blob/main/4-screenshot-of-API-results.JPG?raw=true)

---

## Console Errors

The error in my distributed site was because there was a 
problem within "script.js" (one of the website files I uploaded into S3). The "script.js" file is referencing a "PROD" stage API placeholder and not my actual URL.

To resolve the error, I updated "script.js" by replacing some placeholder text with the API's "PROD" stage invoke URL. I then reuploaded "script.js" into S3 because the S3 bucket was still storing the last uploaded version (i.e. with the error).

I ran into a second error after updating script.js. This was an error on the browser side with CORS because API gateway, by default, is only configured to allow requests from users directly running its Invoke URL in the browser (not with CloudFront).

![Image](https://github.com/GilbertEmodi/AWS-ThreeTier-Web-App/blob/main/5-screenshot-of-console-error.JPG?raw=true)

---

## Resolving CORS Errors

To resolve the CORS error, I first went into the API (in API Gateway) and enabled CORS on the "/users" resource. I then made sure "GET" requests are enabled, and referenced the CloudFront domain as the domain getting access.

I also updated my Lambda function because it needs to be able to return CORS headers to show that it has permission to invoke the API's URL and return a response. I added 'Access-Control-Allow-Origin' as a header in the response.

![Image](https://github.com/GilbertEmodi/AWS-ThreeTier-Web-App/blob/main/6-screenshot-of-updated-lambda-CORS.JPG?raw=true)

---

## Fixed Solution

I verified the fixed connection between API Gateway and CloudFront by looking up user data in the distributed site again. In my final test, user data could be returned - so a user request in the presentation tier gets data from the data tier.

![Image](https://github.com/GilbertEmodi/AWS-ThreeTier-Web-App/blob/main/7-screenshot-of-successful-webpage.JPG?raw=true)

---

---
