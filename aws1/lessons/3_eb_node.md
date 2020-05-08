---
title: Node Elastic Beanstalk Workshop
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
			<img class="section-image" src="{{ site.url }}/assets/images/node.png" alt="NodeJS logo">
			<h2 class="section-header">{{page.title}}</h2>
			<p>In this workshop, you'll deploy a pre-built, database-backed Node/Express application using Elastic Beanstalk and CodePipeline.</p>
			<p>Before you start, <b>make sure that you are in the N. Virginia region</b> since this is where you have created previous key pairs.</p>
			<p>You'll want to download <a href="https://awscoursematerials.s3.us-east-2.amazonaws.com/taskManagerNode.zip">this Express app</a>. Unzip it and put it in a directory you want to work with on the command line. We're specifically not using git to share the code so that you can set it up with your own git repository later on, and this exercise will mimic the same steps you'll take for a brand new Express project.</p>
			<p>A few quick notes about this Express app:</p>
			<ul>
				<li>This app uses <code>express 4.17.1</code>.</li>
				<li>We're using <a target="blank" href="https://www.npmjs.com/package/sequelize">Sequelize</a> as the ORM for a Postgres database. This tutorial assumes that you have Postgres installed locally.</li>
				<li>This app has one resource (tasks). The app does not have tests.</li>
				<li>The routes available are:</li>
				<ol>
					<li>GET /</li>
					<li>GET /tasks</li>
					<li>GET /tasks/:id</li>
					<li>POST /tasks, which takes a <code>title</code> (string) and <code>priority</code> (integer) as body keys.</li>
					<li>DELETE /tasks/:id</li>
				</ol>
			</ul>
			<p>If you'd like to test out the app locally before you deploy to Elastic Beanstalk, you can do that with these commands:</p>
			<ol>
				<li><code>npm install</code></li>
				<li><code>npm install -g sequelize-cli</code></li>
				<li><code>npm install -g nodemon</code></li>
				<li><code>createdb taskmanager</code>, which will create a Postgres database locally</li>
				<li><code>sequelize db:migrate</code>, which creates the tasks table in the database from the migration file</li>
				<li><code>sequelize db:seed:all</code>, which will create three rows of dummy data</li>
				<li><code>npm run dev</code></li>
				<li>Navigate to localhost:3000/tasks in your browser. You can also try out the routes listed above either in the browser or using the <a target="blank" href="https://www.getpostman.com">Postman app</a> for POST and DELETE requests.</li>
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
			<p>Your database is being created. In the meantime, open up the Node app that you downloaded and change the <code>config/config.js</code> file by making the production section look like this:</p>
			<pre>
  production: {
    dialect: "postgres",
    host: process.env.RDS_HOSTNAME,
    username: process.env.RDS_USERNAME,
    password: process.env.RDS_PASSWORD,
    port: process.env.RDS_PORT,
    database: process.env.RDS_DB_NAME,
  }</pre>
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
				<li>For <b>platform</b>, choose Node.js, and for platform branch, select "Node.js 12 running on 64bit Amazon Linux 2"</li>
				<li>For <b>Application Code</b>, keep "Sample application" selected. We won't use the sample app, but we also don't want to upload our code through a zip file or S3. Eventually, we'll set up a pipeline to push our code from Github to EB, but that comes later.</li>
				<li>Don't click the blue button! Instead, click the grey <b>Configure more options</b> button. This is what your screen should look like:</li>
				<img style="width: 80%" src="{{ site.url }}/assets/images/createappnode.png" alt="EB create app screenshot">
			</ul>
			<p>This configuration dashboard shows you what Elastic Beanstalk is about to be setup for you. You can customize anything that needs to be added or changed. For now, we'll do the following:</p>
			<ul>
				<li>Under the <b>Software</b> box, click "Edit". NOTE: this is a screenshot from a Rails environment, so ignore those environment variables.</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/ebconfiguresoftware.png" alt="Screen shot for modifying software config">
				<li>Scroll to the bottom where you'll see inputs for <b>Environment properties</b>. In here, you'll need to add five keys, which are the database environment variables we referenced in our <code>config/config.js</code> file earlier plus one more variable:</li> 
				<ul>
					<li>You'll first need to set <code>NODE_ENV</code>: use the value <code>production</code></li>
					<li><code>RDS_PORT</code>: use <code>5432</code> (this is the Postgres port, which you can also find on your RDS dashboard)</li>
					<li><code>RDS_DB_NAME</code>: use <code>taskmanager</code></li>
					<li><code>RDS_USERNAME</code>: use <code>taskmanager</code></li>
					<li><code>RDS_PASSWORD</code>: use <code>taskmanager</code></li>
					<li><code>RDS_HOSTNAME</code>: go back to your RDS dashboard and find the value for endpoint which should be on the <b>Connectivity & security</b> tab. It should look like a URL. Copy that and paste it as the hostname value in your Elastic Beanstalk configuration window. <b>If it's blank, that just means AWS hasn't finished provisioning the database.</b> Just keep refreshing.</li>
					<img style="width: 50%" src="{{ site.url }}/assets/images/endpoint.png" alt="RDS endpoint screenshot">
				</ul>
				<li>This is what your environment properties should look like (with your own values). <b>The order does not matter</b>. If everything looks right, click Save.</li>
				<img style="width: 80%" src="{{ site.url }}/assets/images/rdsnodeenvprops.png" alt="EB environment properties screenshot">
			</ul>
			<p>Next, click "Modify" under the <b>Security</b> configurations box. Choose a keypair from the dropdown (make sure this is a keypair that you have access to!), then save. You do not need to put anything for service role or IAM instance profile.</p>
				<img class="screenshot" src="{{ site.url }}/assets/images/ebmodifysecurity.png" alt="Screen shot for modifying ssh key">
			<p>The rest of the settings are beyond the scope of this exercise. It may be tempting to click on the Database setting, but we'll skip this for now, as we want our database to be separated from our Beanstalk environment. This is best practice so that your data is preserved even if you delete your app's environment.</p>
			<p>Click on the orange <b>Create app</b> button at the bottom of the main screen.</p>
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
				<li>Click the orange <b>Save</b> button.</li>
			</ul>
		</section>
		<hr />
		<section>
			<a name="codepipeline"></a>
			<img class="section-image" src="{{ site.url }}/assets/images/codepipeline.png" alt="AWS CodePipeline Logo">
			<h2 class="section-header">Setting up a Pipeline</h2>
			<p>CodePipline can integrate build and test tools in addition to deploy, but for now, we'll create a pipeline that just watches for a new version of code to be pushed to GitHub, then deploys that code automatically to our Beanstalk environment.</p>
			<p>First, we'll need to initialize our Node project as a git project and create a repository on GitHub for the code.</p>
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
				<img class="screenshot" src="{{ site.url }}/assets/images/cpskipbuild.png" alt="Screen shot for code pipeline skip build">
				<li><b>Don't worry about the scary-sounding message about not skipping the deploy stage.</b> For this stage, select <b>AWS Elastic Beanstalk</b> in the dropdown.</li>
				<li>Choose your taskmanager application with your environment.</li>
				<img class="screenshot" src="{{ site.url }}/assets/images/cpdeploystage.png" alt="Screen shot for code pipeline deploy stage">
				<li>Click <b>Next</b> and review all of your settings. If these are correct, click <b>Create pipeline</b>.</li>
			</ul>
			<p>Again, this process will probably take a while to deploy your code. In the meantime, this is a good chance to check out the <code>.ebextensions</code> directory that came with the project. You can read more about <a target="blank" href="https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions.html">.ebextensions</a> here, but at a high level, the files inside of this directory automate commands that we want to run on our EC2 instances. In our case, the first two commands (00 and 01) fix a weird problem with the name of the node and npm commands on Elastic Beanstalk. The next command (02) migrates the database, and the last command (03) seeds the databse. In actuality, you wouldn't want to see the database in this file since it will get re-run every time you push to Github. Instead, you would remove the line here and seed the database directly from the EC2 instance. If you want more information on how to do that, check out <a target="blank" href="https://gist.github.com/rwarbelow/ab2f7578bdc9186cca3505adeb4ce66c">this gist</a>.</p> 
			<p>To see your app in action, go back to your Elastic Beanstalk dashboard. If you see a large green checkmark you're ready! You can click the URL at the top of the page:</p>
			<img src="{{ site.url }}/assets/images/eburl.png" alt="Screen shot of url from elastic beanstalk">
			<br><br>
			<div class="try-it">
				<h4>Trying Routes</h4>
				<p>Try out the following routes in your app in the browser:</p>
				<ul>
					<li>GET /</li>
					<li>GET /tasks</li>
					<li>GET /tasks/:id (which takes an id, like 1)</li>
				</ul>
				<p>In the Postman app, try out:</p>
				<ul>
					<li>POST /tasks, which takes a <code>title</code> (string) and <code>priority</code> (integer) as body keys.</li>
					<li>DELETE /tasks/:id</li>
				</ul>
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
      <p>In your EC2 panel, click on Security Groups. Find the security group for your RDS database. It should be called "rds-taskmanager". Delete the inbound rule that links it to the Beanstalk security group by clicking the "X" so it has no rules, then click save.</p>
			<img src="{{ site.url }}/assets/images/deletesourcesg.png" alt="Screen shot for deleting RDS security group source">
      <h4>Elastic Beanstalk</h4>
    	<p>Go to your Elastic Beanstalk dashboard and select "Delete application" from the "Actions" dropdown. This will take a while.</p>
			<img src="{{ site.url }}/assets/images/deleteeb.png" alt="Screen shot for deleting Elastic Beanstalk application">
      <h4>RDS</h4>
    	<p>Go to your RDS dashboard and click on "Databases" on the left-hand side. Select your taskmanager database, then choose "Delete" from the "Actions" dropdown.</p>
			<img src="{{ site.url }}/assets/images/deleterds.png" alt="Screen shot for deleting RDS database">
    	<p>Click on "Snapshots" on the left-hand side and delete any snapshots. It is possible that you may not have any snapshots.</p>
			<img src="{{ site.url }}/assets/images/deleterdssnap.png" alt="Screen shot for deleting RDS snapshots">
      <h4>Back to EC2 for Security Groups</h4>
      <p>Now that your RDS database is gone, you can remove your RDS security group. Click on "Security Groups" from the left-hand side, select the "rds-taskmanager" group, and select "Delete Security Group" from the "Actions" dropdown.</p>
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
