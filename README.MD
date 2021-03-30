# What is IR-with-Alexa ?
I will walk you through the process of creating an automation around doing basic Incident Response in AWS with the help of Alexa. Let's say, you have your own Ec2 instance running in AWS which is hosting your website, and one day you found it is down due to some reason (may be an attack?!), so you just need to instruct Your Alexa Device (Echo Dot, or anything else you have with you) to revibe your server, and she will do the job for you.
* If you successfully deployed the project, kudos to you!! In return get me some subscribers for my YouTube Channel👉 https://www.youtube.com/c/BlackPerl
* If you like to support my creation and pay me back a little, you can buy me a coffee ☕ https://www.buymeacoffee.com/BlackPerl

# Setup
You will need the following to get started:
* Alexa Developer Account, if you don't have, create one here 👉 https://developer.amazon.com
* Some AWS Knowledge such as basic navigation around console, launching EC2 instances, basic knowledge of Lambda, IAM roles etc. And you are ready to roll!
* There may be some region limitations so for this project I worked out of ‘us-west-2’ region aka Oregon for the Lambda part.

# Let's Begin
## First Part- Alexa Skill

* First, I logged into the Alexa Developer Portal, selected Create Skill, and used the Start from scratch template.
* Under Invocation, the name used to start the Alexa skill, I decided to change it to BlackPerl IR (You can name it anything you like to give!!)
* I added three Intents — these are actions that fulfills a user’s spoken request.
* ✔ GatherServerIntent, used to gather a total number of running instances, utterance = status report
* ✔ TurnOffInstanceIntent, used to turn off the instances (obviously), utterance = turn off everything
* ✔ You can add as many Intents you want, but make sure you create the utterance and create appropriate Lambda function to cover that.
* After these were created, I saved the model. The next point required adding an Endpoint, located on the left-side menu, after Interfaces.
* Your Alexa Skill ID will be displayed here (remember this, you will need it soon). I paused here and switched over to the AWS Console to set up the Lambda function.

## Second Part- Lambda Function (switch to Lambda in the AWS console)

* Select Author from Scratch.
* Give the proper name (as you liked) for the Lambda Function.
* At the IAM section, if you don't have a already created IAM role for basic Lambda, select create Lambda and from AWS managed policy select basic Lambda Function.
* After it was created, I copied and pasted the whole ARN (Amazon Resource Name) of the function located in the top-right corner over to the Default Region section under the Endpoint tab in the Alexa’s Developer Portal.
* Then I copied the Skill ID and jumped back over to the Lambda Function in the AWS Console. I added an Alexa Skills Kit Trigger, pasted the Skill ID, and clicked Add. Now these are connected.
* **NOTE** Since I already know that getting an instance state for all ec2 instances across all regions takes more than 3 seconds, I updated the Timeout setting from 3 to 30 seconds. If this was not set, Alexa would timeout after 3 seconds while the script was running and say “There was a problem with ….”
* The last part of the lambda function is to upload the code. So take the lambda_python.py code, package it and upload in the Lambda section. I hope you know how to package it? 😉😉 If not, follow the below codes-
* ✔ virtualenv --python=/usr/local/bin/python3.7 .
* ✔ source bin/activate
* ✔ pip install boto3
* ✔ pip install ask-sdk
* ✔ deactivate
* ✔ cd lib/python3.7/site-packages
* ✔ vi lambda_function.py (paste my code here)
* ✔ zip -r9 lambda_package.zip .

## Third Part- The IAM Policy

* Now we need to ensure the Lambda Function has correct authorization to describe my ec2 instances and stop the running ones.
* I created a new policy. You can found the code in this repo, so just copy it and paste it in JSON.
* I took this policy and added it lambda_basic_execution role created earlier.


# Now to jump back over to the Alexa Developer Portal and select Build Model!! If all is working, we should be able to type "Run BlackPerl IR" and see the Magic happenning!! 
## I have added the Python Code to create/lanuch a new EC2 instance here. So feel free to take it and replicate the same above process to let alexa create/launch a new instance on behalf of you!! 

# Thanks for taking the time to read!!
