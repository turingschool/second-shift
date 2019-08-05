---
title: Rails Elastic Beanstalk Workshop
layout: page
course: AWS Cloud Fundamentals
courselanding: /aws1/lessons/
session: 3
---

<div id="wrapper">
	<nav id="toc">
		<small><a style="font-style: italic" href="{{side.base_url}}/aws1/lessons/3" title="">< back</a></small>
	</nav>
	<div id="content-container">
		<section>
			<a name="Topic1"></a>
			<img class="section-image" src="{{ site.url }}/assets/images/rails.png" alt="Rails logo">
			<h2 class="section-header">{{page.title}}</h2>
			<p>In this workshop, you'll deploy a pre-built, database-backed Rails application using Elastic Beanstalk and CodeDeploy.</p>
			<p>Before you start, <b>make sure that you are in the N. Virginia region</b> since this is where you have created previous key pairs.</p>
			<p>You'll want to download <a href="https://secondshifttaskmanager.s3.us-east-2.amazonaws.com/taskmanager.zip">this Rails app</a>. Unzip it and put it in a directory you want to work with on the command line. We're specifically not using git to share the code so that you can set it up with your own git repository later on, and this exercise will mimic the same steps you'll take for a brand new Rails project.</p>
			<p>A few quick notes about this Rails app:</p>
			<ul>
				<li>This app uses <code>ruby 2.6.3</code> and <code>rails 5.2.3</code>.</li>
				<li>Because of Beanstalk's defaults, we are using <code>bundler 1.17.3</code>. There is <a target="blank" href="https://stackoverflow.com/questions/55360450/elastic-beanstalk-cant-find-gem-bundler-0-a-with-executable-bundle-gem">a way to use v2 of Bundler</a> with Elastic Beanstalk, but custom platform hooks are beyond the scope of this exercise. To bundle using this specific version of Bundler, you will need to type <code>bundle _1.17.3_</code> from your command line instead of <code>bundle</code>.</li>
				<li>This app has one resource (tasks), and the functionality was built out with rails generate scaffold. The app does not have tests.</li>
			</ul>
			<p>If you'd like to test out the app locally before you deploy to Elastic Beanstalk, you can do that with these commands:</p>
			<ol>
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
				<li>Click the orange "Create Database" button.</li>
				<li><b>Don't</b> click the "Easy Create" slider, as there are two custom configurations that we want for our database that this option takes away from us.</li>
				<li>Select a Postgres database for Engine Type.</li>
				<li>Scroll down and select "Free Tier" for Template.</li>
				<li>In the input fields for DB instance identifier, master username, and master password, type "taskmanager". This is (obviously) not best practice for a production database, but for now, this will make it easier for you to remember the inputs since we'll keep all of them the same.</li>
				<li>Scroll down to Storage and uncheck "Enable storage autoscaling". We won't need that anyway, but we also dn't want to get charged if for some reason we do go over that limit.</li>
				<li>Scroll to Connectivity and click "Additional connectivity configuration". Make sure that NO is checked off in the Publicly Accessible section (it should be, but just double check).</li>
				<li>In that same section, findVPC Security Groups and select the box that says "Create New".</li>
				<li>For "New VPC security group name", enter "rds-taskmanager". This is an arbitrary name, but it will help us identify it later.</li>
				<li>Scroll down to "Additional Configuration" and open up the options.</li>
				<li>In "Initial database name", type "taskmanager" (without the quotes).</li>
				<li>Uncheck "Enable automatic backups". This would be good to have on if we were creating a real app, but to save on S3 storage, we'll skip backups.</li>
				<li>Scroll down to the bottom and click the orange "Create database" button at the bottom.</li>
			</ul>
			<p>Your database is being created. In the meantime, open up the rails app that you downloaded and change the <code>database.yml</code> file by making the production section look like this:</p>
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
				<li>Click the blue "Get started" button.</li>
				<li>For application name, type "taskmanager".</li>
				<li>For platform, choose Ruby.</li>
				<li>For Application Code, keep "Sample application" selected. We won't use the sample app, but we also don't want to upload our code through a zip file or S3. Eventually, we'll set up a pipeline to push our code from Github to EB, but that comes later.</li>
				<li>Don't click the blue button! Instead, click the grey "Configure more options" button. This is what your screen should look like:</li>
				<img style="width: 80%" src="{{ site.url }}/assets/images/createapp.png" alt="EB create app screenshot">
			</ul>
			<p>This configuration dashboard shows you what Elastic Beanstalk is about to be setup for you. You can customize anything that needs to be added or changed. For now, we'll do the following:</p>
			<ul>
				<li>Under the "Software" box, click "Modify".</li>
				<li>Scroll to the bottom where you'll see inputs for Environment properties. In here, you'll need to add five keys, which are the database environment variables we referenced in our <code>database.yml</code> file earlier: <code>RDS_DB_NAME</code>, <code>RDS_USERNAME</code>, <code>RDS_PASSWORD</code>, <code>RDS_HOSTNAME</code>, and <code>RDS_PORT</code>.</li>
				<li>For the values, you'll use <code>taskmanager</code> for the database name, username, and password.</li>
				<li>For the hostname value, go back to your RDS dashboard and find the value for endpoint which should be on the "Connectivity & security" tab. It should look like a URL. Copy that and paste it as the hostname value in your Elastic Beanstalk configuration window. <b>If it's blank, that just means AWS hasn't finished provisioning the database.</b> Just keep refreshing.</li>
				<img style="width: 80%" src="{{ site.url }}/assets/images/endpoint.png" alt="RDS endpoint screenshot">
				<li>For the port, type 5432 (this is the Postgres port, which you can also find on your RDS dashboard).</li>
				<li>The final variable we need to set is <code>SECRET_KEY_BASE</code>. Get the value of this by typing <code>rake secret</code> in the terminal while you're inside of your Rails project. Whatever gets output is the value of this environment variable.</li>
				<li>This is what your environment properties should look like (with your own values). If everything looks right, click Save.</li>
				<img style="width: 80%" src="{{ site.url }}/assets/images/envprops.png" alt="EB environment properties screenshot">
			</ul>
			<p>Next, click "Modify" under the Security configurations. Choose a keypair from the dropdown (make sure this is a keypair that you have access to!), then save.</p>
			<p>The rest of the settings are beyond the scope of this exercise. It may be tempting to click on the Database setting, but we'll skip this for now, as we want our database to be separated from our Beanstalk environment. This is best practice so that your data is preserved even if you delete your app's environment.</p>
			<p>Click on the blue "Create app" button at the bottom.</p>
			<p>Compared to Heroku, AWS Elastic Beanstalk takes a surprisingly long time to provision and configure your resources. This process will probably take about 5-10 minutes, so let's move on to setting up CodePipeline while we wait.</p>
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
				<li>On the left-hand side, click "Security Groups" under "Network & Security".</li>
				<li>Find the security group that says "rds-taskmanager" under Group Name and select it.</li>
				<li>Click on the tab that says "Inbound". You should see that it has a rule for inbound PostgreSQL, but the source is YOUR personal IP address (check this by Googling "what is my IP address" and comparing the two), not the IP of the EC2 instance that will need to access it.</li>
				<li>Click the Edit button.</li>
				<li>Delete the current source, and start typing "sg". You should see all of your security groups pop up. Select the one that has "awseb" in its name -- this is the security group that AWS Elastic Beanstalk made for us, which is the one that our EC2 instance will be in. We want to accept PG traffic from that instance, so we'll reference its security group.</li>
				<img src="{{ site.url }}/assets/images/ebsg.png" alt="Screenshot selecting correct security group">
				<li>Click the blue "Save" button.</li>
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
				<li>Click the orange "Create Pipeline" button.</li>
				<li>For pipeline name, enter "taskmanagerdevpipeline" (or something to that effect).</li>
				<li>Leave everything else as is, and click "Next"</li>
				<li>For source provider, choose GitHub and connect your account.</li>
				<li>In "Repository", select the repository you created earlier. If nothing appears in repository, wait a bit, then re-click in the box. Your repos will appear eventually.</li>
				<li>Select your master branch, and use GitHub Webhooks.</li>
				<li>Click "Next"</li>
				<li>We will skip the build stage, so select the grey button on bottom that says "Skip build stage"</li>
				<li>For the deploy stage, select "AWS Elastic Beanstalk" in the dropdown.</li>
				<li>Choose your taskmanager application with your environment.</li>
				<li>Click "Next" and review all of your settings. If these are correct, click "Create pipeline".</li>
			</ul>
			<p>Again, this process will probably take a while to deploy your code. If you go to your Elastic Beanstalk dashboard, you'll know the deployment is done and successful when you see the large green checkmark.</p>
			<p>To see your app in action on AWS, click on the url at the top of the page:</p>
			<img src="{{ site.url }}/assets/images/eburl.png" alt="Screen shot of url from elastic beanstalk">
		</section>
		<hr />
		<section>
      <a name="cleanup"></a>
      <img class="section-image" src="{{ site.url }}/assets/images/cleaning.svg" alt="Mop and bucket">
      <h2 class="section-header">Cleanup</h2>
      <p>There are a lot of resources that we created during this workshop, and they need to be stopped/deleted in a very specific order.</p>
      <h1>WIP: THIS PART IS NOT FINISHED</h1>
      <h4>CodePipeline</h4>
      <ol>
      	<li>In the CodePipeline landing page, select your pipeline, then click "Delete Pipeline". Confirm the deletion.</li>
      </ol>
      <h4>EC2</h4>
      <ol>
        <li>In your EC2 panel, click on Security Groups and delete the connections between your EC2 and RDS security groups. This step is essential; otherwise, your Elastic Beanstalk teardown will fail.</li>
      </ol>
      <h4>RDS</h4>
      <ol>
      	<li>Click on "Snapshots" on the left-hand side and delete any snapshots.</li>
      </ol>
    </section>
    <hr />
	</div>
</div>