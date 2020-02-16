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






# K8s basic architecture illustration

**Prerequisites**
- Assume we have a web app and a db app have been containerized. These two apps are supposed to talk to each other.

## Pods
Pods are the smallest unit in K8s, each pod encapsulates a container or multiple containers, however pods won't have containers for one same app if you want to scale up the instances, the proper way is to create more pods. Why do we need pods is because this is the object that can be managed by other K8s objects. For the diagram that we can see, we have 6 pods in total in the cluster. 
The way to create Pods is by create a manifest(All of the K8s objects could be created by the manifest file. 

_A typical pod manifest:_
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: demoapp
spec:
  containers:
     - name: demoapp
     - image: nginx
```


## ReplicaSets
ReplicaseSet is another K8s object, it has it's only responsibility, which is creating replicas for an app instance, and it will also ensure the instances will be maintained in the number of specficiation, for example, if you specify 3 replicas for one instance, if one pod fails, it will auto create another pods. 

_A replicaSet manifest_
```yaml
apiVersion: v1
kind: ReplicaSet
metadata:
  - name: demorc
spec:
  template:
    metadata:
      name: demoapp
      type: front-end
    spec:
      containers:
      - name: demoapp
      - image: nginx
    replicas: 3
 selector:
   matchLabels:
     type: front-end
```


## Deployments
Deployment in a K8s object, it is in a higher hierachy than ReplicaSets. Many times, when we create a deployments manifest, we could specify replicas, it will help us to create a replicaset without creating a replicaset itself manually. So we need to know the deployments object itself is not responsible for managing the replicas, it is the replicaset it created managing replicas. So what deployments do?
- Rolling out apps (update apps)
- Rolling back apps (undo updates)
- Scale up 
- Pause and resume deployments.
When the pod spec changeds, the deployments will be notify there is an update, it will do the rolling updates for us by update pod one by one.
_A deployments manifest_
```yaml
apiVersion: v1
kind: ReplicaSet
metadata:
  - name: demorc
spec:
  template:
    metadata:
      name: demoapp
      type: front-end
    spec:
      containers:
      - name: demoapp
      - image: nginx
    replicas: 3
 selector:
   matchLabels:
     type: front-end
```


## Services
Service is used to enable communication between different network namespaces. By default, each pod has it's own IP since it is an isolated network namespace. Service is helping to mapping out the request from outside of the network to the pod.
For example, if a user from external network want to access your web application. What service does is it listens on the port(NodePort) and forward the requests to the app.
_A service manifest_
```yaml
apiVersion: v1
kind: Service
metadata: 
  name: demo-service
spec:
  type: NodePort
  ports:
    - targetPort: 80
    - port: 80
    - NodePort: 30008
  selector:
    app: demoapp
    type: front-end
```
