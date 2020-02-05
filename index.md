# What happens when you type www.google.com.au in the browser and press enter?

Have you ever thought about what happens when you type www.google.com.au in your browser and hit enter before you actually see the google logo and that familiar search bar? It normally happens very quick, but there is actually a lot of things happening behind the scene.

Essentially, the behaviour of hitting www.google.com.au means you are asking the web page from another computer(google's server in this case) through the internet. 
You could imagine google's server locates in somewhere and it has an unique IP address works just like your home's address. Each URL you type in will have a corresponding IP address, for example, www.google.com.au has an IP address of 172.217.203.94. So you can actually get same google page by typing http://172.217.203.94. However these IP addresses are not easy to be remembered by human, so that's why we are having those **domain names**(such as google.com, youtube.com)

So the first thing when you type the domain name, someone needs to find the correct IP address for it and let your computer find the google server by using that IP address. 

For doing that, there is a system that maintains the name of the URL and the corresponding IP address, we call the system **DNS(Domain Name System)**. There are multiple DNS servers in the internet, but before your request goes to those DNS servers in the internet. Your browser will check caches in the following order

**1. Browser cache**
**2. OS DNS cache**
**3. Router's cache**
**4. DNS Server's cache**

**DNS servers**, also known as **recursive resolvers**, is where the last stop that could return cache and they are normally owned by your **ISP(Internet service provider)**. If there is no cache data related to www.google.com.au 's IP address, DNS server will initiate a DNS search query through internet. This search is being called **recursive search**.


## Recursive search
For explainning the recursive search, there are three types of other DNS servers need to be understood first. 
They are:
- Root Server
- TLD(Top Level DNS ) Server
- Authoritative server

### Root Server
Root Server is the first stop for the resolver to query for the domain name's IP address. There are 13 Root servers scattering across the world and they are known by all resolvers, so that the resolver knows where to find the root server at first stage. So at this stage, the resolver takes the domain name www.google.com.au and ask root server what is the IP address of this URL. Root server won't know the answer, cause it doesn't store the IP address itself, but it knows where to find by looking at .com and .au and tell the resolver where to find the .com and .au relevant info from another kind of DNS server - TLD.

### TLD
TLD maintains information for the domain names that share 
1. Same country code such as .au, .jp
2. Internationalized country code (countrycode written in native language)
3. Generic domain extenstion (.com,  .net)
4. Infrastructure TLDs(.ARPA)
 
So If a user is searching for google.com.au, after receiving a response from a root server, the recursive resolver would then send a query to a .com and .au TLD nameserver, which would respond by pointing to the authoritative server for the domain.

### Authoritative server

The authoritative server where holds the actual records of IP address and gives the resolver the final answer of what is the IP address of www.google.com.au.

If the resolver gets the IP address, it will cached the data and return it back to your browser, the browser will then send a HTTP request from the google's IP address to get the web page and after google respond the resources, it will be showing on your browser.

One thing need to be noted that, everytime the recursive resolver gets responses from one kind of DNS servers, it will save the address of the next server so they could be cached later.

**Reference:** 
1.https://howdns.works/
2.https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/
