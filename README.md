mod_flyover
===========

mod_flyover is aim to be use as an apache 2.4 module.

A simple example of the use of mod_flyover is as follow: 

html templates -> become function in mod_flyover
html_templates -> include -> become a function call in mod_flyover

All function generate from html_templates are outputing directly with 
ap_rprintf, or in other words directly output from apache.

This is working in two steps:  
- a html template parser that generate apr C code.
- the generate C code can then be compile into apache as a module.

Q: Why parsing in C? 
A: Freakin fast. When I want to test code, I do not want to wait for compiling.

Q: What is the objective of this?
A: Well simply put, it takes your html garbage, convert it to C, then compile-it
   as a module for apache 2.4. It use APR which is an amazing framework that 
   support multi-threading like a breeze. Which mean, your entire HTML code 
   would be deliver directly from memory without requiring any disk IO 
   (excluding logs). 

Q: Isn't crazy to but all HTML into memory?
A: Memory is cheap and unless you have over 200mb of HTML with 2gb of RAM 
   you should probably think about upgrading your server first. The main thing 
   that will bite you without even noticing on a server is IO power, 
   escpecially on Virtual Machine (especially EC2) where the IO can be quit 
   bad. You can easilly get 2x more power of your current server by 
   simply running from flyover.

Q: But wait, isn't not totally crazy to put a whole website into memory?
A: Absolutely not! with apache worker it will be in memory only once per 
   process, each thread will deliver the data from the same memory pool.

Q: Can I get any faster?
A: If you have good CPU in hands, you can suck even more performance. 
   Just load mod_pagespeed after mod_flyover.

Q: Can I use PHP inside my HTML Templates?
A: It is possible, but using PHP after compilation would probably defeat 
   the purpose of this module. However, if you use proper MVC pattern you 
   may get a good performance boost.
