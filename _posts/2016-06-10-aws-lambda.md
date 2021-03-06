---
layout: post
title: "[AWS Lambda] In interaction with AWS S3"
date: 2016-06-10 12:49:32.000000000 +07:00
type: post
published: true
status: publish
categories:
- Cloud Computing
- Tech
- News
tags:
- AWS
- Lambda
meta:
  _wpcom_is_markdown: '1'
  _oembed_86eb2db72a8dd859b7b6d90ecf132835: "{{unknown}}"
  _oembed_4c1ceac0fd46cd98ba3b7aaec44a2f2b: "{{unknown}}"
  _edit_last: '61498925'
  _oembed_148498a93369048392209abb93ac0b00: "{{unknown}}"
  geo_public: '0'
  _publicize_job_id: '23699971655'
  _oembed_4efecfeed6e658194085dadf7dc5d00a: "{{unknown}}"
  _oembed_f526962253d2e9961f9a6669e0458f49: "{{unknown}}"
  _oembed_686cb87b1f4d2757099d4aa6509db478: "{{unknown}}"
author:
  login: vnstack
  email: vietstack@gmail.com
  display_name: vietstack
  first_name: Viet
  last_name: Stack
---
<p>The purpose of this post is nothing else just make an overview of interaction between AWS Lambda and AWS S3. You can easily search on the Internet about the information about what AWS Lambda and AWS S3 are, I just wanna make it understandable in an easy way like below:</p>
<ul>
<li>AWS Lambda is a PaaS solution. Imagine that you are a developer, you write a bunch of code line to do something and you need an environment to test it. AWS provide that enviroment for you. For example, write your code in local or in any AWS instance, create a zip file including your source code and its dependencies then upload it to the AWS Lamda service. In the AWS Lamda service, you can test your code monotonely or in the interaction with other services such as S3, Kinesis, DynamoDB, etc. Altenatively, you can copy/paste directly your source code into AWS Lambda GUI. The point here that you do not need to take care about the necessary resources (CPU, RAM, network, etc.) to run your code, AWS Lambda will do it for you.</li>
<li>
<p>AWS S3 is a storage solution. It is simple a storage, which is out there somewhere that you can use to store whatever you want.</p>
</li>
</ul>
<p>&nbsp;</p>
<p>In this post, I am going to show you the prototype of interaction between AWS Lambda and AWS S3. The testing procedure is below:</p>
<ul>
<li>Create an Ubuntu instance.</li>
</ul>
<li>
<p>Create two buckets in the AWS S3:</p>
</li>
<p style="padding-left:30px;">- Source bucket called “tutj” where I store an image called “cover.jpeg”</p>
<p style="padding-left:30px;"><a href="https://vietstack.files.wordpress.com/2016/06/cover.jpg"><img class="aligncenter  wp-image-760" src="{{ site.baseurl }}/pictures/cover.jpg" alt="cover" width="542" height="364" /></a></p>
<p style="padding-left:30px;text-align:center;">Picture 1. cover.jpeg</p>
<p style="padding-left:30px;">- Destination bucket called “tutjresized” where I am going to nothing else just copy that image “cover.jpeg”. Actually, you can do anything you want with image (resize, change color, etc.) then store it to the destination bucket.</p>
<p style="padding-left:30px;text-align:center;"><a href="https://vietstack.files.wordpress.com/2016/06/s3_bucket.png"><img class="aligncenter size-full wp-image-762" src="{{ site.baseurl }}/pictures/s3_bucket.png" alt="S3_bucket" width="756" height="394" /></a></p>
<p style="padding-left:30px;text-align:center;">Picture 2. AWS S3 buckets</p>
<ul>
<li>Access to that instance, install AWS CLI and use it to create a Lambda function:</li>
</ul>
<p style="padding-left:30px;">Function-name: “CopyImage”</p>
<p style="padding-left:30px;">Source code and dependencies: “/home/ubuntu/CopyImage.zip”</p>
<p style="padding-left:30px;">Handler: “CopyImage.handler”. It will be called each time AWS S3 triggers AWS Lambda.</p>
<p style="padding-left:30px;">Profile: “tutj”. The AWS User.</p>
<p style="padding-left:30px;"><span style="color:#ff0000;">$ sudo aws lambda create-function --region us-east-1 --function-name CopyImage –zip-file fileb:///home/ubuntu/CopyImage.zip –role arn:aws:iam::856421922278:role/lambda-s3-execution-role --handler CopyImage.handler --runtime python2.7 --profile tutj --timeout 10</span></p>
<p style="padding-left:30px;text-align:center;"><a href="https://vietstack.files.wordpress.com/2016/06/lambda_function.png"><img class="aligncenter  wp-image-761" src="{{ site.baseurl }}/pictures/lambda_function.png" alt="Lambda_function" width="690" height="347" /></a></p>
<p style="text-align:center;">Picture 3. AWS Lambda CopyImage function</p>
<ul>
<li>After testing the “CopyImage” function, the “cover.jpeg” is moved to the new destination bucket “tutjresized”.</li>
</ul>
<p>&nbsp;</p>
<p><a href="https://vietstack.files.wordpress.com/2016/06/tutjresized.png"><img class="aligncenter size-full wp-image-763" src="{{ site.baseurl }}/pictures/tutjresized.png" alt="tutjresized" width="756" height="389" /></a></p>
<p style="text-align:center;">Picture 4. Image in the destination bucket</p>
<p>&nbsp;</p>
<p>For more information, follow this below link on AWS:</p>
<p>http://docs.aws.amazon.com/lambda/latest/dg/with-s3.html</p>
