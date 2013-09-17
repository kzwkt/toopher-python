#ToopherPython

[![Build Status](https://travis-ci.org/toopher/toopher-python.png?branch=master)](https://travis-ci.org/toopher/toopher-python)

#### Introduction
ToopherPython is a Toopher API library that simplifies the task of interfacing with the Toopher API from Python code.  This project wrangles all the required OAuth and JSON functionality so you can focus on just using the API.

#### Learn the Toopher API
Make sure you visit (http://dev.toopher.com) to get acquainted with the Toopher API fundamentals.  The documentation there will tell you the details about the operations this API wrapper library provides.

#### OAuth Authentication
First off, to access the Toopher API you'll need to sign up for an account at the developers portal (http://dev.toopher.com) and create a "requester". When that process is complete, your requester is issued OAuth 1.0a credentials in the form of a consumer key and secret. Your key is used to identify your requester when Toopher interacts with your customers, and the secret is used to sign each request so that we know it is generated by you.  This library properly formats each request with your credentials automatically.

#### The Toopher Two-Step
Interacting with the Toopher web service involves two steps: pairing, and authenticating.

##### Pair
Before you can enhance your website's actions with Toopher, your customers will need to pair their phone's Toopher app with your website.  To do this, they generate a unique, nonsensical "pairing phrase" from within the app on their phone.  You will need to prompt them for a pairing phrase as part of the Toopher enrollment process.  Once you have a pairing phrase, just send it to the Toopher web service and we'll return a pairing ID that you can use whenever you want to authenticate an action for that user.

##### Authenticate
You have complete control over what actions you want to authenticate using Toopher (for example: logging in, changing account information, making a purchase, etc.).  Just send us the user's pairing ID, a name for the terminal they're using, and a description of the action they're trying to perform and we'll make sure they actually want it to happen.

#### Librarified
This library makes it super simple to do the Toopher two-step.  Check it out:

```python
import toopher

# Create an API object using your credentials
api = toopher.ToopherApi("<your consumer key>", "<your consumer secret>")

# Step 1 - Pair with their phone's Toopher app
pairing_status = api.pair("pairing phrase", "username@yourservice.com")

# Step 2 - Authenticate a log in
auth = api.authenticate(pairing_status.id, "my computer")

# Once they've responded you can then check the status
auth_status = api.get_authentication_status(auth.id)
if (auth_status.pending == false and auth_status.granted == true):
	# Success!
```

#### Handling Errors
If any request runs into an error a `ToopherApiError` will be thrown with more details on what went wrong.

#### Dependencies
This library uses the python-oauth2 library to handle OAuth signing and httplib2 to make the web requests.  If you install using pip (or easy_install) they'll be installed automatically for you. 

#### Try it out
Check out `demo.py` for an example program that walks you through the whole process!  Just download the contents of this repo, make sure you have the dependencies listed above installed, and then run it like-a-this:
```shell
$ python ./demo.py
```
