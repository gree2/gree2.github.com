---
layout: post
title: "tool automating remote desktop connection"
description: ""
category: [tool]
tags: [mstsc, auto, cmdkey]
---
{% include JB/setup %}


### batch

* [reference](http://stackoverflow.com/questions/16479652/automating-remote-desktop-connection)

* [reference](http://stackoverflow.com/questions/16479652/automating-remote-desktop-connection)

			cmdkey /generic:192.168.100.100 /user:Administrator /pass:password
			mstsc  /v:192.168.100.100
			cmdkey /delete:TERMSRV/192.168.100.100

### Shortcut keys that can be used when connected to a Remote Desktop

* [reference](http://ss64.com/nt/mstsc.html)

<table id="keys">
	<tr> <th scope="col">Windows</th> <th scope="col">Remote Desktop</th> <th scope="col">Purpose</th>
	</tr>
	<tr> <td>Ctrl + ALT + Del</td> <td>Ctrl + ALT + END</td> <td>Bring up the Windows Security dialog box</td>
	</tr>
	<tr> <td>ALT + TAB</td> <td>ALT + Page Up</td> <td>Switch between programs from left to right</td>
	</tr>
	<tr> <td>ALT + Shift + TAB</td> <td>ALT + Page Down</td> <td>Switch between programs from right to left</td>
	</tr>
	<tr> <td>ALT + Esc <br> ALT + Shift + Esc</td> <td>ALT + INSERT</td>
	<td>Switch between programs in the order they were started</td>
	</tr>
	<tr> <td>Ctrl + Esc</td> <td>ALT + HOME</td> <td>Display the Start menu</td>
	</tr>
	<tr> <td>n/a</td> <td>Ctrl + ALT + BREAK</td> <td>Switch the client between full-screen mode and window mode</td>
	</tr>
</table>

* `remote desktop Shortcut do not work`