---
title: Basic Ruby S3 SDK Exercises
layout: page
course: AWS Cloud Fundamentals
courselanding: /aws1/lessons/
class: 1
---


<div id="wrapper">
	<nav id="toc">
		<small><a style="font-style: italic" href="{{side.base_url}}/aws1/lessons/1" title="">< back</a></small>
	</nav>
	<div id="content-container">
		<section>
			<img class="section-image" src="{{ site.url }}/assets/images/code.png" alt="Code in text editor">
			<h2><a name="Topic1">{{page.title}}</a></h2>
			<p>In this section, you'll use the Ruby AWS SDK to interact with the S3 service.</p>
			<p>By default, AWS will use the configuration that is located in <code>~/.aws/configuration</code>. This file contains the Access Key ID and Secret Access Key that you set when you typed <code>$ aws configure</code>.</p>
		</section>
		<hr />
		<section>
			<h2><a name="Topic1">Setup</a></h2>
			<ol>
				<li>Make a new directory <code>s3_ruby_exercises</code>.</li>
				<li>Create a new file called <code>s3repl.rb</code>.</li>
				<li>Install the Ruby SDK on the command line with <code>$ gem install aws-sdk</code></li>
				<li>Make a Gemfile: <code>touch Gemfile</code> and add the following:</li>
				<pre>source "https://rubygems.org"
gem 'aws-sdk', '~> 3'</pre>
				<li>At the top of your <code>s3repl.rb</code>, add the following code which will require the 'aws-sdk' and create an object you will use to interact with S3:</li>
				<pre>require 'aws-sdk-s3'
s3 = Aws::S3::Client.new(region: 'us-east-2')</pre>
			</ol>
				<p>Remember, you don't need to set env variables or import keys since the SDK looks in your <code>~/.aws/configuration</code> file.</p>
		</section>
		<hr />
		<section>
			<h2><a name="Topic1">Creating Buckets</a></h2>
			<p>Use the <a href="https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#createBucket-property">createBucket</a> documentation (and/or Google) to write code that creates an empty bucket in your account.</p>
			<p>Extensions:</p>
			<ul>
				<li>Can you accept a command line argument for the bucket name? Need help? Try this code in a file: <code>puts "Hello, #{ARGV[0]}"</code> And run it using <code>ruby myfile.rb bob</code></li>
				<li>Instead of a command line argument, can you accept a dynamic value for the bucket name that is entered while the program is running? Hint:</li>
				<pre>puts "How are you feeling today?"
response = gets.chomp</pre>
				<li>Can you create a bucket where encryption is enabled automatically?</li>
				<li>Can you create a bucket where versioning is enabled automatically?</li>
				<li>Right now, your bucket is created in the default region you set during <code>$ aws configure</code>. Using code, can you change the region where your bucket is created?</li>
			</ul>
		</section>
		<hr />
		<section>
			<h2><a name="Topic1">Uploading Objects</a></h2>
			<p>Use the <a href="https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putObject-property">putObject</a> documentation (and/or Google) to write code that uploads a file (object) in the bucket you just created.</p>
			<p>Extensions:</p>
			<ul>
				<li>Can you accept a dynamic value for the object to be uploaded that is entered while the program is running?</li>				
			</ul>
		</section>
		<hr />
		<section>
			<h2><a name="Topic1">Listing Objects</a></h2>
			<p>Use the <a href="https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property">listObjects</a> documentation (and/or Google) to write code that lists all of the objects in a bucket.</p>
			<p>Extensions:</p>
			<ul>
				<li>Can you accept a dynamic value for the bucket name that is entered while the program is running? Hint:</li>				
				<li>Can you accept a dynamic value for how many objects within a bucket should be returned that is entered while the program is running?</li>				
			</ul>
		</section>
		<hr />
		<section>
			<h2><a name="Topic1">Deleting Objects</a></h2>
			<p>Use the <a href="https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteObject-property">deleteObject</a> documentation (and/or Google) to write code that deletes an object in the bucket.</p>
			<p>Extensions:</p>
			<ul>
				<li>Can you accept a dynamic value for the object that is entered while the program is running?</li>				
			</ul>
		</section>
		<hr />
		<section>
			<h2><a name="Topic1">Listing Buckets</a></h2>
			<p>Use the <a href="https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listBuckets-property">listBuckets</a> documentation (and/or Google) to write code that lists all of the buckets in your account.</p>
			<p>Extensions:</p>
		</section>
		<hr />
		<section>
			<h2><a name="Topic1">Full REPL Functionality</a></h2>
			<p>Use the code that you wrote in the above exercises to create a full REPL that has a similar interaction pattern to this:</p>
			<pre>What do you want to do? Enter one of the following:
b: list buckets
o: list objects in specific bucket
mb: make bucket
u: upload file
d: delete file
q: quit
------------
b
------------
Here are your buckets:
- myfirstwebsite
- images
What do you want to do? Enter one of the following:
b: list buckets
o: list objects in specific bucket
mb: make bucket
u: upload file
d: delete file
q: quit
------------
mb
------------
What would you like to call your bucket?
------------
documents
------------
Here are your buckets:
- myfirstwebsite
- images
- documents
What do you want to do? Enter one of the following:
b: list buckets
o: list objects in specific bucket
mb: make bucket
u: upload file
d: delete file
q: quit
------------
o
------------
For which bucket would you like to list your objects?
------------
images
------------
The objects in your images bucket are:
- penguin.jpg
- logo.png
- banner.jpg
What do you want to do? Enter one of the following:
b: list buckets
o: list objects in specific bucket
mb: make bucket
u: upload file
d: delete file
q: quit
------------
q
------------
Goodbye!
</pre>
		</section>
		<hr />
		<section>
			<h2>Email Notifications Upon Object Upload</h2>
			<p>This section goes beyond what we did in Class 1 to incorporate AWS Simple Notification Service (SNS). The instructions below will set you up with a program where you receive an email whenever an object is uploaded to a specific bucket.</p>
			<p>If you'd like to read more about SNS before diving in, click <a href="">here</a>.</p>
			<p>First, we'll set up an SNS topic, then set up a subscription to that topic for a specific email address.</p>
			<ol>
				<li>In your AWS Management Console in the browser, select "SNS" from the service list.</li>
			</ol>
		</section>
		<hr />
	</div>
</div>


    
