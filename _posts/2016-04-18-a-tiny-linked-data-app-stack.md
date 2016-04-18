---
layout: post
title: A Tiny Linked Data Application Stack
date: 2016-04-18
author: Kevin Clair
---

For a few months I've been kicking around some ideas on ways to publish a small amount of RDF triples (currently <50K; I'm sure it'll grow as I expand and refine the data but probably not more than 1M), consisting of metadata about local name authorities from our [special collections and archives](https://duarchives.coalliance.org), so that I can assign URIs to our local terms. Potentially I would like to open it up to other memory institutions around Colorado and Wyoming who manage similar name authority data, so that they can use the URIs and contribute their own data to the project, but first I want to get DU's house in order.

Here is what I have so far (all on my laptop, nothing in production):

* Triples stored in Jena Fuseki; one day also to be published with URIs at a dedicated project site
* Indexed in Solr, somehow (probably via JSON-LD but if it indexes N-Triples directly, so much the better)
* Public access via Blacklight

Here are some questions I have:

* ...could Solr index N-Triples directly? Everywhere online I looked said to use JSON instead, but that's not how I've been representing my RDF up to this point.
* Suppose I wanted to elicit contributions from others. Is this a thing that's done? What are some tools I should consider for that?
* Am I missing some obvious step I can't see?

Going to live this project out in public for a while and hope it doesn't prevent me from presenting it at a conference somewhere. [Send me an e-mail, please](mailto:kevin@jackflaps.net), if you have ideas. thanks!
