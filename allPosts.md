---
layout: default
title: All posts on ifrb
---

## All posts

<table id="allPosts">
  {% for post in site.posts do %}
  <tr>
  	<td>
    	{{ post.date | date_to_long_string }}
    </td>
    <td class="postLink">
    	<a href="{{ post.url }}">{{ post.title }}</a>
	</td>
  </tr>
  {% endfor %}
</table>
