---
layout: default
---
<section>
  <h2>Latest Posts</h2>
  <ul>
    {% for post in site.posts limit:3 %}
      <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>
</section>




## Knowledge Base

<ul>
  <li>
    <a href="/kernel/">Kernel Articles</a>
  </li>
</ul>


<ul>
  <li>
    <a href="/dt/">Device Trees</a>
  </li>
</ul>
<ul>

  <li>
    <a href="/linux-drivers/">Device  drivers</a>
  </li>
</ul>

<ul>
  <li>
    <a href="/misc/">Miscellaneous Articles </a>
  </li>
</ul>


