---
layout: post
title: Fix Top Containers
date: 2016-06-14
author: Kevin Clair
---

[I wrote a Ruby script](https://github.com/duspeccoll/as_utils/blob/master/fix_top_containers.rb) that links the box records in our ArchivesSpace instance that won't automatically get linked to top containers when we migrate to v1.5.0. We needed to write this because we have boxes containing material from multiple series in a collection, and since up to this point we haven't had a collection management system that separates content from carriers, we can't uniquely identify a "box" record based on the barcode, because currently (we're on v1.4.2 at the moment) B063.01.*0035* and B063.03.*0035*, or whatever, will have the same barcode.

When we upgrade to a 1.5.0 release candidate, the migrator will import one instance of a barcode successfully, and then return "Mismatch when mapping between indicator and indicator_1" errors for all of the other instances of that barcode it finds. This script links all of the additional archival object instances of that barcode to the existing top container with that barcode on. If you'd like to use it, bear in mind that it only works with that specific error, because it's the only error we encountered with our data when we tested the 1.5.0 release candidates.

You can run the CSV report that the container conversion job generates as input to this script; to find it, go into "Background Jobs" in your 1.5.0 ArchivesSpace instance, click on "View" for the container_conversion_job type (if you've just upgraded it will be the first job in the list), and then click on the "File" link in the "Files" section of the job page. From this we derived a smaller CSV file consisting of just the archival object URI and its barcode. I used Open Refine for this because I'm better at data munging with GREL than I am with Excel; here are the steps I took:

1. Remove every column except the 'instance' column and the 'uri' column containing Archival Object URIs
2. From the 'instance' column, select "Edit column" and then "Add new column based on this column"
3. I named the column "barcode" but feel free to name it whatever. The expression value you'll want is: **partition(value, "barcode_1\"=>")[2].split(',')[0].replace("\"",'')**
4. You can now delete the 'instance' column and export the project as CSV. You'll need to delete the header row and change the newline characters from '\r\n' to '\n' for the script to work.

Hopefully this helps someone with their 1.5.0 upgrade! [Let me know](mailto:kevin@jackflaps.net) if you have any questions or advice about improving this; as ever, I'm figuring this out as I go.
