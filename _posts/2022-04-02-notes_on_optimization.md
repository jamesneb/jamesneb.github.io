---
layout: post
title: Notes on VRP Optimization
---

As consumers, most of us don't put a great deal of thought into how the products we purchase travel
from storage facility to doorstep - we mostly just care that they arrive as scheduled and in good
condition. Beneath the surface, though, is a fascinating technical problem that always goes
someting like this: A company maintains a fleet of *vehicles* which need to service a collection of
*customers* in a certain *timeframe*. How do we make sure that we are doing this efficiently?
Failure to do so results in extra cost due to **fuel consumption**, **vehicle maintenance** and
**labor**. When you take these considerations together with special constraints defined by the
particular business, such as a **maximum working day**, **guaranteed time**, and **time windows**,
you get <abbr title="Vehicle Routing Problem">VRP</abbr>. 
<p>
VRP is a generalization of the Traveing Salesman Problem - no exact solutions are known for the size
of practical, real-world data-sets, and so typically heuristics are employed to reach reasonable
solutions that may not be theoretically optimal. Optimality is typically defined by the business,
and then coded into a set of mathematical constraints.
<p>

## Defining the data model

Any solid VRP project needs to begin with an analysis of the available data and the generation of a
data model, which is a mathematical description of the entities you are attempting to optimize. In
fact, results can vary widlly based on small changes in this model, so it pays to get it right. 

For *any* use case, you need to be able to answer the following questions:

* Can I assume my *vehicle fleet* is homogenous? (Do they all have the same capacity)
* Can I assume my vehicles begin and end their journey in the same place? 
* Do I have time restrictions? (Can customers be served at any time?)
* Do any customers have priority over any others?
* Are my vehicles allowed to *refill* after a delivery? 
* Can any vehicle service any customer?

The answers to all of these questions give you a different *variant* of the traditional VRP
problem, and each variant will require subtly different **metaheuristics** to solve optimally - and
this is *before* we've added special business logic to the list of constraints.

Additionally, you'll need to determine if the data you have available is sufficient for getting
proper results from a solver. Geospatial data is notoriously *noisy*, and historical dispatches
often contain traces of judgment errors on the part of the dispatcher, which will ruin A/B
comparisons.

For any VRP, your dataset will have to be capable of supporting the following:

* Retrieving a geo-encodable address for every pickup and every delivery
* Associating a trip time between every pickup and every delivery 
* Providing a vehicle count which is *mathematically capable* of servicing all of the requests
* Providing a starting location for every vehicle (this can be the same for all vehicles)
* Providing a total **demand** for each customer

Getting trip times between points is *expensive*, and there are basically three ways to do it. One
is to use a matrix api, which is the most expensive option. When the number of pickups and/or
deliveries exceeds a threshold **x** (different for each provider), your costs will sky-rocket. The
second option is not to encode the trip distances some other way (usually using the Pythagorean
Theorem), which gives you an approximation which is reasonable more often than not, but cannot
account for traffic and road networks. This is inexpensive, but brittle.

The final option is to use whatever historical data you have to associate a trip time with a node.
For instance, if you have data on repeat customers that indicates your vehicles will average **y**
miles to travel to this customer's address, using this data inside a solver can sometimes be even
more accurate than a time matrix.

The problem with the final approach is that is difficult to isolate the trip time for a single
customer from the circumstances that led to that trip time.Perhaps you are looking at a
particularly busy day, or a combination of requests that put your trucks further from the delivery
location than they would normally be.

** WORK IN PROGRESS**
 
