---
title: Cloud 9 Setup
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
      <img class="section-image" src="{{ site.url }}/assets/images/cloud9.png" alt="Cloud9 logo">
      <h2 class="section-header">Cloud 9: AWS's Online IDE</h2>
      <p>AWS provides an online Integrated Development Environment (or IDE). Follow these instructions to instantly have access to all of the tools you will need to build a Ruby application.</p>
      <ol>
        <li>Find "Cloud9" in the AWS services menu.</li>
        <li>Click the orange "Create Environment" button.</li>
        <li>Give it a name (MyCloud9Environment, for example) and click Next.</li>
        <li>Don't change any settings on the "Configure Settings" screen; just click next again.</li>
        <li>On the confirmation screen, click "Create Environment."</li>
        <li>Near the bottom, you'll see the command line. Type <code>rvm install 2.5.5</code> to get the correct version of Ruby. This will take a few minutes.</li>
        <img class="screenshot" src="{{site.url}}/assets/images/rvminstall.png" alt="Screenshot of Cloud9 terminal">
      </ol>
      <p><b>You're done!</b> Head back over to the Lambda workshop page to get started.</p>
    </section>
    <hr>
  </div>
</div>