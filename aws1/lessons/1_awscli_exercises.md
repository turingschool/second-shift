---
title: AWS CLI Exercises
layout: page
course: AWS Cloud Fundamentals
courselanding: /aws1/lessons/
class: 1
---


<div id="wrapper">
	<nav id="toc">
		<small><a style="font-style: italic" href="javascript:history.back()" title="">< back</a></small>
	</nav>
	<div id="content-container">
		<section>
			<img class="section-image" src="{{ site.url }}/assets/images/code.png" alt="Code in text editor">
			<h2><a name="Topic1">{{page.title}}</a></h2>
			<p>In this section, you'll use the Ruby AWS SDK to interact with the S3 service.</p>
			<p>By default, AWS will use the configuration that is located in <code>~/.aws/configuration</code>. This file contains the Access Key ID and Secret Access Key that you set when you typed <code>$ aws configure</code>.</p>
		</section>
		<section>
			<h4>IAM from the CLI</h4>
			<p>Installing and configuring the AWS CLI tool gives you command line access to the AWS service suite. Although we won't dive deeply into the CLI tool functionality, let's try a few commands:</p>
			<div class="try-it">
				<h4>Try It: IAM from the CLI</h4>
				<p>Type the following commands:</p>
				<ul>
					<li><code>aws iam list-users</code></li>
					<li><code>aws iam list-groups</code></li>
					<li><code>aws iam get-user</code></li>
				</ul>
				<p>Can you use the <a target="blank" href="https://docs.aws.amazon.com/cli/latest/reference/iam/tag-role.html">IAM tag-role documentation</a> to give your user a tag? You can use any key and value that you want, or you can use the key (department) and value (accounting) from the docs example.</p>
				<p>The reason that these commands work for us is that our CLI is configured with a user that has admin access. If we were to reconfigure with a user that has fewer permissions, </p>
			</div>
			<br>
			<div class="try-it">
				<h4>Try It: S3 from the CLI</h4>
				<p>Type the following commands:</p>
				<ul>
					<li><code>aws s3 ls</code></li>
					<li>Make a new bucket: <code>aws s3 mb s3://&lt;YOURBUCKETNAMEHERE&gt;</code></li>
				</ul>
				<p>Can you use the <a target="blank" href="https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html">S3 cp documentation</a> to copy a file on your computer to a specific bucket in your account?</p>
			</div>
		</section>
		<hr />
	</div>
</div>