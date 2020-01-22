## Using Neo4J in Business Process Modeling Scenarios

When people think about Neo4j and problems it's good at solving there's a pretty wide range of use cases that comes to mind - recommendation engines, fraud ring detection, bills of materials, social network scenarios, multi-level marketing systems, asset management and analysis, role based access control, graph algorithms and so on.

One of the use cases that don't strike people as being "graphy" per se is Business Process Management. So let's talk about this and hopefully introduce yet another use case to the ever growing list of things Neo4J is good at.

Let's take a look at a very simple business process.
+ Requestor submits a request
+ Approver reviews it and either approves the request (at which point the process is done) or rejects it and the requestor has to modify the request based on approver's input and re-submit it


![alt text](https://github.com/rossgabay/neo_bpm_blog/blob/master/bpm_sample_resized.png)

Doesn't get much simpler than that! 
So now let's think about the data nomenclature involved here and what we can do with it.

1. Request
2. Request State
3. Request Lifecycle Events - submitted, approved, rejected
4. Metadata associated with Lifecycle Events - approved/rejected by/when/comments/etc
5. Actors involved in the process - request submitters and approvers
6. Roles assigned to these Actors to indicate what they can do to requests in the certain stages of the flow
7. Process itself! The sequence of events, allowed state transitions, etc.


What can we do with this data as in what kinds of questions can we answer based on it?
Well, we can do things like:

+ Find all Requests in the system
+ Find all Requests in the certain state
+ Find all Requests submitted by a certain Requestor
+ Find out whether a specific Request can be approved/rejected/re-submitted
+ See what comments were provided when a Request was rejected
+ Check what state transition is allowed for a specific request 
+ Check whether state transition being applied to the request is allowed based on Actor's role
+ If we get ambitious - maybe an end-to-end lifecycle of a Request as it's traveling through the flow, that would be really cool

there's more we can do here but this is a good starting point.

Let's take a stab at building a data model based this nomenclature and see if/how we can answer these questions.
Data model will have three major parts to it:
1. Business Process Model elements
2. Request - related elements
3. Role Based Access Control elements

Let's start with the Business Process Model. There is a couple of different ways to think about it - for example we can think about it in term of the Actions performed by Actors or we can think about it in terms of the States our Requests can travel through in the system. Let's take the State-based approach and see how things pan out.

```
create (ss:StateSubmitted)
create (sa:StateApproved)
create (sr:StateRejected)
create (ss)-[:ALLOWED_TRANSITION]->(sa)
create (sa)-[:ALLOWED_TRANSITION]->(sr)
create (sr)-[:ALLOWED_TRANSITION]->(ss)
return ss,sa,sr
```







