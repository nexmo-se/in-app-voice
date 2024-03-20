# In-app Voice

This repository includes a sample WebRTC client code in JavaScript using Vonage Client SDK, as well as the corresponding sample server code.

Using a WebRTC client is also known as in-app voice.

The sample code here allows a WebRTC client to receive calls from PSTN, to make calls to PSTN, and make or receive calls with another WebRTC client.

## Set up

### Set up the server application

Copy or rename env-example to .env<br>
Update parameters in .env file<br>
Have Node.js installed on your system, this application has been tested with Node.js version 18.19.1<br>
Install node modules with the command "npm install"<br>
Start the server application with the command "node in-app-voice-server"<br>
This Node.js server application (this repository) is running on local port 8000.</br>

### Local deployment using ngrok

If you plan to test using `Local deployment with ngrok` (Internet tunneling service), here are the instructions to set up ngrok (Internet tunneling service):<br>
- [Install ngrok](https://ngrok.com/download),<br>
- Make sure you are using the latest version of ngrok and not using a previously installed version of ngrok,
- Sign up for a free [ngrok account](https://dashboard.ngrok.com/signup),<br>
- Verify your email address from the email sent by ngrok,<br>
- Retrieve [your Authoken](https://dashboard.ngrok.com/get-started/your-authtoken),<br>
- Run the command `ngrok config add-authtoken <your-authtoken>`<br>
- Set up the tunnel
	- Run `ngrok config edit`
		- For a free ngrok account, add following lines to the ngrok configuration file (under authoken line):</br>
		<pre><code>	
		tunnels:
			mytunnel:</br>
				proto: http</br>
				addr: 8000</br>
		</code></pre>
		- For a [paid ngrok account](https://dashboard.ngrok.com/billing/subscription), you may set a ngrok hostname that never changes on each ngrok new launch, add following lines to the ngrok configuration file (under authoken line) - set hostname to actual desired values:</br>
		<pre><code>	
		tunnels:
			mytunnel:</br>
				proto: http</br>
				addr: 8000</br>
				hostname: setahostnamehere.ngrok.io*</br>
		</code></pre>			
		
- Start the ngrok tunnel
	- Run `ngrok start mytunnel`</br>
	- You will see lines like
		....</br>
		*Web Interface                 http://127.0.0.1:4040</br>                             
		Forwarding                    https://xxxxxx.ngrok.io -> http://localhost:8000*</br> 
	- Make note of *https://xxxxxx.ngrok.io* (with the leading https://), as it will be needed in the next steps below.</br>	

Reminder: The Node.js server application (this repository) is running on local port 8000.</br>

### Non local deployment

If you are using hosted servers, for example Heroku, your own servers, or some other cloud provider,
you will need the public hostnames and if necessary public ports of the servers that
run this server application (from this repository),</br>
e.g.</br>
	*`myappname.herokuapp.com`, `myserver.mycompany.com:40000`*</br>

For Heroku deployment, see more details in the next section **Command Line Heroku deployment**.  

## WebRTC client

The sample WebRTC client in this repository is based on Vonage Client SDK.</br>

Open it in a web browser using the address:</br>

https://<myserver-address>/client.html</br>

e.g.</br>
https://xxxxxx.ngrok.io/client.html</br>
or</br>
https://myappname.herokuapp.com/client.html</br>
or</br>
https://myserver.mycompany.com:40000/client.html</br>

You are ready to:</br>
- Receive PSTN calls,</br>
- Make PSTN calls,</br>
- Receive or place a call to/from another WebRTC client (opened from the same link - logged in as a different user).</br>




