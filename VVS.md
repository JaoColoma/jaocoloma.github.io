---
layout: post
title: Voters Verification System
permalink: /vvs/
published: true
---

The Voters Verification System is a system that is used during elections, specifically during the voting period. Its main goal is to 
verify the votes casted by the voters. This verification is done by sending a text message to the voter, which contains the list of candidates that they voted. The target users of this program are the COMELEC and the voters. This will help the COMELEC
with the voting verification process. This can be used a long side with the printing of the voting receipt or it could also be used as an
alternative. Voters will also be more confident about the votes that they casted because they will be able to verify it and keep a record
of the candidates that they voted for.

Download Powerpoint Presentation [here](https://github.com/JaoColoma/jaocoloma.github.io/blob/master/presentation.pdf?raw=true)

#### Clone a copy of the sample program
1.Clone this repository on your computer using git: [VVP](https://github.com/JaoColoma/albacol.git).


<br>

#### Create a Twilio account
1. Go to [Twilio's website](https://www.twilio.com) and click the `Sign Up` button on the upper right

2.  Fill the registration up, and choose the following options in the drop down list

	||||
	|---|---|---|
	| **What are you building?** | SMS Alerts|
	| **Choose your language** | Java |
	| **Which product do you plan to use first?** | Programmable SMS |
	
	<br>
4. Click `Get Started`

5. Before you can proceed to using Twilio's services, Twilio has to verify one phone number from you, choose `Philippines` (area code +63) and in the textbox, the rest of your phone number (ex. 9xxxxxxxxx)

6. You will be redirected to a page with a textbox. Once you receive the verification code in your phone, enter it in the text box and proceed.

8. On the current page, click `Get your first Twilio Number` and click `Choose this number`. Take note of your new number and click `Done`. For Testing purposes, you must change the sender number in
the TwilioServlet File found in the directory of the cloned repository (albacol/src/main/java/net/tutorial/Servlet/TwilioServlet.java) in the line “params.put(“From”, “insert your twilio number here”)”. Ideally, this is not changed because the COMELEC already programmed its number here. Just to be clear, this will not be accessible to the public.

9.  In order for the Twilio service in Bluemix to work, it requires your Twilio account's `Auth Token` and `Account SID` both which can be found [here](https://www.twilio.com/user/account) by clicking `Show API Credentials`. Take note of both of these credentials, you will be using this when binding Twilio to your Bluemix app.

#### Setting Twilio up
Before you are able to send text messages through the app, there are some things you need to set up since Free users have some limitations.

1. To be able to send to Philippine numbers, you have to enable it [here](https://www.twilio.com/user/account/settings/international/sms).

<br>

####Deploy Sample Application in Bluemix 

1. Enter the directory of the cloned repository using the command prompt.

2. Enter the 'gradle assemble' command.

3. Login to your Bluemix account using the `cf` tool.

	```text
	> cf login -a https://api.ng.bluemix.net -s dev
	```
	
	>When asked for a username (e-mail address) and password, enter the username and password of your Bluemix account.
	
	>-a to specify the API endpoint (https://api.ng.bluemix.net).
	
	>-s to specify space name (dev).

	<br>
	
4. Upload the sample application to your Bluemix account.

	```text
	> cf push hackathon-<group-name> -m <mem> -p build/libs/myapp.war
	```

	**Example:**
		
	```text
	> cf push hackathon-albacol -m 256M -p build/libs/myapp.war
	```
	-m to specify the memory to be allocated for the application being pushed (256M)
	
	-p to specify the file to be uploaded (myapp.war)
	
	-<your-name> is for you to have a unique bluemix url

	<br>
	
5. Go to IBM Bluemix Website and Login .  Once logged in, click `DASHBOARD`.  

	The `Applications` section of your dashboard shows a widget representing the application `hackathon-<your_name>` you deployed earlier.
	
	<br>
	
6. Click the widget of your application to see its overview.
	
1. Click the `ADD A SERVICE OR API` link.  You will be redirected to the `Catalog` page. 

1. Look for the `SQL Database` service and click it.

1. In the `Service name` text box, type `sqlDB-<your-name>`.

1. Click the `CREATE` button.

1. When asked to restage your application, click the `Cancel` button. 

2. Click the 'Overview' option on the menu on the left. This will bring you back the Overview Page of your application.

1. Click the `ADD A SERVICE OR API` link.  You will again be redirected to the `Catalog` page. 

1. Look for the `Twilio` service and click it.

1. In the `Service name` text box, type `twilio-<your-name>`.

1. Enter the Accound SID and Authentication Token in their respective fields. Check credentials [here](https://www.twilio.com/user/account)

1. When asked to restage your application, click the `Restage` button. Wait for you application to restage.

1. Once done, you can now access your application. Enter http://hackathon-your-name.mybluemix.net/Homepage.jsp.


#### Note:

1. Before using the application, you must first register a voter. This can be done by accessing the CreateAccount page. You can access it
using http://hackathon-your-name.mybluemix.net/CreateAccount.jsp.

2. The Numbers that are to be used for registering must first be verified [here](https://www.twilio.com/user/account/phone-numbers/verified) by clicking `Verify a Number`.


####Delete the sample application for housekeeping

1. Go to Bluemix.
2. Click Dashboard.
3. From the Applications List, click the gear icon of the created application. In the Services tab, make sure that the services are selected. In the Routes tab, make sure that the route (i.e., URL) is selected.
4. Choose Delete App.

<br>

####End of Tutorial
