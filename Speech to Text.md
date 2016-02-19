---
layout: post
title: Speech to Text Tutorial
permalink: /speechtotext/
published: true
---

The Speech to Text service processes and converts human voice to text form.

In this tutorial you will deploy a sample JSP application which uses the Speech to Text Service. 


####Copy Sample Application
You will download a copy of a sample application that you will deploy in your Bluemix account.

1.Create a `s2t` directory in the Desktop.

2.Download [s2t.war](https://github.com/JaoColoma/jaocoloma.github.io/blob/master/s2t.war?raw=true).

3.Move the war file to the `s2t` directory.

<br>

####Deploy Sample Application in Bluemix using the `cf` tool.

1. Open a terminal window and go to the `s2t` directory in the desktop.

2. Login to your Bluemix account using the `cf` tool.

	```text
	> cf login -a https://api.ng.bluemix.net -s dev
	```
	
	>When asked for a username (e-mail address) and password, enter the username and password of your Bluemix account.
	
	>-a to specify the API endpoint (https://api.ng.bluemix.net).
	
	>-s to specify space name (dev).

	<br>
	
3. Upload the sample application to your Bluemix account.

	```text
	> cf push s2t-<your_name> -m 256M -p s2t.war
	```

	**Example:**
		
	```text
	> cf push s2t-coloma -m 256M -p s2t.war
	```
	-m to specify the memory to be allocated for the application being pushed (256M)
	
	-p to specify the file to be uploaded (s2t.war)
	
	-<your-name> is for you to have a unique bluemix url

	<br>
	
1. Go to IBM Bluemix Website and Login .  Once logged in, click `DASHBOARD`.  

	The `Applications` section of your dashboard shows a widget representing the application `s2t-<your_name>` you deployed earlier.

	
	<br>
	
1. Click the widget of your application to see its overview.
	
1. Click the `ADD A SERVICE OR API` link.  You will be redirected to the `Catalog` page. 

1. Look for the `Speech to Text` service and click it.

1. In the `Service name` text box, type `Speech to Text - <your-name>`.

1. Click the `CREATE` button.

1. When asked to restage your application, click the `RESTAGE` button.  Wait for your application to restage.

1. Open another browser tab (do not close the browser tab containing your Bluemix account).  Go to `http://s2t-<your_name>.mybluemix.net` to test if the sample application can already connect to the Speech to Text service.

1. Choose the file that you want to upload.
	
1. Click the `Upload` button.  
2. After clicking the upload button, the output text will be shown.

	<br>

####Analyze how the Speech to Text Application works

The code shownbelow is the source code found in the Speech to Text Servlet:


	```
	//Part 1
	   if(ServletFileUpload.isMultipartContent(request)){
            try {
                List<FileItem> multiparts = new ServletFileUpload(
                                         new DiskFileItemFactory()).parseRequest(request);
              
                for(FileItem item : multiparts){
                    if(!item.isFormField()){
                       
                       // Part 2 
                        SpeechtoTextConnector connector = new SpeechtoTextConnector();
                        SpeechToText service = new SpeechToText();
                        service.setUsernameAndPassword(connector.getUsername(),connector.getPassword());
                        
                        // Part 3
                        InputStream in = item.getInputStream();
                        String PREFIX = "temporary";
                        String SUFFIX = ".wav";

                        File tempFile = File.createTempFile(PREFIX, SUFFIX);

                        FileOutputStream out = new FileOutputStream(tempFile);
                        IOUtils.copy(in, out);
        				// Part 4	
					 Map<String, Object> params = new HashMap<String, Object>() ;
     
					params.put("SpeechToText.AUDIO", tempFile);
					params.put("SpeechToText.CONTENT_TYPE", "audio/wav");
					params.put("SpeechToText.CONTINUOUS", "true");
					params.put("SpeechToText.WORD_CONFIDENCE", "");
					params.put("SpeechToText.MAX_ALTERNATIVES", "2");
					params.put("SpeechToText.CONTINUOUS", "true");
					params.put("SpeechToText.TIMESTAMPS", "true");
					params.put("SpeechToText.INACTIVITY_TIMEOUT", "-1");

		   // Part 5
                    SpeechResults transcript = service.recognize(params);
                    String type = transcript.toString();       
                	request.setAttribute("message", type);
	```
	Part 1 shows the retrieval of the file that was chosen.
	Part 2 shows the connection to the service. The credentials are obtained and are passed to the service.
	Part 3 since the retrieval of the file returns an InputStream, it is converted to a file because this is the needed parameter.
	Part 4 values are set for the SpeechToText input using a hashmap.
	Part 5 the hashmap is passed to the service.recognize method, which is the method that converts the input file to text. The 				method then returns a SpeechResult and that result is converted in to a string using the toString() function. 				After that, it is shown on the screen.

####Delete the sample application for housekeeping

1. Go to Bluemix.
2. Click Dashboard.
3. From the Applications List, click the gear icon of the created application.
4. Choose Delete App.

<br>

####End of Tutorial
