---
layout: post
title: Writing ArchivesSpace Plugins
date: 2015-12-04
author: Kevin Clair
---

Part of my job is to customize [our instance](http://duarchives.coalliance.org) of [ArchivesSpace](http://archivesspace.org) to account for the peculiarities of our processing workflow at [work](http://library.du.edu). Like every archive, we have certain classification conventions for our collections and their components; we also have a [digitization program](https://specialcollections.du.edu), for which we'd like to track materials and run reports. Our archivists had also become accustomed to certain things in our old collection management system that ArchivesSpace doesn't have, and while some of them are on the LYRASIS development docket, it's sometimes faster just to get those features back ourselves.

In addition to some [local customizations](https://github.com/duspeccoll/plugins_local) we've done, I've written three plugins that are, or will soon be, in production:

* [next resource](https://github.com/duspeccoll/next_resource) allows an archivist to select one of our four collecting areas; based on this choice, it loads the New Resource form, with the identifier field pre-populated with whatever the next collection number in sequence for that collecting area happens to be
* [next accession](https://github.com/duspeccoll/next_accession) does the same thing, only for Accessions
* [digital object manager](https://github.com/duspeccoll/digital_object_manager) aims to one day be a one-stop shop for managing our digital objects and synchronizing the metadata about them in ArchivesSpace with the metadata about them in our Islandora repository. Currently the only thing it does is copying the metadata from an item (Archival Object) record into a new Digital Object, then creating a link between the two. We do this so that we can get a complete MODS record for Islandora ingest. (It *seems* like we could just pull Archival Object metadata into the MODS record without this extra step as long as it's linked to the Digital Object, but I haven't gotten it to work yet; if you have, [let me know?](mailto:kevin@jackflaps.net))

I wanted to write a little about how I've gotten to the point where I can punch out three plugins. I don't typically think of myself as a developer--I was asked not to continue in the computer science program in college after I got a 68 in CS 102--but I knew a little bit from taking those two intro classes, and in my last job I wrote just enough Perl for various projects that I had a handle on the basics. Here are some of the things that have helped me on my way since coming to Denver:

* **A supportive and encouraging workplace.** All technological problems are essentially social, after all. I've been very fortunate to have both a supervisor willing to allow me the time and freedom to become familiar with the ArchivesSpace code base and how we can extend it to make our work easier, and a stakeholder community in the library that's very forthcoming with what isn't working for them, both during the migration and the 16-or-so months we've been in production on it. I won't say it's been all smiles, but when there are problems I hear very quickly exactly what they are. Especially when I was first starting out, having clear indications of what people wanted helped immensely with setting boundaries on what I should be doing and focusing my learning efforts. It's impossible to overstate how important building (and maintaining!) this kind of community is, especially since so much of what I do in this space was not in the original job description, and because ours was not really an open-source shop when I arrived.
* **An understanding hosting provider.** I would like to send a special shout-out to [our consortium](http://coalliance.org), who hosts our ArchivesSpace instance for us. I've been periodically able to get backups of our production database from them, for which I'm very grateful, because I can hack on production on either of my ArchivesSpace virtual machines (one at work, one on my laptop) without breaking production. It's a lot easier to mess around with code that won't work and might break the database if I can just drop it and re-install whenever.
* **Utterly immersing myself in the code base.** While I'm still nervous about submitting any pull requests, I've lost count of how many hours I've spent on [the ArchivesSpace Github](https://github.com/archivesspace/archivesspace), figuring out what different bits of the code mean. This was particularly helpful for learning Ruby, a language I knew nothing about before we got into ArchivesSpace; many was the evening spent with the ArchivesSpace code in one tab and Stack Overflow in the other, piecing things together. It's maybe a deeper dive than I would have been comfortable with had I been starting from scratch, but it worked nevertheless. Though not as well as...
* **Looking at other plugins.** [There are a bunch out there now](https://archivesspace.atlassian.net/wiki/display/ADC/Plugins+and+Scripts), though when I first tried my hand at it I was limited to the LCNAF plugin that shipped with the very early ArchivesSpace versions, and [the example plugin for generating accession identifiers](https://teaspoon-consulting.com/articles/archivesspace-plugins.html) that Mark Triggs wrote. We're not using either one of them anymore, but seeing examples that worked, and clear explanations of *why* they worked, were really what got me on my way. If you're just starting out, Mark's post is a really good introduction. Though progress was slow at first, because getting that first one written meant...
* **An unbelievable amount of trial and error.** It's one thing to see the plugin code; another altogether to try and fit all the model/view/controller pieces together in a way that successfully does what seemed so easy when you were doing it on command line through the API. This was lonely and stupid work and I frequently hated it! Many times I sunk an hour into figuring out why a controller wasn't working, only to learn it was something silly like using 'nil?' instead of 'empty?,' or forgetting an equals sign somewhere. But I spend about two hours a day on public transportation, and this is a good way to spend them. ("A heroically long commute without Internet access" is perhaps another thing that's helped me on my way, but that's another post entirely.)

"Great," you are perhaps thinking, "but how do I do it?" If I could start over with the benefit of hindsight, here is what I would do:

* **Get a little bit of understanding of Ruby.** The book everyone said I should read when I started was [why's Poignant Guide to Ruby](http://poignant.guide), <span class="small">which I haven't actually finished...</span> Or poke around [the Ruby on Rails guides](http://guides.rubyonrails.org/getting_started.html), which were also very helpful for me (especially the MVC stuff). The Javascript I've had to write is all pieced together from Stack Overflow searches, and there isn't enough of it to really justify diving any deeper in, but I wish I'd known more fundamentals of Ruby before I'd started; it would have probably saved me a ton of time.
* **Learn the API.** [Here](https://archivesspace.github.io/archivesspace/doc/file.API.html) is the documentation. Every plugin I've written or have started to write began as one or more API calls first. If you're feeling adventurous, [the backend controller source code](https://github.com/archivesspace/archivesspace/tree/master/backend/app/controllers) tells you what each API call does; I learned quite a bit about the guts of ArchivesSpace by searching for each method I found in there to see what they did.
* **Check out those other plugins.** There are so many more of them now than there used to be! The two I mentioned above are pretty uncomplicated examples of how different pieces of plugins fit together, but the others listed on the ArchivesSpace wiki are in production somewhere (I assume), and touch on way more aspects of ArchivesSpace functionality than I even knew were available.
* **Ask questions.** If you're a community member, the LYRASIS user group list is an excellent source of information; if you're not, the Google group is also excellent, though not as high-traffic.

I only sort of know what I'm doing yet, but it's been fun to play around and make mistakes and eventually get a product that saves everyone time and frustration. I'd be interested to hear about your experience; you can find me below.