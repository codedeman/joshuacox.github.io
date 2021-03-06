---
title: Two Column Content Rasmus Style
published: true
layout: post
disqus: yes
fbcomments: no
category: jekyll
photo_url: /img/rasmus.jpg
description: Rasmus Andersson has a great two column stack of his posts let’s duplicate it
tags: 
  - jekyll
  - javascript
---

![](/img/rasmus.jpg)

[Rasmus Andersson](http://rsms.me/) has a great two column stack of his posts let’s duplicate it.  

## _includes/head.html

First you’ll need to add this little guy to your `_includes/head.html`

       <script>window.initFuncs = [];</script>
       </script>


## index.html

Now let’s create our index.html.  First I use the default layout with nothing else in the YAML front matter

{% raw  %}
      ---
      layout: default
      ---
{% endraw  %}


Then there is the posts section that Rasmus made

{% raw  %}
      <!-- <div class="content full-bleed intro">
        Welcome to my homepage!!1
      </div> -->
      <section id="posts" class="posts section">
      </section>
      <div class="posts">
        <div class="content full-bleed" id="recent-posts">
        {% for post in site.posts limit:50 %}<a
          href="{{ post.url }}" class="post-excerpt{% if post.photo_url %} photo{% endif %}">
            <div class="padded-content">
              <div class="title"><h5>{{ post.title }}</h5></div>
            {% if post.photo_url %}
              <div class="image" style="background-image:url('{{ post.photo_url }}')"></div>
            {% else %}
              <!-- <div class="title">{{ post.title }}</div> -->
              <info datetime="{{ page.date | date: "%Y-%m-%d" }}">
                {{ post.date | date: "%b %Y" }} &nbsp - &nbsp
              </info>
              {% if post.description %}
              <span class="body">{{ post.description | strip_html }}</span>
              {% else %}
              <span class="body">{{ post.excerpt | strip_html }}</span>
              {% endif %}
            {% endif %}<!-- post.photo_url -->
            </div>
          </a>{% endfor %}
          <hr>
          <div class="breaker"></div>
          <div class="end">
            <a href="/archive/">More...</a>
          </div>
        </div>
      </div>
{% endraw  %}

Then the java script that does the magic

{% highlight javascript %}
      <script type="text/javascript">
      (function(){

      var columns = null;

      function f(){
        var cols = window.innerWidth >= 890 ? 2 : 1;
        if (columns === cols) { return; }
        columns = cols;
        // if (cols === 2) console.log('switch to multi-column');
        // else            console.log('switch to single-column');

        var posts = document.getElementById('recent-posts');
        var childNodes = posts.childNodes, i, L = childNodes.length, node, h,
            col_width, col0_y, col1_y, col1_x,
            origin = {x:0, y:0}, node_count = 0, is_col0, row0_max_y = 0;

        posts.style.position = (columns === 1) ? null : 'relative';

        for (i = 0; i !== L; ++i) {
          node = childNodes[i];
          if (node.nodeType === Node.ELEMENT_NODE) {
            if (node.className === 'breaker') {
              if (columns === 1) {
                node.style.height = null;
              } else {
                node.style.height = Math.max(col0_y, col1_y) + 'px';
              }
              break;
            }
            if (columns === 1) {
              node.style.position = null;
              node.style.top = null;
              node.style.left = null;
              node.style.width = null;
              continue;
            }
            node.style.position = 'absolute';
            if (col0_y === undefined) {
              origin.x = node.offsetLeft;
              origin.y = node.offsetTop;
              col_width = node.clientWidth;
              col0_y = origin.y + node.clientHeight;
              row0_max_y = col0_y;
              node.style.top = '0px';
              node.style.left = '0px';
              node.style.width = col_width + 'px';
              //node.style.border = '1px solid red'
            } else {
              if (col1_y === undefined) {
                col1_y = origin.y + node.clientHeight;
                col1_x = origin.x + node.clientWidth;
                if (col1_y > row0_max_y) {
                  row0_max_y = col1_y;
                }
                node.style.top = '0px';
                node.style.left = col1_x + 'px';
                node.style.width = col_width + 'px';
              } else {
                // node.style.position = 'absolute';
                is_col0 = (node_count % 2) === 0;
                node.style.width = col_width + 'px';
                if (is_col0) {
                  node.style.left = origin.x + 'px';
                  node.style.top = col0_y + 'px';
                  node.setAttribute('data-height', node.clientHeight);
                  col0_y += node.clientHeight;
                  node.classList.add('col0');
                } else {
                  node.style.left = col1_x + 'px';
                  node.style.top = col1_y + 'px';
                  node.setAttribute('data-height', node.clientHeight);
                  col1_y += node.clientHeight;
                  node.classList.add('col1');
                }
              }
            }
            ++node_count;
          }
        }
      }

      function mysetup(){
        f();
        var onresize = f;
        // var timer = null;
        // function onresize() {
        //   if (timer !== null) { return; }
        //   timer = setTimeout(function(){ timer = null; f(); }, 100);
        // }
        if (window.addEventListener) {
          window.addEventListener('resize', onresize);
        } else {
          window.attachEvent('resize', onresize);
        }
      }

      // if (window.addEventListener) {
      //   window.addEventListener('DOMContentLoaded', setup);
      // } else {
      //   window.attachEvent('onload', setup);
      // }

      window.initFuncs.push(mysetup);

      })();
      </script>
{% endhighlight %}

Phew that was a humdinger.  Nice Job [Rasmus](http://rsms.me/)

This is now the default index for this [blog](http://joshuacox.github.io/)
