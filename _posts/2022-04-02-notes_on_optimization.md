---
layout: post
title: Notes on VRP Optimization
---
<p class="indent">
 As consumers, most of us don't put a great deal of thought into how the products we purchase travel
from storage facility to doorstep - we mostly just care that they arrive as scheduled and in good
condition. Beneath the surface, though, is a fascinating technical problem that always goes
something like this:
</p>
<p>
<blockquote>
 A company maintains a fleet of <em>vehicles</em> which need to service a collection of
<em>customers</em> in a certain <em>time-frame</em>.</blockquote>
</p><p>
How do we make sure that we are doing this efficiently?
</p>
<p>
Failure to do so results in extra cost due to <strong>fuel consumption</strong>, <strong>vehicle maintenance</strong> and
<strong>labor</strong>. When you take these considerations together with special constraints defined by the
particular business, such as a <em>maximum working day</em>, <em>guaranteed time</em>, and <em>time windows</em>,
you get <abbr title="Vehicle Routing Problem">VRP</abbr>. 
</p>

<p class="indent">
<abbr title="Vehicle Routing Problem">VRP</abbr> is a generalization of the Traveling Salesman Problem - no exact solutions are known for the size
of practical, real-world data-sets, and so typically heuristics are employed to reach reasonable
solutions that may not be theoretically optimal.<code>Optimality is typically defined by the business,
and then coded into a set of mathematical constraints.</code>
</p>

<h2> Defining the data model</h2>
<p>
Any solid <abbr title="Vehicle Routing Problem">VRP</abbr> project needs to begin with an analysis of the available data and the generation of a
data model, which is a mathematical description of the entities you are attempting to optimize. In
fact, results can vary wildly based on small changes in this model, so it pays to get it right. 
</p>
For <em>any</em> use case, you need to be able to answer the following questions:
<p>
* Can I assume my <strong>vehicle fleet</strong> is homogeneous? (Do they all have the same capacity)
* Can I assume my vehicles begin and end their journey in the same place? 
* Do I have time restrictions? (Can customers be served at any time?)
* Do any customers have priority over any others?
* Are my vehicles allowed to *refill* after a delivery? 
* Can any vehicle service any customer?
</p><p class="indent">
The answers to all of these questions give you a different variant of the traditional <abbr
title="Vehicle Routing Problem">VRP</abbr>
problem, and each variant will require subtly different <strong>metaheuristics</strong> to solve optimally - and
this is before we've added special business logic to the list of constraints.
</p>
<h3> Data Collection</h3>
<p>
Additionally, you'll need to determine if the data you have available is sufficient for getting
proper results from a solver. Geospatial data is notoriously noisy, and historical dispatches
often contain traces of judgment errors on the part of the dispatcher, which will ruin A/B
comparisons.
</p>
For any <abbr title="Vehicle Routing Problem">VRP</abbr>, your dataset will have to be capable of supporting the following:
<p>
* Retrieving a geo-encodable address for every pickup and every delivery
* Associating a trip time between every pickup and every delivery 
* Providing a vehicle count which is *mathematically capable* of servicing all of the requests
* Providing a starting location for every vehicle (this can be the same for all vehicles)
* Providing a total <strong>demand</strong> for each customer
</p>
<h4> A note on distance & time calculations</h4>
<p>
Getting trip times between points is <em>expensive</em>, and there are basically three ways to do it. One
is to use a matrix api, which is the most expensive option. When the number of pickups and/or
deliveries exceeds a threshold <code> x </code> (different for each provider), your costs will sky-rocket. The
second option is not to encode the trip distances some other way (usually using the <strong>Pythagorean
Theorem</strong>), which gives you an approximation which is reasonable more often than not, but cannot
account for traffic and road networks. This is inexpensive, but brittle.
</p>
<p>
The final option is to use whatever historical data you have to associate a trip time with a node.
For instance, if you have data on repeat customers that indicates your vehicles will average <code> y</code>
miles to travel to this customer's address, using this data inside a solver can sometimes be even
more accurate than a time matrix.
</p>
<p>
The problem with the final approach is that is difficult to isolate the trip time for a single
customer from the circumstances that led to that trip time.Perhaps you are looking at a
particularly busy day, or a combination of requests that put your trucks further from the delivery
location than they would normally be.
</p>
** WORK IN PROGRESS**
 
