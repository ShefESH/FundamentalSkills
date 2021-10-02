# Fundamental Skills - Burp Suite

|Category|Experience Level|
|---|---|
|Web|Novice|

## Contents
- [[#Fundamental Skills - Burp Suite]]
  - [[#Introduction]]
  - [[#Setting up Burp Suite]]
    - [[#Proxying Traffic]]
    - [[#Adding the Certificate]]
  - [[#Intercept]]
    - [[#Traffic Filtering]]
  - [[#Repeater]]
  - [[#Intruder]]
  - [[#Cheatsheet]]
  - [[#Worksheet]]

## Introduction

To properly understand this lesson, we recommend first completing the [[Web 2 - Understanding HTTP Requests]] lesson if you are unfamiliar with HTTP requests.

Burp Suite is a powerful tool for inspecting HTTP traffic. It acts as a **proxy** between your *browser* and the *server*. It captures HTTP requests before they are sent to the server, and lets you **view** and **modify** their details on the fly.

What does this mean?
- you can see exactly what is happening when you visit a webpage; this includes request headers added automatically by your browser, request data, and server responses including redirects
- you can see 'behind the scenes requests' that you might not have been aware of
- you can edit requests before they reach the server - this allows you to experiment with site features, and potentially discover broken or unintended functionality on the server

Burp also has many other powerful features, such as the Repeater and Intruder modules which we will address in this lesson.

## Setting up Burp Suite

To download the free version of Burp Suite, visit [this page](https://portswigger.net/burp/communitydownload), click Download, and choose the most appropriate version for your Operating System. Then run any necessary installers.

When you launch Burp Suite, you will see the following screen:

![[Pasted image 20211001214048.png]]

Click 'Next' to start a temporary project, then 'Start Burp' on the next screen.

You should now see the Burp Homepage:

![[Pasted image 20211001214133.png]]

### Proxying Traffic

By default, Burp sets up a HTTP proxy on address 127.0.0.1 (aka localhost, the address of your computer) and port 8080. You can see the proxy settings in the *Proxy > Options* tab:

![[Pasted image 20211001214356.png]]

There are lots of options, but we will focus on the first one. This lets us set the port and address to listen on. We can add, edit, and remove proxies with the buttons on the left, and enable/disable them with the checkbox.

To direct requests from your browser to your proxy, you must instruct the browser to point at Burp. I will show Firefox in this lesson, but PortSwigger have written a [guide for Google Chrome](https://portswigger.net/burp/documentation/desktop/external-browser-config/browser-config-chrome) if that's the browser you prefer.

In Firefox, navigate to `about:preferences` in the URL bar. This will open your settings menu. Scroll to the bottom where the Network Settings are, and click 'Settings':

![[Pasted image 20211001214748.png]]

Now add proxy settings to match the Proxy Listener settings in Burp:

![[Pasted image 20211001214840.png]]

You can also use the browser extension FoxyProxy to more easily configure, enable, and disable proxies - install it by visiting the [extension page](https://addons.mozilla.org/en-GB/firefox/addon/foxyproxy-standard/) and clicking 'Add to Firefox':

![[Pasted image 20211001215007.png]]

If you commonly use private browsing mode, you must also enable it to run in those windows:

![[Pasted image 20211001215036.png]]

You can close the window that opens, then click the extension in the top-right:

![[Pasted image 20211001215121.png]]

Click 'Options' to add a proxy, then fill in the details to match the listener:

![[Pasted image 20211001215221.png]]

Click 'Save', then you can click the extension again and click your new proxy to enable it and start directing traffic:

![[Pasted image 20211001215333.png]]

Now, when you visit a webpage you will see the request pop up in the Burp Suite *Proxy > Intercept* window:

![[Pasted image 20211001215536.png]]

Burp will now 'hold' the traffic until you either click 'Forward' or turn Intercept off. After it passes through Burp it will go to the server, and you can view the history in the 'HTTP History' tab. We'll cover Intercept in more detail later in this lesson.

If you get a message in your browser (or any other tool) saying that the "Proxy Server is refusing connections", you probably haven't launched Burp or enabled the proxy listener.

If you see an 'Unknown Host' error, you might not have an internet connection:

![[Pasted image 20211002101641.png]]

### Adding the Certificate

If you try to visit a HTTPS website with Burp Suite running, you may be warned by your browser or Antivirus that the certificate is untrustworthy.

To avoid this warning, you must import Burp's certificate into your browser. To get the certificate, navigate to `localhost:8080` in your browser while Burp is running (or `localhost:[PORT]` if you're using a different port to listen on) and click the 'CA Certificate' button:

![[Pasted image 20211001220004.png]]

This will download the `cacert.der` file. You can now import this into your browser by navigating to `about:preferences#privacy` in browser and scrolling to the Certificates section:

![[Pasted image 20211001220110.png]]

Click 'View Certificates', then 'Import', then select the downloaded certificate and tell Firefox to trust it to identify websites:

![[Pasted image 20211001220154.png]]

Click 'OK'. Burp should now work normally over HTTPS.

## Intercept

The Intercept tab is where you can inspect HTTP/S traffic before forwarding it to the server.

In the tab you can view all elements of the request, including headers and request body:

![[Pasted image 20211001215536.png]]

The HTTP history tab shows *all* previous requests, and their responses:

![[Pasted image 20211001223638.png]]

To modify a request before it hits the server, you can hold it in the Intercept tab (by leaving Intercept on and not forwarding the request), then send it to the Repeater by pressing `Ctrl + R`. Once your request is in the Repeater, click drop on the Intercept tab to stop the original request from sending. You can now modify the request.

For a practical example of this, watch [this video](https://www.youtube.com/watch?v=yQl5RA6APyQ&t=860s) on the Academy HacktheBox machine.

### Traffic Filtering

You may find that your HTTP History gets clogged with a lot of other requests, especially if you're using Burp Suite from your day-to-day machine.

To prevent this, you can add a certain website to your 'scope'. Visit the 'Target' tab, where you can see all websites that Burp has captured traffic for:

![[Pasted image 20211002101806.png]]

Right-click the website you are testing (in this case, juice-shop.herokuapp.com) and click 'Add to scope':

![[Pasted image 20211002101846.png]]

Click yes, and Burp should stop logging traffic from other sites.

Your history tab will now be much cleaner:

![[Pasted image 20211002101931.png]]

## Repeater

The Repeater tab can be used to Edit & Resend requests, similar to with the [[Web 1 - Inspecting a Webpage#Network|Developer Tools]].

Here we see the request for the shefesh page that was sent to the Repeater by pressing `Ctrl + R`:

![[Pasted image 20211001224546.png]]

We can edit any values in the request, such as headers and data, then press 'Send' and view the results of the response:

![[Pasted image 20211001224717.png]]

The size of the response is shown in the bottom-right corner - checking whether this changes is a good indicator of whether the changes you made had any impact.

We can also change the HTTP request method (right-click + 'Change request method') from the repeater tab, to conveniently turn a GET request for a specific resource into a POST request:

![[Pasted image 20211001225120.png]]

You can use this tab to experiment with different values in parameters, try to send requests that may trigger errors in the application, try to exploit [IDOR](https://portswigger.net/web-security/access-control/idor)-style vulnerabilities, and resend payloads to web shells - the possibilities are endless.

## Intruder

The Intruder tab is similar to the Repeater tab, in that it can resend HTTP requests. The extra feature of the Intruder tab is its ability to automate the sending of HTTP requests within a certain scheme, automatically replacing parts of the request with a sequence of predefined values.

What does this mean? It means you can take a captured Burp request - for example, a GET request to `http://example.com/profile?id=1` - and select a portion of the URL to modify according to a ruleset.

You may, for example, want to perform a brute force attack against the website and see if you can discover any other user profiles by their ID. You could do this by hand, visiting `http://example.com/profile?id=2` and `http://example.com/profile?id=3` until you find a match - or you could tell the Intruder to do it for you.

> Important note: the following example is purely illustrative. We are not performing a vulnerability assessment on example.com, and there is no intention of causing undesired effects. The URL /profile does not exist on example.com, so the requests will have no impact besides a small amount of traffic - they merely illustrate what the Intruder could be used for. Remember you can only perfom a vulnerability assessment on a site that you have explicit permission to do so on, such as juice-shop.herokuapp.com.

Press `Ctrl + I` to send a request to the Intruder:

![[Pasted image 20211001233115.png]]

The default attack type is 'Sniper', which will insert values into the desired 'positions'.

Now we need to define these positions - we can do so by highlighting the text we want to be replaced, and clicking the 'Add' button on the side:

![[Pasted image 20211001233148.png]]

This adds two characters round the text, indicating it will be replaced: `§1§`

We then go to the Payloads tab and tell Burp which values should replace the text in each position. The most simple payload type is 'Numbers', for which we can fill out the numbers 1-10 sequentially, with a step of 1 (meaning the numbers will increase by one with each request):

![[Pasted image 20211001233249.png]]

Then click 'Start Attack' - this will re-make these requests, replacing the `id` parameter with a new number each time. Clicking a request shows it in more detail:

![[Pasted image 20211001233543.png]]

Using this method we could look for a page that returns a non-404 status code, indicating we had found a profile.

This method is often called fuzzing. It can be done much better by other tools, but the concept is important and sometimes the intruder can be helpful for small-scale fuzzing. We will teach you about more efficient fuzzing tools as the semester goes on.

> If you wanted to reuse a request captured by Burp, perhaps in a `curl` command or to pass to a specialised fuzzing program, then you can right-click the request and click 'Copy as curl command'

## Cheatsheet

`Ctrl + R` to send a request to the Repeater.

`Ctrl + I` to send a request to the Intruder.

## Worksheet

1) Visit [https://juice-shop.herokuapp.com/](https://juice-shop.herokuapp.com/) and proxy the traffic to Burp Suite. Add an item to your basket, and see if you can identify which request adds the item (hint: it's a POST request)
2) After identifying this request, send it to the Repeater tab. Can you modify it so the item is added to another user's basket? (hint: it might not be as simple as changing one value - an error message in the response might tell you more)
3) View your basket, and identify the request that returns the basket data. Can you modify this request to *view* another user's basket? Use this to verify the previous exercise