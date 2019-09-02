---
title: Session 1 Bonus Content
layout: page
course: AWS Cloud Fundamentals
courselanding: /aws1/lessons/
session: 1
---

<h1>Additional Session 1 Material</h1>

<p>If time permits, here are a few "extras" we'll discuss:</p>

<h2>Cross Origin Resource Sharing</h2>
<div class="try-it">
  <h4>Try It: Cross Origin Resource Sharing</h4>
  <p>Let's say you want to keep all of your images in a separate bucket but be able to access them programatically with JavaScript. In order to do this, we'll need to enable CORS on the bucket that contains our image. </p>
  <ol>
    <li>In your text editor, open up the <code>app.js</code> file we downloaded earlier and uncomment the lines near the bottom.</li>
    <li>In the fetch call, insert the URL for the S3 image you uploaded in the previous section.</li>
    <li>Re-upload just the app.js file to your bucket.</li>
    <li>Open up your static site in an incognito tab with the developer tools console open.</li>
    <li>Try clicking the "add image" button.</li>
    <li>Back in your original images bucket, click on the "Permissions" tab and select CORS Configuration.</li>
    <li>Paste the following <a target="blank" href="https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html">CORS configuration</a> into the editor and click save. You can also change the * to the URL of your static site to limit cross-origin requests just to your site.</li>
    <pre>&lt;?xml version="1.0" encoding="UTF-8"?&gt;
      &lt;CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"&gt;
      &lt;CORSRule&gt;
      &lt;AllowedOrigin&gt;*&lt;/AllowedOrigin&gt;
      &lt;AllowedMethod&gt;GET&lt;/AllowedMethod&gt;
      &lt;AllowedHeader&gt;*&lt;/AllowedHeader&gt;
      &lt;/CORSRule&gt;
    &lt;/CORSConfiguration&gt;</pre>
    <li>Close and re-open your incognito tab and try the "add image" button.</li>
  </ol>
</div>

<h2><a>Overview of AWS Services</a></h2>
<p>As of 2019, Amazon Web Services is the market leader in cloud services with a 30% market share, followed my Microsoft Azure, Google Cloud, and Alibaba Cloud. AWS provides cloud-based solutions in a variety of areas. Take a look at the graphic below from <a target="blank" href="https://aws.amazon.com/products/" target="blank" title="">AWS Products</a> page:</p>
<img src="{{ site.url }}/assets/images/awsservices.png" alt="AWS Services">
<div class="try-it">
  <h4>Try It: Service Scavenger Hunt</h4>
  <p>Use the <a target="blank" href="https://aws.amazon.com/products/" target="blank" title="">AWS Products</a> page to determine which AWS tools you could use for the following scenarios:</p>
  <ol>
    <li>I need to store large amounts of data at very low cost.</li>
    <li>I need to be able to protect my application from DDoS attacks.</li>
    <li>I need to create API endpoints that I can use for both my mobile app and the front end of my web app.</li>
    <li>I need to turn written text into realistic speech.</li>
    <li>I need to automate the deployment of my application whenever I push to GitHub.</li>
    <li>I need to deploy and manage my Rails or ExpressJS web application.</li>
    <li>I need to create a SQL database for my app to interact with.</li>
    <li>I need to test my React Native app on Android, iOS, and FireOS devices.</li>
  </ol>
</div>

<h2>CloudFront Distributions vs. Cross Region Replication</h2>
<p>AWS S3 also supports <span class="vocab"><a target="blank" href="https://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html">Cross Region Replication</a></span> (CRR), the purpose of which is sometimes confused with CloudFront. Cross Region replication is, in general, used for disaster backup and compliance, whereas CloudFront is used for decreasing latency to geographically distributed users.</p>