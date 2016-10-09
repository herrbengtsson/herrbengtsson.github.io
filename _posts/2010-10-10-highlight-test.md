---
layout: post
title: "Highlight testing"
category: Markup
---
### Ruby
{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

```ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
```

### Python
{% highlight python %}
#!/usr/bin/env python2

'''
Description
'''

from os import isfile, isdir

def main():
    # Comment
    print("Hello world")

if __name__ == "__main__":
    main()
{% endhighlight %}

### HTML
{% highlight html %}
<html>
<head>
<title>jQuery Hello World</title>

<script type="text/javascript" src="jquery-1.2.6.min.js"></script>

</head>

<body>

<script type="text/javascript">

$(document).ready(function(){
 $("#msgid").html("This is Hello World by JQuery");
});

</script>

This is Hello World by HTML

<div id="msgid">
</div>

</body>
</html>
{% endhighlight %}


### Bash
{% highlight shell %}
axl@axl:~/$ date
Sun  2 Oct 21:39:03 BST 2016
axl@axl:~/$ pwd
/home/axl
axl@axl:~/$ 
{% endhighlight %}


