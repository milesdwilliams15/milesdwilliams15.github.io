---
title: Blog
permalink: /blog/
---

<div class="topnav">
    <a class="active" href="https://milesdwilliams15.github.io/"><strong>Home</strong></a>
    <a href="https://github.com/milesdwilliams15/job-market-materials/raw/main/cv.pdf"><strong>CV</strong></a>
    <a href="https://milesdwilliams15.github.io/blog/"><strong>Blog</strong></a>
    <a href="{{ site.github.owner_url }}"><strong>GitHub</strong></a>
    <a href = "{{ site.data.social-media.email.href }}{{ site.data.social-media.email.id }}" title="Email me"><strong>Email</strong></a>
    <div class="dropdown">
        <button class="dropbtn"><strong>About</strong> <i class="fa fa-caret-down"></i></button>
        <div class="dropdown-content">
            <a href = "https://milesdwilliams15.github.io/research/"><strong>Research</strong></a>
            <a href = "https://milesdwilliams15.github.io/software/"><strong>Software</strong></a>
            <a href = "https://milesdwilliams15.github.io/teaching/"><strong>Teaching</strong></a>
        </div>
    </div>
</div>  
<br/>


This blog has two purposes. First, it's a convenient place for me to record my latest thoughts, research, and teaching materials (as they accumulate over time). Second, I generally avoid being active on standard forms of social media (and I think I'm happier and healthier because of that choice), but I still think it's good to have an online presence. A blog is a nice way to do that---or, at least it's a nice way for *me* to do that.

You can see my posts ordered from most to least recent below. 

<ul id="archive">
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