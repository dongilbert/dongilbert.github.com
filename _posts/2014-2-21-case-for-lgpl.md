---
layout: post
title: "A Case for the LGPL"
date: 2014-2-21 12:51
comments: true
categories: joomla, lgpl
---

This past week has been pretty crazy in the Joomla world. The Framework developers and PLT requested that OSM change the license of the Framework from GPL to LGPL (something we are legally allowed to do under the terms of the Joomla Contributor License Agreement). Who knew all hell would break loose about the topic? The first reply in the discussion area on the topic was full of FUD about the actual change, and a flood of "+1 Brian! NO LGPL!" soon followed. My post (copied below) got buried on the second page, so I wanted to repost it here.

Some unbiased facts:

- 99.99999% of the code is covered by the JCA, which gives OSM the legal right to relicense the code as LGPL.
- 48 lines of code that remain in the Framework are not covered by the JCA.
- 25% of the Framework contributors explicitly support changing the license to LGPL. The remaining 75% implicitly support it by way of the JCA.
- The Framework Team is unanimous in its request to change the license to LGPL v2.1+
- The PLT is unanimous in its request to change the license to LGPL v2.1+

What you can learn from these facts is that those people who actually wrote the Framework codebase support the change. I think it would be reasonable to support them in this request.

Furthermore, the LGPL is an open source license and it satisfies the 4 freedoms that we all know and love. An excerpt taken from https://www.gnu.org/philosophy/free-sw.html

<blockquote>
A program is free software if the program's users have the four essential freedoms:

- The freedom to run the program, for any purpose (freedom 0).
- The freedom to study how the program works, and change it so it does your computing as you wish (freedom 1). Access to the source code is a precondition for this.
- The freedom to redistribute copies so you can help your neighbor (freedom 2).
- The freedom to distribute copies of your modified versions to others (freedom 3). By doing this you can give the whole community a chance to benefit from your changes. Access to the source code is a precondition for this.
</blockquote>

All 4 freedoms are kept in tact by a license change to LGPL v2.1+.

Richard Stallman wrote a great post on ["Why you shouldn't use the Lesser GPL for your next library"](https://www.gnu.org/philosophy/why-not-lgpl.html). Brian Teeman already alluded to this post, even though he ignored the strongest argument <i>for</i> the LGPL that is stated therein.

<blockquote>
Using the ordinary GPL is not advantageous for every library. There are reasons that can make it better to use the Lesser GPL in certain cases. The most common case is when a free library's features are readily available for proprietary software through other alternative libraries. In that case, the library cannot give free software any particular advantage, so it is better to use the Lesser GPL for that library.

This is why we used the Lesser GPL for the GNU C library. After all, there are plenty of other C libraries; using the GPL for ours would have driven proprietary software developers to use another—no problem for them, only for us.
</blockquote>

The second sentence states that it would be advantageous for a library to use the LGPL when a free library's features are readily available through alternative libraries. This, in my opinion, is the strongest argument for the LGPL. Nothing in the Joomla Framework is unique. It is a set of building block libraries, whose functionality is readily available in alternative libraries. What differs in the Framework is the implementation of that functionality in ways that makes sense to the contributors. This however doesn't suggest uniqueness, but rather an alternative implementation for the same functionality.

Stallman goes on to state in the second quoted paragraph that using the GPL for a library whose features are readily available in an alternative can actually hurt that library. Using the GPL for the Framework will drive proprietary software developers to use another - no problem for them, only for us.

Where Stallman's "why not the LGPL" argument breaks down is when he turns it from a technical argument into a religious one. His last paragraph (that Brian quoted) starts with <blockquote>But we should not listen to these temptations</blockquote>. The only other place you read such warnings against evil is in religious texts, as when the serpent beguiled Eve, or when in the Psalms David is writing his son to be wary of "the strange women".  However, this is <b>not</b> a religious debate, but a technical one.

<b>tl;dr;</b>
Based on the technical merits of the LGPL v2.1+, the fact that it respects the 4 freedoms, as well as the support of those who actually wrote the code for the Framework, I feel we would be remiss to not take action and approve the request.
