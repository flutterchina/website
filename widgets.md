---
layout: page
title: Widgets 目录
permalink: /widgets/
summary: Flutter Widget列表
keywords: Flutter控件列表
---


使用Flutter的一套的视觉、结构、平台、和交互式的widgets，快速创建漂亮的APP.

<p>
除了按类别浏览widget外，您还可以在<a href="/widgets/widgetindex/">Flutter widget 索引</a>浏览Flutter中的所有widgets。
</p>

<ul class="cards">
{% for section in site.data.catalog.index %}
	<li class="cards__item">
	    <div class="card">
		    <h3 class="catalog-category-title"><a class="action-link" href="/widgets/{{section.id}}">{{section.name}}</a></h3>
		    <p>{{section.description}}</p>
		    <div class="card-action">
		        <a class="action-link" href="/widgets/{{section.id}}">查看</a>
		    </div>
		</div>
		
	</li>
 {% endfor %}
</ul>
