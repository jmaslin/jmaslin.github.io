---
layout: page
title: Portfolio
permalink: /portfolio/
---

<p>The latest projects I have been working on can be found on this page. For a full demo or more information, please contact me.</p>
<!-- <hr /> <br /> -->

<ul class="portfolio-nav">
	<a id="coopswitch"><li>Coopswitch</li></a>
	<a id="eventme"><li>EventMe</li></a>
	<a id="merecraft"><li>Merecraft</li></a>
</ul>

<ul class="portfolio">
	<li id="coopswitchItem">
		<h3>Coopswitch</h3>
		<p class="post-meta">
			<strong>Established</strong> November 2013 | 
			<strong>First Git Commit</strong> May 2014 | 
			<strong>Currently</strong> In Progress
		</p>
		<p class="post-meta"><strong>Development</strong> - <a href="https://github.com/jmaslin/Coopswitch">Github Repo</a></p>

		<ul>
			<li><strong>PHP</strong> and <strong>MySQL</strong> were used as the backend for this project. Instead of using a framework, I decided start from scratch in order to develop a greater understanding of the language.</li>
			<li><strong>Twitter's Bootstrap</strong> framework, which consists of <strong>HTML, CSS, </strong>and <strong>Javascript</strong> classes was used for the majority of the frontend UI/UX. I also used <strong>JQuery</strong> for certain design elements and submitting <strong>AJAX</strong> requests.</li>
			<li>This project is hosted on a small Digital Ocean<strong> VPS</strong> and is protected with a <strong>SSL</strong> script and passwordless auth via <strong>SSH</strong>.</li>
		</ul>
		<br />

		<p class="post-meta"><strong>About</strong></p>
		<p>Coopswitch is my first major web project and was created to solve the issue of switching co-op cycles at Drexel University.</p>
	</li>

	<li id="eventmeItem">
		<h3>EventMe</h3>
		<p class="post-meta">
			<strong>Established</strong> October 2014 | 
			<strong>First Git Commit</strong> November 2014 | 
			<strong>Currently</strong> In Progress
		</p>
		<p class="post-meta"><strong>Development</strong> - <a href="https://github.com/jmaslin/EventMe">Github Repo</a></p>

		<p class="post-meta"><strong>About</strong></p>
		<p>EventMe was created to improve the admittance speed at venues for students.</p>
	</li>

	<li id="merecraftItem">
		<h3>Merecraft</h3>
		<p class="post-meta">
			<strong>Established</strong> April 2013 |
			<strong>First Opened</strong> May 2013 | 
			<strong>Currently</strong> Left in August 2014
		</p>

		<p class="post-meta"><strong>About</strong></p>
		<p>Merecraft is a small community based around the popular game Minecraft. Besides founding the server, I helped grow the community, code custom Java plugins, and perform server maintenance.
		</p>
	</li>

</ul>

<script>

	$( document ).ready(function() {

		$('.portfolio > li').hide();

		var url = window.location.href;

		if (url.indexOf("#") != -1) {
			project = url.substring(url.indexOf("#"), url.length);
			$('.portfolio-nav a'+project).addClass('selected');
			$('.portfolio > li'+project+"Item").fadeIn();
		}

	});

	$('.portfolio-nav a').click(function(e) {

		e.preventDefault();
		
		itemSelected = $(this).attr('id');

		$.each($('.portfolio > li'), function() {
			if (itemSelected != $(this).attr('id')) {
				$(this).hide();
			}
		});

		$('#'+itemSelected+"Item").fadeIn();

		$('.portfolio-nav').find('.selected').removeClass('selected');
		$(this).find('li').addClass('selected');
		// $(this + ' > li').addClass("selected");

	});

</script>