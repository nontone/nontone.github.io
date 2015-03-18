---
layout: post
title:  "Sandi Metz TRUE"
date:   2015-03-18 15:03:00
categories: ruby,refactoring
---

เขียนโค้ดมีคุณภาพตามแนวทาง Sandi Metz's TRUE

TRUE ย่อมาจาก
<ul>
  <li>Transparent</li>
  <li>Reasonable</li>
  <li>Usable</li>
  <li>Exemplary</li>
</ul>

เมื่อไหร่ที่โค้ดเขียนตามนี้ ถือว่า ดีงาม

<h4>Transparent</h4>
It is easy to see the code’s function and the effect of a change
อ่านง่ายและ

Opaque
{% highlight ruby %}
def ex(x)
  Account.transform(x * REQ_A).monkey_kick(:2, self)
end
{% endhighlight %}

Transparent
{% highlight ruby %}
def extend(number_of_months)
  modified_month_count = number_of_months * loyalty_bonus

  Account.extend_subscription(modified_month_count)

  notify_accounting({action: extension, user: self})
end
{% endhighlight %}

อ่านมาจาก 
<%= link_to "Introducing Sandi Metz's TRUE", 
  "http://designisrefactoring.com/2015/02/08/introducing-sandi-metz-true/" %>