---
title: Blog
permalink: /blog/
---

<!-- Load an icon library -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">

<div class="topnav">
  <a href="https://github.com/milesdwilliams15/job-market-materials/raw/main/cv.pdf"><i class="fa fa-fw fa-file"></i> CV</a>
  <a href="https://milesdwilliams15.github.io/research/"><i class="fa fa-fw fa-area-chart"></i> Research</a>
  <a href="https://milesdwilliams15.github.io/teaching/"><i class="fa fa-fw fa-mortar-board"></i> Teaching</a>
  <div class="dropdown">
        <button class="dropbtn">
        <i class="fa fa-angle-double-down"></i> More</button>
        <div class="dropdown-content">
            <a href="{{ site.data.social-media.email.href }}{{ site.data.social-media.email.id }}"><i class="fa fa-fw fa-envelope"></i> Email</a>
            <a href="{{ site.github.owner_url }}"><i class="fa fa-fw fa-code-fork"></i> My GitHub</a>
            <a href = "https://milesdwilliams15.github.io/software/"><i class="fa fa-fw fa-gears"></i>My Software</a>
            <a href="https://milesdwilliams15.github.io/blog/"><i class="fa fa-fw fa-pencil"></i> My Blog</a>
        </div>
    </div>
</div>

<p> </p>

## My Blog

This blog has two purposes. First, it's a convenient place for me to record my latest thoughts, research, and teaching materials (as they accumulate over time). Second, I generally avoid being active on standard forms of social media (and I think I'm happier and healthier because of that choice), but I still think it's good to have an online presence. A blog is a nice way to do that---or, at least it's a nice way for *me* to do that.

You can see my posts ordered from most to least recent below. 

<ul id="archive" style="list-style-type:none">
{% for post in site.posts %}
  {% capture y %}{{post.date | date:"%Y"}}{% endcapture %}
  {% if year != y %}
    {% assign year = y %}
    <h2 class="blogyear">{{ y}}</h2>
  {% endif %}
<li class="archiveposturl"><span><a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></span><br/>
<span class = "postlower">

<!--<strong>Author:</strong> {{post.author}} -->
<strong>Category:</strong>  {% if post.categories %}
 
  {% for cat in post.categories %}
  <a href="/categories/#{{ cat }}" title="{{ cat }}">{{ cat }}</a>&nbsp;
  {% endfor %}

{% endif %} <!-- {{ post.categories | first }} -->
<strong style="font-size:100%; font-family: 'Titillium Web', sans-serif; float:right; padding-right: .5em">{{ post.date | date: '%d %b %Y' }}</strong> 
</span> 

</li>
{% endfor %}
</ul>