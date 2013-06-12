---
layout: default
title: Home
---

        <!-- content start -->
        <div class="span8">
		
		  {% for post in paginator.posts %}
		  <div class="row-fluid">
            <div class="span12">
              <h2>{{ post.title }}</h2>
              <p>{{ post.content | truncatewords }}</p>
              <p><a href="{{ post.url }}" class="btn">更多 »</a></p>
            </div>
	      </div>
		  {% endfor %}
		  
		  <!-- paginate start -->
	      <div class="pagination pagination-centered">
            <ul>
              <li><a href="#">上一页</a></li>
              <li><a href="#">1</a></li>
              <li><a href="#">2</a></li>
              <li><a href="#">3</a></li>
              <li><a href="#">4</a></li>
              <li><a href="#">5</a></li>
              <li><a href="#">下一页</a></li>
            </ul>
          </div>
	      <!-- paginate end -->
		  
        </div>
		<!-- content end -->
		
		<!-- sidebar start -->
        <div class="span2">
          <div class="well sidebar-nav">
            <ul class="nav nav-list">
              <li class="nav-header">Sidebar</li>
              <li class="active"><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
              <li><a href="#">Link</a></li>
            </ul>
          </div>
        </div>
		<!-- sidebar end -->