+++
title = "Reducing the Set Cover Problem to SAT"
description = ""
author = "David Blitz, Andreas Gemsa"
date = 2019-01-30T16:41:29+01:00
tags = ["SAT", "SET-COVER", "MiniSAT", "logistics"]
draft = true
+++

## The Motivation
The other day, we were discussing a problem that could come up in a logistics context: Imagine a warehouse that is divided into different zones - maybe each zone corresponds to a floor in the warehouse or different hallrooms. In each zone we store an amount of different items and each day we are processing a certain amount of orders which we are pooling together into one big list of items. The problem we were considering is the following:
How can we assign the items that are ordered each day to the zones such that we assign them to a minimal number of zones?

## The Formal Problem - Set-Cover
We can see this problem in a more abstract way: The list of ordered items, we can treat as a set of items which we designate by \\(O\\) - we are considering only sets of *different* items, i.e. no multi-sets. 
Each zone, \\(Z\\), can also be seen as a set of items - again no multiple item in each given zone. 
Hence, our warehouse \\(W\\) corresponds to a set of sets of items. Now, our problem can be seen as a search for a subset \\(U \subset W\\), such that all elements from \\(O\\) are included in one of the elements of \\(U\\), i.e. 
$$O \subset \cup U$$.
But this nothíng else than the Set-Cover problem which was treated in this (link!!) 1972 paper by Karp, in which he proved that Set-Cover along with 20 other problems are NP-complete(link!). 

## Excursion - NP-completeness
If you ever heard anything about complexity classes, you probably associate the phrase *NP-complete* with really hard. 
Why are SAT-Solvers fast?

