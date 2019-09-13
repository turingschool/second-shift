---
title: Node Lambda Workshop
layout: page
course: AWS Cloud Fundamentals
courselanding: /aws1/lessons/
session: 4
---

<div id="wrapper">
  <nav id="toc">
    <small><a style="font-style: italic" href="javascript:history.back()" title="">< back</a></small>
  </nav>
  <div id="content-container">
    <section>
      <a name="workshop"></a>
      <img class="section-image" src="{{ site.url }}/assets/images/convert.svg" alt="Convert image">
      <h2 class="section-header">Workshop: Triggering SNS Notifications using Lambda</h2>
      <p>In this section, you'll build out a function that is triggered when you upload an object to an S3 bucket. That function will use the AWS Node SDK to push out a notification to subscribers on what we call an "SNS Topic".</p>
      <p>Full disclosure: You can do this same exact thing in the browser console without using a Lambda function. However, the goal of this exercise is to show you that you can build a function to do literally anything and run it on Lambda. We also want to show you how to develop code locally and push it to Lambda.</p>
    </section>
    <hr>
    <section>
      <h2>Setup</h2>
      <p>First, we'll create an SNS topic. A topic is kind of like a mailing list. We'll set it up so that our mailing list sends out a notification every time an object is uploaded to an S3 bucket.</p>
      <ol>
        <li>From the services menu, find Simple Notification Service.</li>
        <li>Type in a name for your topic, like "S3UploadNotification" and click the orange button.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/snslanding.png" alt="SNS landing page">
        <li>On the Details page, leave everything <b>as is</b> and click "Create topic" at the bottom.</li>
        <li>Click on the orange "Create Subscription" button.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/createsub.png" alt="SNS create subscription">
        <li>For protocol, select "SMS" and enter your phone number. Click Create Subscription.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/sms.png" alt="SNS sms setup">
        <li>Go back to topics and click on your S3UploadNotification. Make note of the <b>ARN</b> as you'll need it later on.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/snstopic.png" alt="SNS topic ARN">
      </ol>
      <p>Next, we'll create an IAM role for our Lambda function to use. It will need to have permission for the SNS service:</p>
      <li>Earlier, we created a role directly from the Lambda console. Since we'll be deploying from the command line in this workshop, we'll create the role separately. Go to your IAM console and click on <code>roles</code>. From there, click the blue "Create Role" button.</li>
        <li>For "Select type of trusted entity", click on Lambda:</li>
        <img class="screenshot" src="{{site.url}}/assets/images/lambdarolessc.png" alt="Choosing a Lambda role">
        <li>On the next screen, we're going to add two permissions policies: <b>AWSLambdaExecute</b> and <b>AmazonSNSFullAccess</b>. Search for each of them and check the box.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/lambdaexecute.png" alt="Finding LambdaExecute permissions for role">
        <img class="screenshot" src="{{site.url}}/assets/images/snsaccess.png" alt="Finding SNS permissions for role">
        <li>Click through the tags portion. For Role Name, use <code>lambda-sns-role</code>.</li>
        <li>Once you've created the role, click back into it and make note of the ARN (shown below). You will need it in the next section.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/lambdasnsrole.png" alt="ARN for Lambda role">
    </section>
    <section>
      <h2>Lambda Code</h2>
      <p>Now we'll move to the command line to get our Lambda code setup.</p>
      <ol>
        <li>Clone down the Node skeleton: <code>$ git clone https://github.com/rwarbelow/secondshiftlambdanode.git</code>. cd into the directory and open up the code in your text editor. Look at the <code>index.js</code> file and the <code>handler</code> function to get a better idea of what will happen the first time you run the code.</li>
        <li>From the command line, type <code>$ npm install</code>.</li>
        <li>The AWS CLI expects a zip file when we create a Lambda function. Because of that, we'll want to compress all of our code by typing this on the command line (inside of your project directory): <code>$ zip -r function.zip .</code> (including the period!) This will compress all of our folders and files (indicated by the <b>.</b>) recursively (-r) into function.zip, a file now living inside your base directory. Type <code>ls</code> to verify that function.zip exists.</li>
        <li>Finally, we'll use the AWS CLI to push our compressed function code up to Lambda. You'll need to replace anything in capital letters. Below is the structure of this command:</li>
        <pre>$ aws lambda create-function --function-name &lt;ARBITRARY NAME OF LAMBDA FUNCTION&gt; 
--zip-file fileb://&lt;NAME OF ZIP FILE&gt; --handler &lt;FILENAME.FUNCTIONNAME&gt; --runtime nodejs10.x 
--role &lt;LAMBDA ROLE ARN&gt;</pre>
        <p>Here's what my actual command looks like with the values filled in. I called my function <code>S3TextMessageTrigger</code>, so that's how it will appear in the AWS console:</p>
        <p><b>If you copy and paste this into your terminal, make sure that all of the commands are on one line (not separated into three like you see below).</b></p>
        <pre>$ aws lambda create-function --function-name S3TextMessageTrigger 
--zip-file fileb://function.zip --handler index.handler --runtime nodejs10.x 
--role arn:aws:iam::903497756277:role/lambda-sns-role</pre>
        <li>Open up the Lambda console in the browser and click into your newly created function.</li>
        <li>Create a new test event and check that your event data gets logged. The test should succeed but return null since we're not explicitly returning anything from the method.</li>
      </ol>
    </section>
    <hr>
    <section>
      <h2>S3 Event Data</h2>
      <p>Next, we'll configure our test to mimic the data that would be sent over in the case of an S3 upload and see if we can get our Lambda function to pull out the relevant data: bucket name and object name.</p>
      <ol>
        <li>Create a new test event using the S3 Put Object template. You can name the event test <code>PutS3Object</code></li>
        <img class="screenshot" src="{{site.url}}/assets/images/tests3put.png" alt="S3 Put Object test template">
        <li>Run your test and look at the output for the event. This output indicates the data that will be sent to Lambda when a new object is uploaded to our S3 bucket. Below is the same data, just formatted nicer:</li>
        <pre>{
  "Records" => [
    {
      "eventVersion" => "2.0", 
      "eventSource" => "aws:s3", 
      "awsRegion" => "us-east-1", 
      "eventTime" => "1970-01-01T00:00:00.000Z", 
      "eventName" => "ObjectCreated:Put", 
      "userIdentity" => {
        "principalId" => "EXAMPLE"
      }, 
      "requestParameters" => {
        "sourceIPAddress" => "127.0.0.1"
      }, 
      "responseElements" => {
        "x-amz-request-id" => "EXAMPLE123456789", 
        "x-amz-id-2" => "EXAMPLE123/5678abcdefghijklambdaisawesome/mnopqrstuvwxyzABCDEFGH"
      }, 
      "s3" => {
        "s3SchemaVersion" => "1.0", 
        "configurationId" => "testConfigRule", 
        "bucket" => {
          "name" => "example-bucket", 
          "ownerIdentity" => {
            "principalId" => "EXAMPLE"
          }, 
          "arn" => "arn:aws:s3:::example-bucket"
        }, 
        "object" => {
          "key" => "test/key", 
          "size" => 1024, 
          "eTag" => "0123456789abcdef0123456789abcdef", 
          "sequencer" => "0A1B2C3D4E5F678901"
        }
      }
    }
  ]
}</pre>
	      <li>Back in your text editor, modify your code so that it can access the bucket name and the object name. These are the two pieces of data we'll use to compose a text message to our subscribers. We've written the code to get the bucket name; <b>your job is to write the code to get the object key name.</b></li>
	      <pre>var AWS = require('aws-sdk');
var util = require('util')

exports.handler = function(event, context, callback) {
  var bucket = event.Records[0].s3.bucket.name;
  var key = // YOUR CODE HERE  <----------------
  console.log(`Here is the bucket: ${bucket}`;
  console.log(`Here is the key: ${key}`;
};</pre>
        <li>Re-zip your updated function code, then push it back up to Lambda:</li>
        <pre>$ zip -r function.zip .
$ aws lambda update-function-code --function-name S3TextMessageTrigger --zip-file fileb://function.zip</pre>
        <li>Run your PutS3Object test event to see if it correctly prints out the bucket and key name. If not, rework your code, re-zip, use the update-function-code command, and try again.</li>
      </ol>
    </section>
    <hr>
    <section>
      <h2>Adding SNS Functionality</h2>
      <p>Finally, we'll add the code that will have the SNS service send out a message to subscribers of our topic.</p>
      <ol>
        <li>Since this isn't a coding class, we'll give you the code for sending out message to your SNS topic. Use the code below to update your <code>index.js</code> file.</li>
        <pre>var AWS = require('aws-sdk');

exports.handler = function(event, context, callback) {
  var bucket = event.Records[0].s3.bucket.name
  var key = event.Records[0].s3.object.key

  var params = {
    Message: `Hello! Just wanted to let you know that ${key} was uploaded to ${bucket}.`, 
    TopicArn: 'INSERT THE ARN OF YOUR TOPIC FROM EARLIER HERE'
  };

  var publishTextPromise = new AWS.SNS().publish(params).promise();

  publishTextPromise
  .then(
    function(data) {
      console.log(`${params.Message} successfully published to ${params.TopicArn}.`);
    }
  )
};</pre>
				<li>Re-zip your updated function code, then push it back up to Lambda:</li>
        <pre>$ zip -r function.zip .
$ aws lambda update-function-code --function-name S3TextMessageTrigger --zip-file fileb://function.zip</pre>
				<li>Run your PutS3Object test again. You should get a text message on your phone! Granted, it has fake data. Let's get this connected to S3 in the next section.</li>
      </ol>
     </section>
     <hr>
     <section>
     	<h2>S3 Triggers</h2>
   		<p>Now we'll set up an event to trigger our Lambda function. We want this function to run anytime we upload a new object into an S3 bucket.</p>
   		<ol>
        <li>First, open a new tab for your S3 console and create a new bucket. The name does not matter, but remember what you call it. It does not need public access or any other fancy settings.</li>
	     	<li>Next, click back to your Lambda tab and click on on Add Trigger:</li>
        <img class="screenshot" src="{{site.url}}/assets/images/addtriggersns.png" alt="Button to add Lambda trigger">
        <li>Select <b>S3</b> for the trigger, <b>your bucket name</b> for Bucket, <b>All create events</b> for the Event type, and press the orange <b>Add</b> button at the bottom.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/triggersettings.png" alt="Config for S3 Lambda trigger">
        <li>Before we try uploading an image, let's open CloudWatch logs which will give us live updates. First, click on <b>Monitoring</b>, then select <b>View logs in CloudWatch</b>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/lambdamonitor.png" alt="View monitoring on CloudWatch">
        <li>In CloudWatch, logs are divided into <span class="vocab">streams</span>, small sequences of log events broken up by time and Lambda container. For example, if you invoked a Lambda function three times in the same second, you would get three streams. If you invoke a Lambda function one right after the other, the logs would be in the same stream. Don't click on any of the logs yet since a new one might appear when we upload an image.</li>
        <li>In a new tab, open S3, go to your bucket and upload something (an image, file, etc.).</li>
        <li>Head back to CloudWatch and see if a new log appeared. If so, click it. If not, click the most recent one and scroll to the bottom. You should be able to see the steps Lambda is taking in addition to any errors. Watch your logs carefully. If you see something like this, that means your function took longer than the default 3 seconds to execute.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/timeout.png" alt="Cloudwatch timeout indicator">
        <p>If this happens, head back over to Lambda and adjust your timeout maximum. You can find this configuration by scrolling down the page of your Lambda function:</p>
        <img class="screenshot" src="{{site.url}}/assets/images/fixtimeout.png" alt="Fix timeout in Lambda">
        <li>If you don't get any errors in CloudWatch, Check your phone. Tada! 🎉 Try uploading a few more images to see if your function is consistent.</li>
      </ol>
     </section>
    <hr>
    <section>
      <img class="section-image" src="{{ site.url }}/assets/images/cleaning.svg" alt="Mop and Bucket">
    	<h2>Cleanup</h2>
    	<p>AWS is very generous with Lambda limits as we saw during the lesson. Therefore, unlike with EC2 or Beanstalk, you won't be charged for these resources (unless you're planning to upload and convert 1,000,000 PNGs this month). However, if you want to clean up everything we created today, here's what you need to do:</p>
    	<ol>
    		<li>Delete your S3 bucket.</li>
    		<li>From the Lambda functions dashboard, select your <b>S3TextMessageTrigger</b> function and choose <b>Delete</b> from the <b>Actions</b> dropdown.</li>
    		<li>Go to the IAM console, click on <b>Roles</b>, select your <b>grayscale-sns-role</b> and delete it.</li>
        <li>Go to your SNS dashboard and delete both your topic and the subscription.</li>
    	</ol>
    </section>
  </div>
</div>