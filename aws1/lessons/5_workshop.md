---
title: Connecting a Front End to API Gateway and Lambda
layout: page
course: AWS Cloud Fundamentals
courselanding: /aws1/lessons/
session: 5
---

<div id="wrapper">
  <nav id="toc">
    <small><a style="font-style: italic" href="javascript:history.back()" title="">< back</a></small>
  </nav>
  <div id="content-container">
    <section>
      <a name="workshop"></a>
      <img class="section-image" src="{{ site.url }}/assets/images/pets.svg" alt="Pets">
      <h2 class="section-header">Workshop: Connecting a Front End to API Gateway and Lambda</h2>
      <p>We'll combine what we've learned about S3 static sites, API Gateway endpoints, and Dynamo DB functions to create a front end that can communicate with our API Gateway and Lambda backend.</p>
    </section>
    <hr>
    <section>
      <h2 class="section-header">Setup</h2>
      <p>Downloaded the <a href="https://secondshiftapigatewayfrontend.s3-us-west-1.amazonaws.com/apigatewayfrontend.zip">static site files which will serve as our frontend</a>.</p>
      <p>Open the files in your text editor and look around, particularly at the <code>app.js</code> file. There are three places labeled <b>YOUR ENDPOINT HERE</b> that we'll edit as we create our API Gateway later in this workshop.</p>
      <p>Use <code>$ open index.html</code> to open the site in your browser.</p>
    </section>
    <hr>
    <section>
      <h2 class="section-header">Creating your API Gateway and First Endpoint</h2>
      <ol>
        <li>In the API Gateway console, create a new REST API Gateway with the name <code>petRescue</code>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/createapi.png" alt="Screenshot for API Gateway creation">
        <li>From the <b>Actions</b> menu, select <b>Create Resource</b>. Use <code>/pets</code> for the Resource Name and Resource Path, and make sure to check off <b>Enable API Gateway CORS</b>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/petsresource.png" alt="Screenshot for API resource creation">
        <li>From the <b>Actions</b> menu, select <b>Create Method</b>. In the dropdown that appears, choose <b>GET</b>. Click the checkbox to save it.</li>
        <div class="flex-container img-steps">
        <div style="flex-basis: 100%">
          <img src="{{site.url}}/assets/images/createmethod.png">
        </div>
        <div style="flex-basis: 100%">
          <img src="{{site.url}}/assets/images/selectget.png">
        </div>
        <div style="flex-basis: 100%">
          <img src="{{site.url}}/assets/images/confirm.png">
        </div>
      </div>
      </ol>
    </section>
  </div>
</div>