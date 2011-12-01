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
# Parent process
puts "Hello from parent: #$$"
foo = rand
puts "Parent #$$ foo (#{foo}) before forking & changing"

3.times do
  fork do
    # child process logic here
    trap('INT') do
      puts "Exiting from child: #$$"
      exit
    end
    puts "Child #$$ foo (#{foo})"
    loop do
      puts "Hello from child: #$$"
      sleep 3
    end
    # end child process logic
  end
end

foo = rand

# Parent process logic continues
trap('INT') do
  puts "Exiting from parent: #$$"
  puts "Parent #$$ foo (#{foo}) when exiting"
  exit
end

puts "Parent #$$ foo (#{foo}) after forking & changing"

Process.waitall
{% endhighlight %}

Run this and you'll see that the child pids differ of course from the parent's pid. You can also see that the exiting (via `Ctrl+c`) can be handled differently for child processes as it is from the parent.

{% highlight bash %}
∴ ruby fork.rb 
Hello from parent: 30609
Parent 30609 foo (0.6330290120289636) before forking & changing
Child 30611 foo (0.6330290120289636)
Hello from child: 30611
Parent 30609 foo (0.7532945400843011) after forking & changing
Child 30613 foo (0.6330290120289636)
Hello from child: 30613
Child 30616 foo (0.6330290120289636)
Hello from child: 30616
Hello from child: 30611
Hello from child: 30613
Hello from child: 30616
Hello from child: 30611
Hello from child: 30613
Hello from child: 30616
^CHello from child: 30611
Hello from child: 30613
Hello from child: 30616
^CExiting from parent: 30609
Parent 30609 foo (0.7532945400843011) when exiting
Exiting from child: 30611
Exiting from child: 30613
Exiting from child: 30616
{% endhighlight %}


The newly created child process will have a copy of the parent's memory. You can see that in the example above because `foo` in the children is set to the `foo` instantiated in the parent, even though later the parent's `foo` changes. Since CRuby is not [copy-on-write](http://en.wikipedia.org/wiki/Copy-on-write) friendly like REE, physical memory must be allocated to run the child process. This is an advantage [Ruby Enterprise Edition](http://www.rubyenterpriseedition.com) (which has a copy-on-write-friendly GC) has over CRuby.
