## What happens when you type www.google.com.au in the browser and press enter?

Have you ever thought about what happens when you type www.google.com.au in your browser and hit enter before you actually see the google logo and that familiar search bar? It normally happens very quick but there is actually a lot of things happening.

### Simple version
In one simple sentence, the behaviour of hitting www.google.com.au essentially means you are requesting resources from another machine(google's server in this case) through the internet. You could imagine google's server locates in somewhere and it has an unique Ip address works just like your home's address. Each URL you type in will have a corresponding IP address, for example, www.google.com.au has an IP address of 172.217.203.94. So you can actually get same google page by typing http://172.217.203.94. However these IP addresses are not easy to be remembered by human, so that's why we are having those domain name. So the first thing when you type the domain name, someone need to find the correct IP address for it and let your machine find the google server by using that IP address. For doing that, there is a system that maintains the name of the URl and the corresponding IP address, we call the system DNS(Domain Name System). There are multiple DNS servers in the internet, but before your request goes to those DNS servers in the internet.Your browser will check caches FROM
1. Broswer DNS cache
2. OS DNS cache


You can use the [editor on GitHub](https://github.com/hsy3418/heavenBlog/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/hsy3418/heavenBlog/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
