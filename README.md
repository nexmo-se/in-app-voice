# In-app Voice

This repository includes a sample WebRTC client code in JavaScript using Vonage Client SDK, as well as the corresponding sample server code.

Using a WebRTC client is also known as in-app voice.

The sample code here allows a WebRTC client to receive calls from PSTN, to make calls to PSTN, and make or receive calls with another WebRTC client.

## Set up

### Local deployment using ngrok

If you plan to test using `Local deployment with ngrok` (Internet tunneling service), here are the instructions to set up ngrok:<br>
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
				hostname: setahostnamehere.ngrok.io</br>
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
run this server application (from this repository), e.g.</br>
	*`myappname.herokuapp.com`, `myserver.mycompany.com:40000`*</br>

For Heroku deployment, see more details in the next section **Command Line Heroku deployment**.

### Set up your Vonage Voice API application credentials and phone number

[Log in to your](https://ui.idp.vonage.com/ui/auth/login) or [sign up for a](https://ui.idp.vonage.com/ui/auth/registration) Vonage API account.

Go to [Your applications](https://dashboard.nexmo.com/applications), access an existing application or [+ Create a new application](https://dashboard.nexmo.com/applications/new).

Under **Capabilities** section (click on [Edit] if you do not see this section):

Enable Voice
- Under Answer URL, leave HTTP GET, and enter https://\<host\>:\<port\>/voice/answer (replace \<host\> and \<port\> with the public host name and if necessary public port of the server where this sample application is running), e.g.</br>
*https://yyyyyyyy.ngrok.io/voice/answer*</br>
or
*https://myappname.herokuapp.com/voice/answer*</br>
or
*https://myserver2.mycompany.com:40000/voice/answer*</br>
- Under Event URL, **select** **_HTTP POST_**, and enter https://\<host\>:\<port\>/voice/event (replace \<host\> and \<port\> with the public host name and if necessary public port of the server where this sample application is running), e.g.</br>
*https://yyyyyyyy.ngrok.io/voice/event*</br>
or
*https://myappname.herokuapp.com/voice/event*</br>
or
*https://myserver2.mycompany.com:40000/voice/event*</br>

- Click on [Generate public and private key] if you did not yet create or want new ones, then save as **.private.key** file (note the leading dot in the file name) in this application folder.</br>
**IMPORTANT**: Do not forget to click on [Save changes] at the bottom of the screen if you have created a new key set.</br>
- Link a phone number to this application if none has been linked to the application.

Please take note of your **application ID** and a **linked phone number** (as they are needed in the very next section.)

Note: You may have more than one phone number linked to your application if needed.

For the next steps, you will need:</br>
- Your [Vonage API key](https://dashboard.nexmo.com/settings) (as **`API_KEY`**)</br>
- Your [Vonage API secret](https://dashboard.nexmo.com/settings), not signature secret, (as **`API_SECRET`**)</br>
- Your `application ID` (as **`APP_ID`**),</br>
- A **`phone number linked`** to your application (as **`SERVICE_NUMBER`**), external parties will **call that number**.</br>

### Set up the server application

Copy or rename env-example to .env<br>
Update parameters in .env file<br>
Have Node.js installed on your system, this application has been tested with Node.js version 18.19.1<br>
Install node modules with the command "npm install"<br>
Start the server application with the command "node in-app-voice-server"<br>
This Node.js server application (this repository) is running on local port 8000.</br>

### Command Line Heroku deployment

You must first have deployed your application locally, as explained in previous section, and verified it is working.

Install [git](https://git-scm.com/downloads).

Install [Heroku command line](https://devcenter.heroku.com/categories/command-line) and login to your Heroku account.

If you do not yet have a local git repository, create one:</br>
```bash
git init
git add .
git commit -am "initial"
```

Start by creating this application on Heroku from the command line using the Heroku CLI:
*Note: In following command, replace "myappname" with a unique name on the whole Heroku platform*

```bash
heroku create myappname
```

On your Heroku dashboard where your application page is shown, click on `Settings` button,
add the following `Config Vars` and set them with their respective values:</br>
API_KEY</br>
API_SECRET</br>
APP_ID</br>
SERVICE_NUMBER</br>
PRIVATE_KEY_FILE with the value **./.private.key**</br>

Now, deploy the application:


```bash
git push heroku master
```

On your Heroku dashboard where your application page is shown, click on `Open App` button, that hostname is the one to be used under your corresponding [Vonage Voice API application Capabilities](https://dashboard.nexmo.com/applications) (click on your application, then [Edit]).</br>

For example, the respective links would be (replace *myappname* with actual value):</br>
https://myappname.herokuapp.com/answer</br>
https://myappname.herokuapp.com/event</br>

See more details in above section **Set up your Vonage Voice API application credentials and phone number**.


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




