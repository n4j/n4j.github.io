---
layout: post
title:  "AWS API Gateway + Lambda gotchas"
date:   2016-03-26 14:19:10 -0500
categories: [AWS, AWS API Gateway, Lambda]
keywords: "AWS, AWS API Gateway, Lambda"
---
# AWS API Gateway + Lambda gotchas

Combining `AWS API Gateway` with `AWS Lambda` makes it possible to create a serverless backend architecture. While this has opened a whole new bunch of possiblities in terms of scaling vis-a-vis cost saving, there are several technical and architectural gotchas one must address to fully utilize them.

>This post talks about pitfalls I encountered while working with API Gateway and Lambda, if you are new to this subject then read this nice blogpost from TechCrunch [here](http://techcrunch.com/2015/11/24/aws-lamda-makes-serverless-applications-a-reality/)

### The IAM role assumption blues

Your API gateway endpoint needs to *assume* the IAM role which you had provided and API Gateway, like any other service, must have permission to assume this role. AWS documentation clearly mentions this, however, it's easy to overlook this while in haste.

* Open the IAM console and select the role which you want your API to assume for Lambda execution
* Click on Trust Relationship tab and click on edit, on the screen which follows allow API Gateway service Id "apigateway.amazonwaws.com" to assume this IAM role by adding the following statement

{% highlight javascript %}
	{
		"Sid": "",   
		"Effect": "Allow",
		"Principal": {
		"Service": "apigateway.amazonaws.com"
		},
		"Action": "sts:AssumeRole"  
	}
{% endhighlight %}

Also, if you are using other AWS services like CloudWatch or S3, then your IAM role should have permission to execute these services too.

### Your nodejs zip package must have `package.json` file

For NodeJS if you are uploading a zip file containing source code then your zip file must contain a package.json file and it must indicate the **main** source code file. Otherwise AWS Lambda will probably throw an error indicating that it could not find the `index.handler` (or whatever function name configured as the event handler). For e.g., assuming your main file is index, add this line to your `package.json`
`"main": "index.js"`

### Limited disk access
While Lambda does allow you to upload custom binaries in your package and execute them, it does not allow you to read local files or access any other directories except `/tmp`. If you attempt to do so you would encounter an `EOPEN`  while opening a file and `EACCES` while creating directories.  

Also, the storage space provided by Lambda is ephemeral and limited to **`500 MBs`** at this time, for persistence storage AWS recommends using S3 but any other cloud storage solution like Azure Blob Storage can be used



# Further Reading
[Better Together: Amazon ECS and AWS Lambda](https://forums.aws.amazon.com/thread.jspa?messageID=595370)
