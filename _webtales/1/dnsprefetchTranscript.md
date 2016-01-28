# S1E1 - DNS Prefetching & Preconnect - DONE

Lets begin by learning a web performance trick that can remove about 80 to 200milliseconds on the page load time; which according to well-established research is NOT negligible. Every microsecond after the first 100 adds to a lag perceivable to the end user, making your page load slower than it needs to be.
	SHOW THE USER TIMELINE NOTICE CHART REASEARCH


## dns-prefetch

### DNS Fundamentals
Lets brush up on some DNS fundamentals first. When the browser uses a web address to look for a resource, it converts that address into a set of numbers called an IP address. Every single address beginning with a domain or subdomain has it’s own corresponding IP address. 


It’s the IP address tells the browser where to fetch a resource from. For example google.com by itself means nothing to the computer. Google’s IP address 24.116.134.86 on the other hand, is the real location of Google’s homepage. DNS is the system that finds that corresponding IP address, usually by asking a DNS server over the internet. This entire process of finding the corresponding IP address is called DNS resolution. 

### Core concept
DNS prefetching is based on the idea that the browser should use it’s idle time to perform DNS resolutions ahead of time and store the results in a cache for later.

When the browser eventually needs to perform a DNS resolution that could’ve taken a long time; it doesn’t have to; because it can just grab it from the cache in about 2 milliseconds or lesser. These savings are much more pronounced in modern day web applications that have a lot of external resources that each need DNS resolution. 

### Browsers automatic prefetch
Browsers are very conscious about DNS resolution lags and take active steps to minimize it by using your behaviour making intelligent guesses on what websites you might visit next and dns-prefetching behind the scenes. 

Obviously since they are just guesses, they are going to miss out on a lot of good opportunities to prefetch dns for. No one knows how our website is structured better than us, the web developer. Have an external resource in a deeper page? The browser wouldn’t know that. Only we do. We could then dns-prefetch from the currently loaded page and save time later when the user eventually ends up loading that resource. Inside head, just plop this link tag pointing to that  resource. 
``

It’s hard to measure how long DNS resolutions take because it depends on unpredictable factors such as the end user’s network conditions, browsing history, etc. But to get a rough idea on your local machine, check the inspector waterfall timeline to measure how long DNS resolutions take. We can use tools like Chrome inspector 
	Inspect with chrome and check DNS lags
or `webpagetest`.

Chrome also provides a histogram of the average duration of DNS prefetches on your local machine. 
	SHOW chrome dns-prefetch histogram
![](Screen%20Shot%202016-01-20%20at%206.07.29%20AM.png)


### Same Page DNS Prefetching
Let me give you atleast two examples of where one could use DNS prefetch —
1. Embedded widgets such as a externally hosted video player.
		Youtube video player 
2. Javascript libraries included to the bottom of body. Yeah although they are on the same document and will eventually be looked up anyway; using dns-prefetch gives the browser a little headsup by completing DNS resolution much earlier so the browser will have the IP addresses ready by the time it works it’s way to the bottom.
		Show external source at bottom and dns-prefetch them at head

Pretty much any situation where you have an external domain is a good place to use dns-prefetch at.

### Cons?
1. dns-prefetch is a browser hint i.e. it is merely a suggestion and isn’t  necessarily a command to the browser and in situations it might be deemed detrimental to the device; like in a phone where battery life and bandwidth usage are precious commodities and not worth the multiple extra network calls. 
2. HTML-wise it’s completely backward compatible as well.


## Turning off DNS prefetching on your personal computer
Remember how I said your browser already does a lot of automatic dns-prefetching for you? If you’re not a big of fan of that, you can manually turn  off that behavior on your local machine.

For firefox, open `about:config` and toggle this entry
	network.dns.disablePrefetch

Safari’s can be turned off with this Webkit preference 
	defaults write com.apple.safari WebKitDNSPrefetchingEnabled -boolean false

Loading Chrome with this flag should do it
	dns-prefetch-disable 
Unchecking this works too
	Chrome > Settings > Show Advanced Settings > Privacy  and disable the option "Predict network actions to improve page load performance."



It is also possible to have your webpage turn off dns-prefetching for your end user’s, by including this tag in `head`
	<meta http-equiv="x-dns-prefetch-control" content="off">



### Chrome about:DNS
Something cool about Google Chrome - It let’s you take a peek into its DNS prefetch cache with  `about:dns`  
	about: DNS

Have I told you yet that you can add prefetch hints in real time with Javascript. Let me create a simple webpage
	MAKE basicpage.html and show DNS side by side
Now I ask the browser to prefetch dns for a random website and we’l wait for it to show up in Chrome’s DNS prefetch cache.
	Add via Javscript the dns-prefetch for randomwebsite.com
	Point to relevant entry in the DNS cache
See? DNS queries for this website are now fetched from here instead of the network.


## Preconnect
Preconnect is a slightly more ambitious browser hint. It takes advantage of the TCP protocol that powers the internet.

So … right after DNS get’s us the web server’s IP address, our computer begins communicating with the web server using a simple greeting called a TCP handshake. All this handshake does is ask the web server to prepare for a request soon. This is a required step in all TCP communications. Preconnect asks the browser to use it’s idle time to not only do the DNS resolution but also the TCP handshake so when it comes time to request the resource, tada it can skip those two steps and get right to fetching the resource. All you have to do is to set it up with this
	Add a Preconnect tag.

When every milliseconds counts, this is a great way to save a few. It’s a pity, support for this browser hint is pretty dismal at this point. We’ll have to wait and see if it improves, but since this is backwards compatible, there is no harm and only benefit to gain by using preconnect right away.


## Conclusion
dns-prefetch and preconnect are one of the cheapest and easiest ways to improve page load time!

Alright then. See you next time!
