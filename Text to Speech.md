---
layout: post
title: Text to Speech Tutorial
permalink: /texttospeech/
published: true
---

The Text to Speech service processes and converts text to natural sounding speech. The input text can be of different languages and the output speech would be complete with correct intonation.

Download Powerpoint Presentation [here](https://github.com/JaoColoma/jaocoloma.github.io/blob/master/presentation.pptx?raw=true)

In this tutorial you will deploy a sample JSP application which uses the Text to Speech Service. 


####Copy Sample Application
You will download a copy of a sample application that you will deploy in your Bluemix account.

1.Create a `t2s` directory in the Desktop.

2.Download [t2s.war](https://github.com/JaoColoma/jaocoloma.github.io/blob/cb8814c95cd14e5d5e7afb5571801c9797b767df/t2s.war?raw=true).

3.Move the war file to the `t2s` directory.

<br>

####Deploy Sample Application in Bluemix using the `cf` tool.

1. Open a terminal window and go to the `t2s` directory in the desktop.

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
	> cf push t2s-<your_name> -m 256M -p t2s.war
	```

	**Example:**
		
	```text
	> cf push t2s-coloma -m 256M -p t2s.war
	```
	-m to specify the memory to be allocated for the application being pushed (256M)
	
	-p to specify the file to be uploaded (t2s.war)
	
	-<your-name> is for you to have a unique bluemix url

	<br>
	
1. Go to IBM Bluemix Website and Login .  Once logged in, click `DASHBOARD`.  

	The `Applications` section of your dashboard shows a widget representing the application `t2s-<your_name>` you deployed earlier.

	
	<br>
	
1. Click the widget of your application to see its overview.
	
1. Click the `ADD A SERVICE OR API` link.  You will be redirected to the `Catalog` page. 

1. Look for the `Text to Speech` service and click it.

1. In the `Service name` text box, type `Text to Speech - <your-name>`.

1. Click the `CREATE` button.

1. When asked to restage your application, click the `RESTAGE` button.  Wait for your application to restage.

1. Open another browser tab (do not close the browser tab containing your Bluemix account).  Go to `http://t2s-<your_name>.mybluemix.net` to test if the sample application can already connect to the Text to Speech service.

1. In the text box, type any word or sentence that you want to synthesize. (For this tutorial, we can only use English words and sentences, but the service can synthesize Brazilian Portuguese, English, French, German, Italian, Japanese, and Spanish)
	
1. Click the `Submit` button.  
2. After clicking the submit button, a file will be downloaded automatically.
3. Locate the downloaded file and rename it and append a `.wav` at the end of the file name.
4. Play the wav file using any audio player.

	<br>

####Analyze how the Text to Speech Application works

The code shownbelow is the source code found in the Text to Speech Servlet:


	```
		// Part 1
			TexttoSpeechConnector connector = new TexttoSpeechConnector();      
  			TextToSpeech service = new TextToSpeech();
  			service.setUsernameAndPassword(connector.getUsername(),connector.getPassword());
        	// Part 2
        		String text = request.getParameter("inputText");
        		String format = "audio/wav";

  			InputStream speech = service.synthesize(text, format);
  		// Part 3
            		OutputStream output = response.getOutputStream();

			    byte[] buf = new byte[2046];
				int len;
				while ((len = speech.read(buf)) > 0) {
					output.write(buf, 0, len);
				}
                        
                response.setContentType("audio/wav");
    
				//ServletContext ctx = getServletContext();  
				response.setHeader("Content-disposition","attachment;filename=output.wav");  
 
				OutputStream os =output;   
                                
				os.flush();  
				os.close();  
	```
	Part 1 shows the connection to the service. The credentials are obtained and are passed to the service.
	Part 2 shows the input to output process. In the first part, the text is retrieved from the index.jsp and the format is set to a wav file. After which, it is passed to the service with the use of the service.synthesize method. The parameters needed are the text and the file format, which are both strings. It returns an InputStream.
	Part 3 shows the download process. An OutputStream is created and the InputStream is written there. The OutputStream is then flushed in order for the file to be downloaded.

####Delete the sample application for housekeeping

1. Go to Bluemix.
2. Click Dashboard.
3. From the Applications List, click the gear icon of the created application.
4. Choose Delete App.

<br>

####End of Tutorial

