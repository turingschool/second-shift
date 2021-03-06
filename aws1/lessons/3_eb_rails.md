---
title: Rails Elastic Beanstalk Workshop
layout: page
course: AWS Cloud Fundamentals
courselanding: /aws1/lessons/
session: 3
---

<div id="wrapper">
	<nav id="toc">
		<small><a style="font-style: italic" href="javascript:history.back()" title="">< back</a></small>
	</nav>
	<div id="content-container">
		<section>
			<a name="Topic1"></a>
			<img class="section-image" src="{{ site.url }}/assets/images/rails.png" alt="Rails logo">
			<h2 class="section-header">{{page.title}}</h2>
			<p>In this workshop, you'll deploy a pre-built, database-backed Rails application using Elastic Beanstalk and CodePipeline.</p>
			<p>Before you start, <b>make sure that you are in the same region where you created your EC2 instances</b> since this is where you have created previous key pairs.</p>
			<p>You'll want to download <a href="https://awscoursematerials.s3.us-east-2.amazonaws.com/taskmanager.zip">this Rails app</a>. Unzip it and put it in a directory you want to work with on the command line. We're specifically not using git to share the code so that you can set it up with your own git repository later on, and this exercise will mimic the same steps you'll take for a brand new Rails project.</p>
			<p>A few quick notes about this Rails app:</p>
			<ul>
				<li>This app uses <code>ruby 2.6.6</code> and <code>rails 5.2.3</code>.</li>
				<li>Because of Beanstalk's defaults, we are using <code>bundler 1.17.3</code>. There is <a target="blank" href="https://stackoverflow.com/questions/55360450/elastic-beanstalk-cant-find-gem-bundler-0-a-with-executable-bundle-gem">a way to use v2 of Bundler</a> with Elastic Beanstalk, but custom platform hooks are beyond the scope of this exercise. To bundle using this specific version of Bundler, you will need to type <code>bundle _1.17.3_</code> from your command line instead of <code>bundle</code>.</li>
				<li>This app has one resource (tasks), and the functionality was built out with rails generate scaffold. The app does not have tests. Yolo.</li>
			</ul>
			<p><b>Although it's not necessary,</b> if you'd like to test out the app locally before you deploy to Elastic Beanstalk, you can do that with these commands:</p>
			<ol>
				<li>First, if you aren't already using ruby 2.6.6, install that version. With rbenv, first upgrade your rbenv packages with <code> brew upgrade rbenv ruby-build</code>, then install 2.6.6 with <code>rbenv install 2.6.6</code>, then set it locally with <code>rbenv local 2.6.6</code>.</li>
				<li><code>bundle _1.17.3_</code></li>
				<li><code>rake db:create db:migrate</code></li>
				<li><code>rails s</code></li>
				<li>Navigate to localhost:3000 in your browser</li>
			</ol>
		</section>
		<hr />
		<section>
			<a name="rds"></a>
			<img class="section-image" src="{{ site.url }}/assets/images/rds.png" alt="AWS RDS Logo">
			<h2 class="section-header">Setting up the RDS Database</h2>
			<p>First, we'll want to set up our RDS database so that we have the endpoint, DB name + password, and port to use when setting up our Elastic Beanstalk environment.</p>
			<ul>
				<li>In your AWS Management Console, open RDS.</li>
				<li>Click the orange <b>Create Database</b> button.</li>
				<li><b>Don't</b> click the "Easy Create" slider, as there are two custom configurations that we want for our database that this option takes away from us.</li>
				<li>Select a Postgres database for <b>Engine Type</b>.</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/selectrds.png" alt="Screen shot for selecting RDS engine">
				<li>Scroll down and select <b>Free Tier</b> for Template.</li>
				<li>In the <b>input fields for DB instance identifier, master username, and master password</b>, type "taskmanager". This is (obviously) not best practice for a production database, but for now, this will make it easier for you to remember the inputs since we'll keep all of them the same.</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/rdssettings1.png" alt="Screen shot for setting up RDS security">
				<li>Scroll down to Storage and uncheck <b>Enable storage autoscaling</b>. We won't need that anyway, but we also dn't want to get charged if for some reason we do go over that limit.</li>
				<li>Scroll to Connectivity and click <b>Additional connectivity configuration</b>. Make sure that NO is checked off in the Publicly Accessible section (it should be, but just double check).</li>
				<li>In that same section, find <b>VPC Security Groups</b> and select the box that says "Create New".</li>
				<li>For <b>New VPC security group name</b>, enter "rds-taskmanager". This is an arbitrary name, but it will help us identify it later.</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/rdsconnect.png" alt="Screen shot for connecting RDS">
				<li>Scroll down to <b>Additional Configuration</b> and open up the options.</li>
				<li>In <b>Initial database name</b>, type <code>taskmanager</code>.</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/rdsadditional.png" alt="Screen shot for naming RDS database">
				<li>Uncheck <b>Enable automatic backups</b>. This would be good to have on if we were creating a real app, but to save on S3 storage, we'll skip backups.</li>
				<li>Scroll down to the bottom and click the orange <b>Create database</b> button at the bottom.</li>
			</ul>
			<p>Your database is being created. In the meantime, open up the rails app that you downloaded and change the <code>database.yml</code> file (it lives in the <code>config</code> folder) by making the production section look like this:</p>
			<pre>production:
  <<: *default
  database: <%= ENV['RDS_DB_NAME'] %>
  username: <%= ENV['RDS_USERNAME'] %>
  password: <%= ENV['RDS_PASSWORD'] %>
  host: <%= ENV['RDS_HOSTNAME'] %>
  port: <%= ENV['RDS_PORT'] %></pre>
		  <p>This sets the database name, username, password, host, and port to be environment variables that we'll set in our EB environment.</p>
		</section>
		<hr />
		<section>
			<a name="ebenv"></a>
			<img class="section-image" src="{{ site.url }}/assets/images/eb.png" alt="AWS Elastic Beanstalk Logo">
			<h2 class="section-header">Preparing an Elastic Beanstalk App and Environment</h2>
			<p>For this exercise, we'll just create a development environment; however, it is possible to have many (ie. dev, prod).</p>
			<ul>
				<li>Open the Elastic Beanstalk console in a new tab. Keep your RDS dashboard open because we'll need to reference values from here later.</li>
				<li>Click the orange <b>Create Application</b> button.</li>
				<li>For <b>application name</b>, type "taskmanager".</li>
				<li>For <b>platform</b>, choose Ruby, and for <b>platform branch</b>, choose "Puma with Ruby 2.6 running on 64bit Amazon Linux"</li>
				<li>For <b>Application Code</b>, keep "Sample application" selected. We won't use the sample app, but we also don't want to upload our code through a zip file or S3. Eventually, we'll set up a pipeline to push our code from Github to EB, but that comes later.</li>
				<li>Don't click the blue button! Instead, click the grey <b>Configure more options</b> button. This is what your screen should look like:</li>
				<img style="width: 80%" src="{{ site.url }}/assets/images/createapp.png" alt="EB create app screenshot">
			</ul>
			<p>This configuration dashboard shows you what Elastic Beanstalk is about to be setup for you. You can customize anything that needs to be added or changed. For now, we'll do the following:</p>
			<ul>
				<li>Under the <b>Software</b> box, click "Edit".</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/ebconfiguresoftware.png" alt="Screen shot for modifying software config">
				<li>Scroll to the bottom where you'll see inputs for <b>Environment properties</b>. In here, you'll need to add six keys, five of which are the database environment variables we referenced in our <code>database.yml</code> file earlier plus one more variable:</li> 
				<ul>
					<li>You'll first need to set <code>SECRET_KEY_BASE</code>: you can get this by typing <code>$ rake secret</code> in your terminal while you're in the Rails app. The output is what you'll paste in for the value.</li>
					<li><code>RDS_PORT</code>: use <code>5432</code> (this is the Postgres port, which you can also find on your RDS dashboard)</li>
					<li><code>RDS_DB_NAME</code>: use <code>taskmanager</code></li>
					<li><code>RDS_USERNAME</code>: use <code>taskmanager</code></li>
					<li><code>RDS_PASSWORD</code>: use <code>taskmanager</code></li>
					<li><code>RDS_HOSTNAME</code>: go back to your RDS dashboard and find the value for endpoint which should be on the <b>Connectivity & security</b> tab. It should look like a URL. Copy that and paste it as the hostname value in your Elastic Beanstalk configuration window. <b>If it's blank, that just means AWS hasn't finished provisioning the database.</b> Just keep refreshing.</li>
					<img style="width: 50%" src="{{ site.url }}/assets/images/endpoint.png" alt="RDS endpoint screenshot">
				</ul>
				<li>This is what your environment properties should look like (with your own values). <b>The order does not matter</b>.</li>
				<img style="width: 80%" src="{{ site.url }}/assets/images/envprops.png" alt="EB environment properties screenshot">
				<li>If everything looks right, click <b>Save</b> which will bring you back to the main configuration screen.</li>
			</ul>
			<p>Next, click "Modify" under the <b>Security</b> configurations box. Choose a keypair from the dropdown (make sure this is a keypair that you have access to!), then save. You do not need to put anything for service role or IAM instance profile -- AWS will generate a role for you.</p>
			<img class="screenshot" src="{{ site.url }}/assets/images/ebmodifysecurity.png" alt="Screen shot for modifying ssh key">
			<p>The rest of the settings are beyond the scope of this exercise. It may be tempting to click on the Database setting, but we'll skip this for now, as we want our database to be separated from our Beanstalk environment. This is best practice so that your data is preserved even if you delete your app's environment.</p>
			<p>Click on the blue <b>Create app</b> button at the bottom.</p>
			<p>Compared to Heroku, AWS Elastic Beanstalk takes a surprisingly long time to provision and configure your resources. This process will probably take about 5-10 minutes.</p>
		</section>
		<hr />
		<section>
			<a name="break"></a>
			<img class="section-image" src="{{ site.url }}/assets/images/break.svg" alt="Coffee and Donuts">
			<h2 class="section-header">Take a Break</h2>
			<p>Whew. Before we create the CodePipeline (which will deploy our app), we need to fix one security group issue, but we can't do that until our security group is provisioned from Beanstalk. Take a short break and come back when your Beanstalk dashboard looks like this:</p>
			<img src="{{ site.url }}/assets/images/beanstalksuccess.png" alt="Successful Elastic Beanstalk dashboard">
		</section>
		<hr />
		<section>
			<a name="secgroup"></a>
			<img class="section-image" src="{{ site.url }}/assets/images/comingsoon.png" alt="AWS Security Group Logo">
			<h2 class="section-header">Fixing Security Group Issues</h2>
			<p>We created a new security group, <code>rds-taskmanager</code>, for our RDS database, but right now it will not accept any incoming traffic from our Elastic Beanstalk app. Let's fix that:</p>
			<ul>
				<li>In a new tab, open up the EC2 dashboard.</li>
				<li>On the left-hand side, click <b>Security Groups</b> under "Network & Security".</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/secgroups.png" alt="Screen shot for security group location">
				<li>Find the security group that says <b>rds-taskmanager</b> under Group Name and select it.</li>
				<li>Click on the tab that says <b>Inbound</b>. You should see that it has a rule for inbound PostgreSQL, but the source is YOUR personal IP address (check this by Googling "what is my IP address" and comparing the two), not the IP of the EC2 instance that will need to access it.</li>
				<li>Click the <b>Edit</b> button.</li>
				<li>Delete the current source (which is your computer's IP address), and start typing "sg". You should see all of your security groups pop up. Select the one that has "awseb" in its name -- this is the security group that AWS Elastic Beanstalk made for us, which is the one that our EC2 instance will be in. We want to accept PG traffic from that instance, so we'll reference its security group.</li>
				<img src="{{ site.url }}/assets/images/ebsg.png" alt="Screenshot selecting correct security group">
				<li>Click the orange <b>Save rules</b> button.</li>
			</ul>
		</section>
		<hr />
		<section>
			<a name="codepipeline"></a>
			<img class="section-image" src="{{ site.url }}/assets/images/codepipeline.png" alt="AWS CodePipeline Logo">
			<h2 class="section-header">Setting up a Pipeline</h2>
			<p>CodePipline can integrate build and test tools in addition to deploy, but for now, we'll create a pipeline that just watches for a new version of code to be pushed to GitHub, then deploys that code automatically to our Beanstalk environment.</p>
			<p>First, we'll need to initialize our Rails project as a git project and create a repository on GitHub for the code.</p>
			<ul>
				<li>From the command line, do <code>git init</code>, <code>git add .</code>, and <code>git commit -m 'Initial commit'</code>.</li>
				<li>In your GitHub account, create a new repository and push your code to it.</li>
			</ul>
			<p>Next, from your AWS Management console, open up CodePipeline in a separate tab and follow these steps:</p>
			<ul>
				<li>Click the orange <b>Create Pipeline</b> button.</li>
				<li>For <b>pipeline name</b>, enter "taskmanagerdevpipeline" (or something to that effect).</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/cpsettings.png" alt="Screen shot for code pipeline settings">
				<li>Leave everything else as is, and click <b>Next</b>.</li>
				<li>For <b>source provider</b>, choose GitHub and connect your account.</li>
				<li>In <b>Repository</b>, select the repository you created earlier. If nothing appears in repository, wait a bit, then re-click in the box. Your repos will appear eventually.</li>
				<li>Select your master branch, and use GitHub Webhooks.</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/cpsourcestage.png" alt="Screen shot for code pipeline source settings">
				<li>Click <b>Next</b>.</li>
				<li>We will skip the build stage, so select the grey button on bottom that says <b>Skip build stage</b>.</li>
				<li><b>Don't worry about the scary-sounding message about not skipping the deploy stage.</b> For this stage, select <b>AWS Elastic Beanstalk</b> in the dropdown.</li>
				<li>Choose your taskmanager application with your environment.</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/cpdeploystage.png" alt="Screen shot for code pipeline deploy stage">
				<li>Click <b>Next</b> and review all of your settings. If these are correct, click <b>Create pipeline</b>.</li>
			</ul>
			<h2 class="section-header">Take a Break</h2>
			<p>Again, this process will probably take a while to deploy your code. If you go to your Elastic Beanstalk dashboard, you'll know the deployment is done and successful when you see the large green checkmark.</p>
			<br><br>
			<div class="try-it">
				<h4>Trying Your App!</h4>
				<p>Once your deployment is done and has succeeded, you can check out your live app. To see your app in action on AWS, click on the url at the top of the page:</p>
				<img src="{{ site.url }}/assets/images/eburl.png" alt="Screen shot of url from elastic beanstalk">
				<p>Take a minute to try out everything in your app. Can you create new tasks? Edit them? Delete them?</p>
			</div>
		</section>
		<hr />
		<section>
      <a name="cleanup"></a>
      <img class="section-image" src="{{ site.url }}/assets/images/cleaning.svg" alt="Mop and bucket">
      <h2 class="section-header">Cleanup</h2>
      <p>There are a lot of resources that we created during this workshop, and they need to be stopped/deleted in a very specific order. <b>DO NOT TRY TO DELETE YOUR EC2 INSTANCE MANUALLY.</b> Follow the instructions below and Elastic Beanstalk will delete everything it provisioned for you.</p>
      <h4>CodePipeline</h4>
    	<p>In the CodePipeline landing page, select your pipeline, then click "Delete Pipeline". Confirm the deletion.</p>
			<img src="{{ site.url }}/assets/images/deletecp.png" alt="Screen shot for deleting code pipeline">
      <h4>EC2</h4>
      <p>In your EC2 panel, click on Security Groups. Find the security group for your RDS database. It should be called "rds-taskmanager". Click on "Edit inbound rules", then delete the inbound rule that links it to the Beanstalk security group, then click "Save rules".</p>
			<img src="{{ site.url }}/assets/images/deletesourcesg.png" alt="Screen shot for deleting RDS security group source">
      <h4>Elastic Beanstalk</h4>
    	<p>Go to your Elastic Beanstalk dashboard and select "Delete application" from the "Actions" dropdown. This will take a while.</p>
			<img src="{{ site.url }}/assets/images/deleteeb.png" alt="Screen shot for deleting Elastic Beanstalk application">
      <h4>RDS</h4>
    	<p>Go to your RDS dashboard and click on "Databases" on the left-hand side. Select your taskmanager database, then choose "Delete" from the "Actions" dropdown. Uncheck "Create final snapshot?" since you don't need one (and they cost money!)</p>
			<img src="{{ site.url }}/assets/images/deleterds.png" alt="Screen shot for deleting RDS database">
    	<p>Click on "Snapshots" on the left-hand side and delete any snapshots. It is possible that you may not have any snapshots.</p>
			<img src="{{ site.url }}/assets/images/deleterdssnap.png" alt="Screen shot for deleting RDS snapshots">
      <h4>Back to EC2 for Security Groups</h4>
      <p>Now that your RDS database is gone, you can remove your RDS security group. Click on "Security Groups" from the left-hand side, select the "rds-taskmanager" group, and select "Delete Security Group" from the "Actions" dropdown.</p>
			<p>If you get an error about a network interface, just wait a moment, as that means your Elastic Beanstalk application has not finished being deleted.</p>
			<img style="width: 80%" src="{{ site.url }}/assets/images/deletesg.png" alt="Screen shot for deleting RDS security group">
			<h4>S3 Artifacts</h4>
			<p>CodePipeline and Elastic Beanstalk both create S3 buckets to store artifacts related to your application versions.</p>
			<p>In the S3 dashboard, select the CodePipeline bucket and delete it. You'll need to click the "empty bucket configuration" link in order for AWS to allow you to do this. Then, go back to the dashboard, select the same bucket, and you should be able to delete it this time.</p>
			<img style="width: 80%" src="{{ site.url }}/assets/images/emptyartifacts.png" alt="Screen shot for deleting RDS security group">
			<p>Next, click into the Elastic Beanstalk bucket. This one has a bucket policy attached to it that does not allow you to delete it. So first, we'll need to delete that bucket policy (see below):</p>
			<img style="width: 80%" src="{{ site.url }}/assets/images/bucketpolicydelete.png" alt="Screen shot for deleting RDS security group">
			<p>Then, repeat the same steps that you did above with the CodePipeline bucket.</p>
    </section>
    <hr />
	</div>
</div>