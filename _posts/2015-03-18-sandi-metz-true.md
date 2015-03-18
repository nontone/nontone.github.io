---
layout: post
title:  "Sandi Metz TRUE"
date:   2015-03-18 15:03:00
categories: ruby
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
<h5>อ่านง่ายและมีผลต่อการเปลี่ยน</h5>

<p>
  สอง method ข้างล่างทำงานเหมือนกัน
</p>

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

<p>
  โค้ด opaque ข้างบนยากต่อการเข้าใจ จู่ๆ ก็มีเลขมหัศจรรย์มาให้เฉย ชื่อ method ก็ไม่รู้ว่าหมายถึงอะไร แถมยังมีตัวแปร constant ที่ชื่อไม่มีความหมายเลยอีก
</p>

<p>
  โค้ด transparent ที่เห็นอาจยังไม่ดีสุด ถึงงั้น่อย่างน้อยเข้าใจได้ดีกว่า เราจะเห็นผลของการเปลี่ยนแปลงจาก loyalty_bonus ได้ในตัวอย่าง โค้ดกำลังบอกเราว่า นี่เกี่ยวกับการเปลี่ยน accounting นะ แทนที่จะแปลกใจกับ money kick คืออะไรว้า?
</p>

<h4>Reasonable</h4>
<h5>ทุกการเปลี่ยนแปลงมีเหตุผล เมื่อโค้ดมันซับซ้อน</h5>

<p>
  เหมือนเคย โค้ดข้างล่างนี่ทำงานเหมือนกัน
</p>

Unreasonable
{% highlight ruby %}
def full_name
  salutation = gender == "M" ? "Mr" : "Ms"
  "#{salutation} #{first_name} #{last_name}"
end
{% endhighlight %}

Reasonable
{% highlight ruby %}
def full_name(salutation: basic_salutation)
  "#{salutation} #{first_name} #{last_name}"
end

def basic_salutation
  salutation = gender == "M" ? "Mr" : "Ms"
end
{% endhighlight %}
<p>
  เปลี่ยน salutation ควรเป็นเรื่องง่าย แต่โค้ด unreasonable กลับออกมาไม่ดีเท่าที่ควร กลับกันโค้ด reasonable ให้เราส่ง salutation อะไรก็ตามที่เราต้องการ หรืออย่างน้อยใช้ basic salutation แทน
</p>

<h4>Usable</h4>
<h5>เอาไปใช้ต่อที่อื่นได้</h5>

Unusuable
{% highlight ruby %}
def square_number(number)
  number ** 2
end
{% endhighlight %}

Usable
{% highlight ruby %}
class Numeric
  def power(x)
    self ** x
  end
end
{% endhighlight %}
<p>
  square_number ในที่นี้กลับใช้ครั้งเดียว มันจะดีมากถ้าเราทำให้มันใช้กับตัวเลขทั้งหมดได้ ทีนี้เราเรียกมันใหม่ว่า power แล้วอยู่ภายใต้ Numberic แทน ซึ่งใช้ได้ดีแล้วล่ะ
</p>

<h4>Exemplary</h4>
<h5>โค้ดที่เขียนไปเป็นตัวอย่างของแนวโค้ดที่เราอยากให้เป็น</h5>

Unworthy
{% highlight ruby %}
class Array
  def first
    self.reverse[1]
  end
end
{% endhighlight %}

Exemplary
{% highlight ruby %}
class MyWeirdList
  include Enumerable

  def second_from_last
    collection[-2]
  end
  alias :first :second_from_last

  def collection
    @collection ||= []
  end
end
{% endhighlight %}
<p>
  ตามแผนเลย? ใช่เลย แต่ว่าพอ google "monkey patch Array" และสิ่งที่แปลกใจ คือ มีอะไรแปลกๆ ที่คนต้องการยัดใส่ Array แล้วมันไม่มีประโยชน์โดยรวมซักเท่าไหร่ ถ้าเราเรียกมันว่า first method เราไม่ต้องการให้คนอื่นมาใช้โค้ดนี้ต่อแน่ๆ
</p>

<p>
  อย่างน้อยที่สุดตัวอย่าง exemplary ก็จำกัดอยู่แค่ class เดียวที่เราตั้งใจต่างออกมา ดีกว่าที่จะเห็นในแบบแรกแน่ๆ
</p>

อ่านมาจาก 
<a href="http://designisrefactoring.com/2015/02/08/introducing-sandi-metz-true/">Introducing Sandi Metz's TRUE</a>