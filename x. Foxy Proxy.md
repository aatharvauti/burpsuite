# 0.1 Foxy Proxy Setup

**Note that this is optional, you can directly use Burp Suite's browser from the proxy tab.**

### Why??

The Burp Proxy works by opening a web interface on `127.0.0.1:8080` (by default). As implied by the fact that this is a "proxy", we need to redirect all of our browser traffic through this port before we can start intercepting it with Burp. We can do this by altering our browser settings or, more commonly, by using a Firefox browser extension called [FoxyProxy](https://getfoxyproxy.org/).

Note: Install the **BASIC** version

Once installed, a button should appear at the top right of the screen which allows you to access your proxy configurations:  
![FoxyProxy button at the top right of the screen highlighted](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/fee3f150ebb4d9301023188fddc0458a.png)  

There are no default configurations, so let's click on the "Options" button to create our Burp Proxy config.

This will open a new browser tab with the FoxyProxy options page: 

![Screenshot showing the FoxyProxy options page with the Add button circled. There are currently no configs](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/5a73425b5de3395c5db2962b9d613506.png)

Click on the "Add" button and fill in the following values:

-   Title: `Burp` (or anything else you prefer)
-   Proxy IP: `127.0.0.1`
-   Port: `8080`  

![Screenshot showing the three fields of the Add Proxy screen filled in as per the list above](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/b2d6f2b724f123070ca434bf2759df91.png)

Now click "Save".

---

When you click on the FoxyProxy icon at the top of the screen, you will see that that there is a configuration available for Burp:  
![Screenshot of the Burp Config in FoxyProxy](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/20f5e9db304d164b57c7f7d89fabc63a.png)

If we click on the "Burp" config, our browser will start directing all of our traffic through `127.0.0.1:8080`. **Be warned**: if Burp Suite is not running, your browser will not be able to make any requests when this config is activated!  

Activate this config now -- the icon in the menu should change to indicate that we have a proxy running:  
![The FoxyProxy icon when we have a proxy selected](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/6a468cdba89180fad2cfb429db0ddf10.png)  

Next, switch over to Burp Suite and make sure the Intercept is On:  
![Screenshot showing the intercept button in Burp Proxy](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/32b3bdd3afad87b916212f078e0f1795.png)  

Now, try accessing the homepage for `http://10.10.244.65/` in Firefox. Your browser should hang, and your proxy will populate with the request headers.

Congratulations, you just intercepted your first request!

Unfortunately, there's a problem. What happens if we navigate to a site with TLS enabled? For example, `https://google.com/`:  
![Screenshot showing the FireFox HSTS warning message about the Portswigger CA cert not being trusted](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/8b4b43cac91cd9a80622b953598d05eb.png)  

We get an error.

Specifically, Firefox is telling us that the Portswigger **C**ertificate **A**uthority (CA) isn't authorised to secure the connection.

Fortunately, Burp offers us an easy way around this. We need to get Firefox to trust connections secured by Portswigger certs, so we will manually add the CA certificate to our list of trusted certificate authorities.

First, with the proxy activated head to [http://burp/cert](http://burp/cert); this will download a file called `cacert.der` -- save it somewhere on your machine.

Next, type `about:preferences` into your Firefox search bar and press enter; this takes us to the FireFox settings page. Search the page for "certificates" and we find the option to "View Certificates":  
![FireFox certificate settings](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/a9de0495b2ac6738520c8f9946afdecb.png)  

Clicking the "View Certificates" button allows us to see all of our trusted CA certificates. We can register a new certificate for Portswigger by pressing "Import" and selecting the file that we just downloaded.

In the menu that pops up, select "Trust this CA to identify websites", then click Ok:  
![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/23e5cb317d00c1a5e64def1d46fa9301.png)  

We should now be free to visit any TLS enabled sites that we wish!

The following video shows the full import process:  

![GIF demonstrating the full cert import process](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/fb2a8717ae887eda024a7791d83cefaf.gif)

