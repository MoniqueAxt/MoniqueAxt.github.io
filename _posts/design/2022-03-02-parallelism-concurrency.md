---
layout: page
subheadline: Uni courses
title:  "Parallelism & Concurrency"
teaser: "I have a deadlock joke..."
#meta_teaser:
breadcrumb: false
categories:
    - courses
header:
    image_fullwidth: "parallel.jpg"
    title: "<br> monique axt <br> software engineer"
---
<b>... but someone else has the punchline.</b>

Using both Java and C++, these university labs covered the benefits, risks and basic implementation of concurrent and parallel programming.
Concepts covered include:

<div class="row t30">
<div class="small-3 columns"><img src="{{ site.urlimg }}parallel-left.jpg"></div>
<div class="columns large-9">
<div style="white-space: pre-wrap"><b>Thread safety</b>
Managing access to state, using threads to synchronize and handle async events, recognising where thread safety is broken and reference escaping.
<i>[Java]</i>
</div>
</div>
</div>

<div class="row t30">
<div class="small-3 columns"><img src="{{ site.urlimg }}parallel-right.jpg"></div>
<div class="columns large-9">
<div style="white-space: pre-wrap"><b>Sharing objects</b>
Recognising the risks of stale data in insufficiently synchronised programs, the difference between visibility and atomicity and strategies for safe publishing.
<i>[Java]</i>
</div>
</div>
</div>

<div class="row t30">
<div class="small-3 columns"><img src="{{ site.urlimg }}parallel-left.jpg"></div>
<div class="columns large-9">
<div style="white-space: pre-wrap"><b>Composing objects</b> 
Composing thread-safe components into larger programs, patterns for designing thread-safe classes, and the importance of encapsulation in concurrency.
<i>[Java]</i>
</div>
</div>
</div>

<div class="row t30">
<div class="small-3 columns"><img src="{{ site.urlimg }}parallel-right.jpg"></div>
<div class="columns large-9">
<div style="white-space: pre-wrap"><b>Concurrency tools</b>
Implementation and use of a Bounded Blocking Queue using producer/consumer pattern, synchronised collections and the use of different synchronisers (condition_variable, locks, mutex).
<i>[C++]</i>
</div>
</div>
</div>

<div class="row t30">
<div class="small-3 columns"><img src="{{ site.urlimg }}parallel-left.jpg"></div>
<div class="columns large-9">
<div style="white-space: pre-wrap"><b>Liveness hazards</b>
Recognising and avoiding liveness hazards such as deadlocks, livelocks and starvation. 
<i>[Java]</i>
</div>
</div>
</div>

<div class="row t30">
<div class="small-3 columns"><img src="{{ site.urlimg }}parallel-right.jpg"></div>
<div class="columns large-9">
<div style="white-space: pre-wrap"><b>GUI & threads</b>
Thread confinement, deadlocks and implementation of responsive GUI applications. 
<i>[Java]</i>
</div>
</div>
</div>


