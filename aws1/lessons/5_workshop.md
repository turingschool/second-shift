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
      <p>Download the <a href="https://secondshiftapigatewayfrontend.s3-us-west-1.amazonaws.com/apigatewayfrontend.zip">static site files which will serve as our frontend</a>.</p>
      <p>Open the files in your text editor and look around, particularly at the <code>app.js</code> file. There are three places labeled <b>YOUR ENDPOINT HERE</b> that we'll edit as we create our API Gateway later in this workshop.</p>
      <p>Use <code>$ open index.html</code> to open the site in your browser.</p>
    </section>
    <hr>
    <section>
      <h2 class="section-header">Setting Up Lambda Functions</h2>
      <p>Open the Lambda console and create three functions. We will use these functions for the backend integration of our API Gateway endpoints. We will be working entirely in the dashboard instead of pushing code up from the command line.</p>
      <ul>
        <li><b>allPets</b>: should return the body "viewing all pets"</li>
        <li><b>onePet</b>: should return the body "viewing one pet"</li>
        <li><b>createPet</b>: should return the body "created a pet"</li>
      </ul>
      <p>Your function dashboard should show the three functions:</p>
        <img class="screenshot" src="{{site.url}}/assets/images/petfunctions.png" alt="Screenshot for pet functions">
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
        <a name="repeat"></a>
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
        <li>Select <b>Lambda Function</b> as the integration type, and start typing the name of your <b>allPets</b> function, which should populate in the list. If you don't see your function, check that you're using the correct region. Click save and confirm the popup.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/allpetssetup.png" alt="Screenshot for API resource creation">
        <li>Click <b>Test</b> on the left-hand side which will bring you to the test screen. Scroll down to the blue <b>Test</b> button and click it. You should see the output below:</li>
        <img class="screenshot" src="{{site.url}}/assets/images/petstestresults.png" alt="Screenshot for test result">
        <li>Click on the <b>GET</b> method and from the <b>Actions</b> menu, select <b>Enable CORS</b>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/enablecors.png" alt="Screenshot for test result">
        <li>Check off <b>DEFAULT 4xx</b> and <b>DEFAULT 5xx</b> and leave all other settings the same. Click the blue <b>Enable CORS and replace existing CORS headers</b> button. Confirm the popup.</li>
        <a name="cors"></a>
        <img class="screenshot" src="{{site.url}}/assets/images/confirmcors.png" alt="Screenshot for test result">
        <li>From the <b>Actions</b> menu, select <b>Deploy API</b>. Create a new v1 Deployment Stage and click <b>Deploy.</b></li>
        <img class="screenshot" src="{{site.url}}/assets/images/newstage.png" alt="Screenshot for new deployment stage">
        <li>Copy your endpoint and paste it into the <code>app.js</code> file on line 6. <b>Don't forget to add the /pets path on the end!</b></li>
        <img class="screenshot" src="{{site.url}}/assets/images/apiurl.png" alt="Screenshot for API URL">
        <img class="screenshot" src="{{site.url}}/assets/images/apijavascript.png" alt="Screenshot for JS code">
        <li>Wait a minute or two, then refresh your <code>index.html</code> file in your browser and click on the <b>Show All Pets</b> button. It should say "Viewing all pets"</li>
        <img class="screenshot" src="{{site.url}}/assets/images/viewallpets.png" alt="Screenshot of website loading data">
      </ol>
    </section>
    <hr>
    <section>
      <h2 class="section-header">Creating an Endpoint with a Dynamic Path</h2>
      <p>In this section, we'll create the <code>/pets/:id</code> path. For now, we'll have our Lambda function just return the value of the path. In Session 6, we'll connect to a DynamoDB to retrieve actual values.</p>
      <ol>
        <li>In your API Gateway dashboard, click <b>Resources</b> under your <b>petRescue</b> API. Then click the <b>Actions</b> dropdown and click <b>Create Resource</b>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/dynamicpathoptions.png" alt="Screenshot showing how to create new resource">
        <li>Enter <code>{id}</code> for both the <b>Resource Name</b> and <b>Resource Path</b>. Check <b>Enable CORS Gateway</b>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/dynamicpath.png" alt="Screenshot showing dynamic path">
        <li>From the <b>Actions</b> menu, select <b>Create Method</b>. In the dropdown that appears, choose <b>GET</b>. Click the checkbox to save it. (This is identical to <b>Step 3</b> from above, just with a different path.)</li>
        <p>*_*_*_*_*_*_ If you're confused about how to do this step, go look at <a href="#repeat">the screenshots</a> from the first time you did this. _*_*_*_*_*_*</p>
        <li>Select <b>Lambda Function</b> as the integration type, and start typing the name of your <b>onePet</b> function, which should populate in the list. Click save and confirm the popup.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/onepet.png" alt="Screenshot showing dynamic path">
        <li>We'll need to send the dynamic portion of the path to our Lambda function, so we're going to use a Mapping Template in the Integration Request. Click on <b>Integration Request</b>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/intreq.png" alt="Screenshot showing integration request">
        <li>Scroll to <b>Mapping Templates</b> and click on the arrow. Select "When there are no templates defined", add a mapping template for "application/json", and click the checkmark.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/contentmt.png" alt="Screenshot setting up mapping template">
        <li>In the textbox at the bottom, enter <code>{ "id": "$input.params('id')" }</code>. This will send a JSON object that has a key of id and a value of the dynamic <code>:id</code> portion of the path from the parameters. <b>Click save</b>. It will look like nothing happened, but it saved :)</li>
        <img class="screenshot" src="{{site.url}}/assets/images/gentemplate.png" alt="Screenshot setting up mapping template">
        <li>Scroll up and click on the backward arrow to go back to Method Execution.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/gotome.png" alt="Screenshot getting back to method execution">
        <li>Click on test. This time, enter a number in the {id} path param for the test.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/pathparam.png" alt="Screenshot entering number for path param">
        <li>Run the test. You should see <code>"body": "\"Viewing one pet\""</code> come back, but our Lambda function isn't doing anything with the data we're passing in.</li>
        <li>Open up the Lambda dashboard in a new tab and go to your <code>onePet</code> function. <b>Can you modify it so that you're using the <code>id</code> key of the event to print out a message that says "Viewing pet 7" (or whatever number gets passed in)?</b></li>
        <li>Once you've fixed and saved your Lambda function code, go back to API Gateway and run your test again. This is the output you should get.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/pet7.png" alt="Screenshot testing API gateway">
        <li>Enable CORS for this path by selecting <b>Enable CORS</b> from the <b>Actions</b> dropdown.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/corsdynamic.png" alt="Screenshot enabling cors">
        <li>Check off <b>DEFAULT 4xx</b> and <b>DEFAULT 5xx</b> and leave all other settings the same. Click the blue <b>Enable CORS and replace existing CORS headers</b> button. Confirm the popup.</li>
        <p>*_*_*_*_*_*_ If you're confused about how to do this step, go look at <a href="#cors">the screenshots</a> from the first time you did this. _*_*_*_*_*_*</p>
        <li>From the <b>Actions</b> menu, select <b>Deploy API</b>. Select your <b>v1</b> deployment stage and click <b>Deploy.</b></li>
        <li>Copy your endpoint and paste it into the <code>app.js</code> file on line 16. <b>Don't forget to keep the <code>/pets/${id}</code> path on the end!</b></li>
        <img class="screenshot" src="{{site.url}}/assets/images/apiurl.png" alt="Screenshot for API URL">
        <img class="screenshot" src="{{site.url}}/assets/images/jsshowpet.png" alt="Screenshot for inserting URL in JS code">
        <li>Wait a minute or two, then refresh your <code>index.html</code> file in your browser. Enter a number in the <b>Pet number</b> box and click on the <b>Get pet</b> button. It should say "Viewing pet 4" (or whatever number you entered).</li>
        <img class="screenshot" src="{{site.url}}/assets/images/dashboardonepet.png" alt="Screenshot of succesful one pet request">
      </ol>
    </section>
    <hr>
    <section>
      <h2 class="section-header">Creating a POST Endpoint to Send Data</h2>
      <p>In this section, we'll add a <code>POST</code> method to the <code>/pets</code>. For now, we'll have our Lambda function just return the value of the pet name we enter. In Session 6, we'll connect to a DynamoDB to retrieve actual values.</p>
      <ol>
        <li>Make sure you're in the <b>Resources</b> section of your API (not the <b>Stages</b> section).</li>
        <li>Under the <code>/pets</code> path, add a <b>POST</b> method by selecting <b>Add Method</b> from the <b>Actions</b> dropdown. Make sure to click the checkmark to save it.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/selectpost.png" alt="Screenshot for selecting post method">
        <li>Select your Lambda function <b>createPet</b> and click <b>Save</b>.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/createpet.png" alt="Screenshot for selecting Lambda function">
        <li>Click on <b>Test</b> and scroll down to where you see the Request Body textbox. Switch over to your text editor and look at how our <code>app.js</code> file is sending the fetch request. The body is an object with keys <code>name</code> and <code>id</code>. We'll need to do the same thing when we set up our test body, using the structure <code>{ "name": "Joey", "id" : "7" }</code>. Enter that in the textbox, then click <b>Test.</b></li>
        <img style="width: 50%" src="{{site.url}}/assets/images/jsbody.png" alt="Screenshot for js code">
        <img class="screenshot" src="{{site.url}}/assets/images/testbody.png" alt="Screenshot for test body">
        <li>This is what you should see for the test output:</li>
        <img class="screenshot" src="{{site.url}}/assets/images/createdpet.png" alt="Screenshot for test body">
        <li>Like before, nothing is happening with the data we're passing in. We don't need to use Mapping Templates this time since the data is not coming out of the URL (it's coming from the body of the request, which is passed directly to Lambda). Open up the Lambda dashboard in a new tab and go to your <code>createPet</code> function. <b>Can you modify it so that you're using the <code>name</code> and <code>id</code> keys of the event to print out a message that says "Created Joey with ID 7" (or whatever name and ID gets passed in)?</b></li>
        <li>Once you've fixed and saved your Lambda function code, go back to API Gateway and run your test again. This is the output you should get.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/successjoey.png" alt="Screenshot for successful pet creation">
        <li>Enable CORS again from the <b>Actions</b> dropdown so that this new POST request has the correct headers.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/corspost.png" alt="Screenshot for enabling cors">
        <li>From the <b>Actions</b> menu, select <b>Deploy API</b>. Select your <b>v1</b> deployment stage and click <b>Deploy.</b></li>
        <li>Copy your endpoint and paste it into the <code>app.js</code> file on line 27. <b>Don't forget to keep the <code>/pets</code> path on the end!</b></li>
        <img class="screenshot" src="{{site.url}}/assets/images/apiurl.png" alt="Screenshot for API URL">
        <img class="screenshot" src="{{site.url}}/assets/images/createjs.png" alt="Screenshot for inserting URL in JS code">
        <li>Wait two minutes, then refresh your <code>index.html</code> file in your browser. Enter a name in the <b>Pet name</b> and number in the <b>Pet id</b> box, then click on the <b>Create pet</b> button. It should say "Created Joey with ID 7" (or whatever number you entered). If this does <b>NOT</b> work, try re-enabling CORS and re-deploying.</li>
      </ol>
    </section>
    <hr>
    <section>
      <h2 class="section-header">Deploying the Front End on S3</h2>
      <ol>
        <li>Open up the S3 console and click <b>Create Bucket</b>. Choose a name for your bucket, then click through the options and make sure to <b>uncheck the Block all public access option</b>.</li>
        <li>Upload all of your frontend files into the bucket.</li>
        <li>In the <b>Permissions</b> tab, paste in this bucket policy that allows public access of your objects. Make sure to insert the ARN for your own bucket where indicated <b>and don't forget to add the <code>/*</code> at the end of your bucket ARN</b> so that the bucket policy refers to objects, not the bucket.</li>
        <pre>{
  "Id": "Policy1565499669881",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1565499668813",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": "INSERT YOUR BUCKET ARN HERE/*",
      "Principal": "*"
    }
  ]
}</pre>
        <li>Click on the <b>Properties</b> tab and enable <b>Static website hosting</b>. Type <code>index.html</code> for the index document. We do not have an error document, so leave that field blank. Click save.</li>
        <li>Click <b>Static website hosting</b> again so that you can get the URL of your website. Try it out!</li>
      </ol>
    </section>
  </div>
</div>