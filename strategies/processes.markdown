---
layout: site
header: Processes
strategies-active: active
---

## Definition

Each time your operating system starts up an instance of a program, it is creating a new running process. So when you run 
{% highlight bash %}
$> ruby -e 'puts 1'
1
$>
{% endhighlight %}
the operating system fires up your Ruby interpreter to run that command, and a new process is born. Since this is a shortlived "program" it also dies quickly. Where Ruby runs into trouble is with longer running programs, like web application servers.

When Ruby on Rails first became popular, the only decent option for serving multiple requests per second were to have multiple instances of Mongrel running. Each server was a separate process, so there was no sharing of memory, which meant a server had to have a good amount of memory or an application had to have a cluster of servers sitting behind a proxy like HAPrixy or nginx.

One way to create more available instances of a program is to `fork` a currently running program.
{% highlight ruby %}
# TODO Add example
{% endhighlight %}
The newly created child process will have a copy of the parent's memory. Since CRuby is not [copy-on-write](http://en.wikipedia.org/wiki/Copy-on-write) friendly like REE, physical memory must be allocated to run the child process.
