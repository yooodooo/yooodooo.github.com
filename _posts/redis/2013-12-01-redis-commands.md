---
layout: post
title: redis commands
tagline: [redis] 
group: redis
---
{% include JB/setup %}

<div class="well">主要是一些常用的redis命令讲解,来源于<a href="http://redis.io/commands">主页</a></div>


<div class="tabbable">

  <ul class="nav nav-tabs">
    <li class="active"><a href="#command_strings" data-toggle="tab">Strings</a></li>
    <li><a href="#command_hashes" data-toggle="tab">Hashes</a></li>
	<li><a href="#command_lists" data-toggle="tab">Lists</a></li>
	<li><a href="#command_sets" data-toggle="tab">Sets</a></li>
  </ul>
  
  <div class="tab-content">
  
	<div class="tab-pane active" id="command_strings">

	<li> <code>set key value</code> 如果key存在那么将会覆盖。如果成功返回OK。在2.6.12后新增了<code>SET key value [EX seconds] [PX milliseconds] [NX|XX]</code> </li>
	<li> <code>set key value</code> 如果key存在那么将会覆盖。如果成功返回OK。在2.6.12后新增了<code>SET key value [EX seconds] [PX milliseconds] [NX|XX]</code> </li>
	<li> <code>set key value</code> 如果key存在那么将会覆盖。如果成功返回OK。在2.6.12后新增了<code>SET key value [EX seconds] [PX milliseconds] [NX|XX]</code> </li>
	<li> <code>set key value</code> 如果key存在那么将会覆盖。如果成功返回OK。在2.6.12后新增了<code>SET key value [EX seconds] [PX milliseconds] [NX|XX]</code> </li>
	<li> <code>set key value</code> 如果key存在那么将会覆盖。如果成功返回OK。在2.6.12后新增了<code>SET key value [EX seconds] [PX milliseconds] [NX|XX]</code> </li>
	<li> <code>set key value</code> 如果key存在那么将会覆盖。如果成功返回OK。在2.6.12后新增了<code>SET key value [EX seconds] [PX milliseconds] [NX|XX]</code> </li>


	</div>
	
    <div class="tab-pane" id="command_hashes">
      <p>hashes commands</p>
    </div>
	
	<div class="tab-pane" id="command_lists">
      <p>lists</p>
    </div>
	
	<div class="tab-pane" id="command_sets">
      <p>sets</p>
    </div>
	
  </div>
</div>