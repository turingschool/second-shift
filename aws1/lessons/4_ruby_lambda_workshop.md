---
title: Ruby Lambda Workshop
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
      <h2 class="section-header">Workshop: Converting Images to Grayscale using Lambda and S3</h2>
      <p>In this section, you'll build out a function that is triggered when you upload an object to an S3 bucket. That function will take the image, turn it into a grayscale version, and re-upload it into a separate S3 bucket.</p>
      <p>Since this function is more involved and will bring in code from a Ruby Gem, we'll develop the function locally and use the AWS CLI to ship our code.</p>
    </section>
    <hr>
    <section>
      <h2>Setup</h2>
      <ol>
        <li>In S3, create two buckets: <code>&lt;bucketname&gt;</code> and <code>&lt;bucketname-grayscale&gt;</code>. For example: <code>racheloriginals</code> and <code>racheloriginals-grayscale</code>. Use the default settings. You do not need to enable public access or create a bucket policy.</li>
        <li>Earlier, we created a role directly from the Lambda console. Since we'll be deploying from the command line in this workshop, we'll create the role separately. Go to your IAM console and click on <code>roles</code>. From there, click the blue "Create Role" button.</li>
        <li>For "Select type of trusted entity", click on Lambda:</li>
        <img class="screenshot" src="{{site.url}}/assets/images/lambdarolessc.png" alt="Choosing a Lambda role">
        <li>On the next screen, add one role for now: <b>AWSLambdaExecute</b>. This will give our Lambda function permissions to write to CloudWatch and access S3 resources. You can find it by typing in the key terms that you see in the screenshot below.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/lambdaexecute.png" alt="Finding LambdaExecute permissions for role">
        <li>Click through the tags portion. For Role Name, use <code>lambda-grayscale-role</code>.</li>
        <li>Once you've created the role, click back into it and make note of the ARN (shown below). You will need it in the next section.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/lambdaarn.png" alt="ARN for Lambda role">
      </ol>
    </section>
    <section>
      <h2>Lambda Code</h2>
      <p>Now we'll move to the command line to get our Lambda code setup. <b>If you do not have Ruby installed on your computer, jump over to <a target="blank" href="{{ site.url }}/aws1/lessons/4_cloud9_setup">these instructions first</a> to get setup with a virtual Ruby development environment.</b></p>
      <ol>
        <li>Clone down the source code: <code>$ git clone https://github.com/rwarbelow/secondshiftlambdaruby.git</code>. cd into the directory and open the code in your text editor. Look at the <code>handler.rb</code> file and the <code>#process</code> method to get a better idea of what will happen the first time you run the code.</li>
        <li>Because we need to ship the code with all of its dependencies, we need to bundle so that the dependency source code is saved within our project folder. From the command line, type <code>$ bundle install --path vendor/bundle</code>. This will create a ./vendor/bundle directory and put your dependencies there. <b>Unlike with Elastic Beanstalk, the Bundler version doesn't matter. However, it does matter that you're using Ruby 2.5.x since that's what Lambda uses.</b></li>
        <li>The AWS CLI expects a zip file when we create a Lambda function. Because of that, we'll want to compress all of our code by typing this on the command line (inside of your project directory): <code>$ zip -r function.zip .</code> (including the period!) This will compress all of our folders and files (indicated by the <b>.</b>) recursively (-r) into function.zip, a file now living inside your base directory. Type <code>ls</code> to verify that function.zip exists.</li>
        <li>Finally, we'll use the AWS CLI to push our compressed function code up to Lambda. You'll need to replace anything in capital letters. Below is the structure of this command:</li>
        <pre>$ aws lambda create-function --function-name &lt;ARBITRARY NAME OF LAMBDA FUNCTION&gt; \
--zip-file fileb://&lt;NAME OF ZIP FILE&gt; --handler &lt;FILENAME.METHODNAME&gt; --runtime ruby2.5 \
--role &lt;LAMBDA ROLE ARN&gt;</pre>
        <p>Here's what my actual command looks like with the values filled in:</p>
        <pre>$ aws lambda create-function --function-name ConvertToGrayscale \
--zip-file fileb://function.zip --handler handler.process --runtime ruby2.5 \
--role arn:aws:iam::903497756277:role/lambda-grayscale-role</pre>
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
	      <li>Back in your text editor, modify your code so that it can access the bucket name and the object (or key) name. These are the two pieces of data we'll need in order to target the image for grayscale conversion. We've written the code to get the bucket name; <b>your job is to write the code to get the object key name.</b></li>
	      <pre>require 'chunky_png'
require 'aws-sdk-s3'

def process(event:, context:)
  bucket = event["Records"].first["s3"]["bucket"]["name"]
  key = #### YOUR CODE HERE ###
  puts "Here is the bucket: #{bucket}"
  puts "Here is the key: #{key}"
end</pre>
        <li>Re-zip your updated function code, then push it back up to Lambda:</li>
        <pre>$ zip -r function.zip .
$ aws lambda update-function-code --function-name ConvertToGrayscale --zip-file fileb://function.zip</pre>
        <li>Run your PutS3Object test event to see if it correctly prints out the bucket and key name.</li>
      </ol>
    </section>
    <hr>
    <section>
      <h2>ChunkyPNG Code</h2>
      <p>Finally, we'll add the code that will use the <a target="blank" href="https://github.com/wvanbergen/chunky_png">ChunkyPNG</a> gem to convert our color file into a grayscale file and test it out with an actual S3 upload.</p>
      <ol>
        <li>Since this isn't a coding class, we won't spend a lot of time figuring out how to use the ChunkyPNG library. Instead, use the code below to update your <code>handler.rb</code> file. Read the comments to help you process what is happening in the code:</li>
        <pre>require 'chunky_png'
require 'aws-sdk-s3'

def process(event:, context:)
  # get the bucket name and object name
  bucket = event["Records"].first["s3"]["bucket"]["name"]
  key = event["Records"].first["s3"]["object"]["key"]

  # create a client to access S3
  s3 = Aws::S3::Resource.new()

  # use S3 to find the specific object
  obj = s3.bucket(bucket).object(key)

  # make a temporary file name, get the object from S3, and put it in the temporary file
  tmp_file_name = "/tmp/#{key}"
  obj.get(response_target: tmp_file_name)

  # load the object into the ChunkyPNG library
  photo = ChunkyPNG::Image.from_file("/tmp/#{key}")

  # convert the photo to grayscale
  result = photo.grayscale

  # create a string for the new file name and the temporary location of the new file
  new_file_name = "gray-#{key}"
  gray_tmp_file = "/tmp/#{new_file_name}"

  # save the grayscale photo into the temporary file
  result.save(gray_tmp_file)

  # create a string that matches the S3 grayscale bucket name you created before
  grayscale_bucket = "#{bucket}-grayscale"

  # use S3 to upload the gray temp file to the grayscale bucket using the new file name
  s3.bucket(grayscale_bucket).object(new_file_name).upload_file(gray_tmp_file)
end</pre>
				<li>Re-zip your updated function code, then push it back up to Lambda:</li>
        <pre>$ zip -r function.zip .
$ aws lambda update-function-code --function-name ConvertToGrayscale --zip-file fileb://function.zip</pre>
				<li>Run your PutS3Object test again and you should see this failure:</li>
        <img class="screenshot" src="{{site.url}}/assets/images/s3denied.png" alt="S3 access denied">
        <p>This is because we're using a test payload where the bucket name and object name are fake! We don't actually have access to the <code>example-bucket</code> used by the test. In the next section, let's configure the actual S3 trigger.</p>
      </ol>
     </section>
     <hr>
     <section>
     	<h2>S3 Triggers</h2>
   		<p>Now we'll set up an event to trigger our Lambda function. We want this function to run anytime we upload a new PNG into our <b>originals</b> S3 bucket. Since this function <b>only works with PNG images</b>, it might be useful for you to download <a href="https://secondshiftpngs.s3-us-west-2.amazonaws.com/PNGsForLambda.zip">this zip file</a> which contains several color PNG images you can use for testing purposes.</p>
   		<ol>
	     	<li>Click on Add Trigger:</li>
        <img class="screenshot" src="{{site.url}}/assets/images/addtrigger.png" alt="Button to add Lambda trigger">
        <li>Select <b>S3</b> for the trigger, <b>your original bucket name</b> for the bucket, <b>All create events</b> for the Event type, and press the orange <b>Add</b> button at the bottom.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/triggersettings.png" alt="Config for S3 Lambda trigger">
        <li>Before we try uploading an image, let's open CloudWatch logs which will give us live updates. First, click on <b>Monitoring</b>, then select <b>View logs in CloudWatch</b>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/monitoring.png" alt="View monitoring on CloudWatch">
        <li>In CloudWatch, logs are divided into <span class="vocab">streams</span>, small sequences of log events broken up by time and Lambda container. For example, if you invoked a Lambda function three times in the same second, you would get three streams. If you invoke a Lambda function one right after the other, the logs would be in the same stream. Don't click on any of the logs yet since a new one might appear when we upload an image.</li>
        <li>In a new tab, open S3, go to your <b>originals</b> bucket, and upload one of the PNG images from the samples folder.</li>
        <li>Head back to CloudWatch and see if a new log appeared. If so, click it. If not, click the most recent one and scroll to the bottom. You should be able to see the steps Lambda is taking in addition to any errors. Watch your logs carefully. If you see something like this, that means your function took longer than the default 3 seconds to execute.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/timeout.png" alt="Cloudwatch timeout indicator">
        <p>If this happens, head back over to Lambda and adjust your timeout maximum. I found that most small PNGs (less than 700x700 pixels) can be processed by ChunkyPNG in 30 seconds or less. You can find this configuration by scrolling down the page of your Lambda function:</p>
        <img class="screenshot" src="{{site.url}}/assets/images/fixtimeout.png" alt="Fix timeout in Lambda">
        <li>If you don't get any errors in CloudWatch, click back over to S3, open to your <code>grayscale</code> bucket, and you should be able to open your grayscale images. Tada! 🎉 Try uploading a few more images to see if your function is consistent.</li>
      </ol>
     </section>
    <hr>
    <section>
      <img class="section-image" src="{{ site.url }}/assets/images/cleaning.svg" alt="Mop and Bucket">
    	<h2>Cleanup</h2>
    	<p>AWS is very generous with Lambda limits as we saw during the lesson. Therefore, unlike with EC2 or Beanstalk, you won't be charged for these resources (unless you're planning to upload and convert 1,000,000 PNGs this month). However, if you want to clean up everything we created today, here's what you need to do:</p>
    	<ol>
    		<li>Delete both of your S3 buckets (original and grayscale).</li>
    		<li>From the Lambda functions dashboard, select your <b>ConvertToGrayscale</b> function and choose <b>Delete</b> from the <b>Actions</b> dropdown.</li>
    		<li>Go to the IAM console, click on <b>Roles</b>, select your <b>grayscale-lambda-role</b> and delete it.</li>
    	</ol>
    </section>
  </div>
</div>