# SMS Starter Example

This is a simple sms application where a user can send an sms, subscribe sms notification and receive real-time sms events (inbound, outbound etc notification).

Strps for Installation

1. Copy `.env.example` and rename to `.env` and add the appropriate values.
   Example : 
   CLIENT_ID=abcde123-12a1-1a23-1234-123a12345a1a
   CLIENT_SECRET=123abcde-a123-1234-abcd-ab12345c67d8
   BASE_URL=https://oauth-cpaas.att.com
   PHONE_NUMBER=+16543219870


2. To install dependencies, run: 
   bundle install

3. To start the server, run:
   bundle exec rackup -p 6000

| ENV KEY       | Description                                                  |
| ------------- | ------------------------------------------------------------ |
| CLIENT_ID     | Private project key                                          |
| CLIENT_SECRET | Private project secret                                       |
| BASE_URL      | URL of the CPaaS server to use                               |
| PHONE_NUMBER  | Phone number purchased in CPaaS portal (sender phone number) |

### Section 1 - Send SMS

---

There are two fields in the form

1. Phone number - The phone number where the SMS is to be sent.
2. Message - A text message for the SMS

When clicked on `Send` button, an SMS is sent out where the `sender` phone number is the one add in `.env` file (PHONE_NUMBER) and `destination` is the one entered in the form.

### Section 2 - SMS notification subscription

---

This represents the subscription to SMS notification and can be found on the top right section. Here a Webhook host URL is to be added.

As incoming notifications are to be received by the local server, there is a requirement of a web server to be running and that web server to have a public IP address. So to use this it is recommended to install and use [ngrok](https://ngrok.com/).

#### How to use ngrok

Once `ngrok` is installed, run the following command
ngrok http 6000

3001 is the `PORT` that is used while running the software.
Once `ngrok` starts forwarding the `localhost`, you would find a similar kind of message in your screen.

Forwarding          -          http://29de1e3e.ngrok.io -> http://localhost:3001
Forwarding          -          https://29de1e3e.ngrok.io -> http://localhost:3001

After this the usage part of `ngrok` is done and we got out public domain, let's shift out attention to the notification subscription.

Copy the `forwarding` domain, for our case it is `https://29de1e3e.ngrok.io` from above and paste it to in the `Webhook host URL` input field in out `SMS starter app`.
Click `Subscribe` and a notification channel would be created with the above domain against the phone number that is described in the `.env` file (PHONE_NUMBER) and all the sms notifications would start coming in.

Note: While entering the ngrok domain and subscribing, make sure that there is not forward slash at the end of the domain.
**Correct** `https://29de1e3e.ngrok.io`
**Incorrect** `https://29de1e3e.ngrok.io/`

### Section 3 - SMS Notification

This is the bottom half of the right section of the application. As in `section 2` we subscribed for all the SMS notification against the phone number described in the `.env` file (PHONE_NUMBER), whenever we send out an sms using the `Send SMS` section or get an sms to the phone number described in the `.env` file (PHONE_NUMBER), we would receive a notification and that notification would be printed out under the `SMS Notification` header