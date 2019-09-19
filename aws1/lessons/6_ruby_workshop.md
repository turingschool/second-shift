---
title: Ruby DynamoDB Workshop
layout: page
course: AWS Cloud Fundamentals
courselanding: /aws1/lessons/
session: 6
---

<div id="wrapper">
  <nav id="toc">
    <small><a style="font-style: italic" href="javascript:history.back()" title="">< back</a></small>
  </nav>
  <div id="content-container">
    <section>
      <a name="workshop"></a>
      <img class="section-image" src="{{ site.url }}/assets/images/convert.svg" alt="Convert image">
      <h2 class="section-header">Workshop: DynamoDB, Lambda, API Gateway, and S3</h2>
      <p>In this section, we'll rewrite our Lambda functions to use the SDK to access DynamoDB data and return it to our API Gateway.</p>
    </section>
    <hr>
    <section>
      <h2>Prerequisites</h2>
      <p>Before beginning this workshop, make sure that you've completed the <a href="{{ site.url }}/aws1/lessons/5_workshop.html">workshop from Session 5</a>.</p>
      <p>Make sure to keep the website URL for your static site handy, as you'll be testing out the update to your Lambda function through the live site.</p>
    </section>
    <hr>
    <section>
      <h2>Using DynamoDB to Pull All Table Data</h2>
      <ol>
        <li>First, we'll need to grant the role permission to access DynamoDB. To do this, go to your IAM dashboard and select Roles:</li> 
        <img class="screenshot" src="{{ site.url }}/assets/images/rolesiam.png" alt="roles on iam dashboard">
        <li>Choose the role for your <b>allPets</b> Lambda function. It should look something like <b>allPets-role-icpb4jiw</b>.</li>
        <li>Click the blue <b>Attach Policies</b> button.</li>
        <img class="screenshot" src="{{ site.url }}/assets/images/attachpolicy.png" alt="attach policy button">
        <li>In the search bar, type <b>dynamo</b> and select the policy that says <b>FullAccess</b>. In real life, you would want to create a custom policy that only gives the Lambda function the limited permissions that it needs for this function, but in order to get up and running, we won't be writing a custom policy for this exercise.</li>
        <img class="screenshot" src="{{ site.url }}/assets/images/dynamoaccess.png" alt="dynamodb access policy">
        <li>Click the blue <b>Attach Policy</b> button at the bottom. You're done adding a policy!</li>
        <li>Navigate back to your Lambda dashboard and open your <code>allPets</code> Lambda function.</li>
        <li>Inside of the code, we'll first require the AWS Ruby SDK (which gives us the ability to write code to interact with the database), then we'll create a DynamoDB client object and use that to return everything in the table with a scan operation:</li>
        <pre>require 'aws-sdk-dynamodb'

def lambda_handler(event:, context:)
    dynamodb = Aws::DynamoDB::Client.new(region: 'us-east-1')
    result = dynamodb.scan({ table_name: 'pets' })
    { body: result }
end
</pre>
        <li>Once you've updated the code, click the orange <b>save</b> button, open up the URL for your S3-hosted static site and test out your All Pets button. Tada! You should see something kind of ugly appear on the page, like this: <code>{:items=>[{"id"=>0.2e1, "name"=>"susan"}, {"id"=>0.1e1, "name"=>"bob"}], :count=>2, :scanned_count=>2, :last_evaluated_key=>nil, :consumed_capacity=>nil}</code></li>
        <li>OPTIONAL: If you're bothered by the way your data is showing up on your static page, you're welcome to change the JavaScript in <code>app.js</code> since this would be the frontend client's job to figure out how to parse and display the data. <b>However, that is not the focus of this workshop, so feel free to leave it in raw form.</b></li>
      </ol>
    </section>
    <hr>
    <section>
      <h2>Inserting an Item in a DynamoDB Table</h2>
      <p>Next, we'll update the code to create a pet in <code>createPet</code>.</p> 
      <ol>
        <li>We'll need to grant the role permission to access DynamoDB. <b>This is the same process that you did above, but for the createPet function</b>. Make sure to do this before moving on!</li>
        <li>Then, update the code:</li>
        <pre>require 'aws-sdk-dynamodb'

def lambda_handler(event:, context:)
    dynamodb = Aws::DynamoDB::Client.new(region: 'us-east-1')

    dynamodb.put_item({
      table_name: 'pets',
      item: {
        "id" => event["id"].to_i,
        "name" => event["name"]
      }
    })
    { body: "You created pet #{event["id"]} named #{event["name"]}" }
end</pre>
        <p><i>(In actuality, you probably wouldn't return a body for an update. However, because we want to see this appear on our page, we'll return the body and our JavaScript will append that message.)</i></p>
        <li>Check whether your function works by clicking the button from your static site.</li>
        <li>OPTIONAL: If you're bothered by the way your data is showing up on your static page, you're welcome to change the JavaScript in <code>app.js</code> since this would be the frontend client's job to figure out how to parse and display the data. <b>However, that is not the focus of this workshop, so feel free to leave it in raw form.</b></li>
      </ol> 
    </section>
    <hr>
    <section>
      <h2>Finding One Specific Item from a DynamoDB Table</h2>
      <p>Finally, we'll change the code to find a specific pet by ID in <code>onePet</code>.</p>
      <ol>
      <li>We'll need to grant the role permission to access DynamoDB. <b>This is the same process that you did above, but for the onePet function</b>. Make sure to do this before moving on!</li>
      <li>Then, update the code:</li>
      <pre>require 'aws-sdk-dynamodb'

def lambda_handler(event:, context:)
    dynamodb = Aws::DynamoDB::Client.new(region: 'us-east-1')
    id = event["id"].to_i
    result = dynamodb.get_item({
      table_name: 'pets',
      key: { 'id' => id }
    })
    { body: "You found #{result[:item]}" }
end</pre>
        <li>Check whether your function works by going to your static site, entering the ID of a pet that exists in your database, then clicking button.</li>
        <li>OPTIONAL: If you're bothered by the way your data is showing up on your static page, you're welcome to change the JavaScript in <code>app.js</code> since this would be the frontend client's job to figure out how to parse and display the data. <b>However, that is not the focus of this workshop, so feel free to leave it in raw form.</b></li>
      </ol>
    </section>
    <hr>
    <section>
      <h2>Done!</h2>
      <p>At this point, return back to the <a href="{{ site.url }}/aws1/lessons/6.html#cleanup">main Session 6 page</a> for cleanup instructions.</p>
    </section>
  </div>
</div>