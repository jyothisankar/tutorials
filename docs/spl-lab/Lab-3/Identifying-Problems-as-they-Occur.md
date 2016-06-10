---
layout: docs
title:  Identifying problems as they occur
description:
weight: 520
---

# Identifying problems as they occur
---

<div class="alert alert-info" role="alert">
<h4><b>This section is optional</b></h4>
It requires letting the job run for over a half hour and then observing the Console. Take a break, or come back to this section after completing the rest.
</div>

1. 	Let the job or jobs run for at least 40 minutes.
Because the file is read every 45 seconds and the throttled drawdown takes a little longer than that (47.55 s), the Throttle’s input buffer eventually fills up. If you let the job(s) run long enough, a red square or yellow triangle will show up in the **PE Connection Congestion** row of the **Summary** card. (The congestion metric for a stream tells you how full the destination buffer is, expressed as a percentage.) At the same time, the **Flow Rate Chart** shows more frequent, lower peaks: the bursts are now limited by the filling up of the Throttle’s input buffer instead of by the data available in the file.

1. 	Hover over the information tool in the **PE Connection Congestion** row in the **Summary** card to find out exactly which PEs are congested and how badly. Also notice how pattern in the **Flow Rate Chart** is now different compared to when the job was young.

    <img width="100%" src="/tutorials/images/Lab3/5.jpg"/>

    The information panel shows at the top of the list: this means that congestion is observed on a stream called **Observations** at the output port of an operator of the same name (you did that by removing the operator alias). Note, however, that while **Observations** is the one that suffers congestion, it is the next operator, called **Throttled**, that causes it.

1. 	What will happen eventually, as you let this run for a long time? Will the FileSource operator continue to read the entire file every 45 seconds? What happens to its input buffer, on the port receiving the file names? How will the DirectoryScan operator respond?

These questions are intended to get you thinking about a phenomenon called <b>back-pressure</b>. This is an important concept in stream processing. As long as buffers can even out the peaks and valleys in tuple flow rates, everything will continue to run smoothly. But if buffers fill up and are never fully drained, the congestion moves to the front of the graph and something has to give: unless you can control and slow down the source (as conveniently happens here), data will be lost. There is no getting around that.

 {% include nextPageFinder.html context=page.url %}
 