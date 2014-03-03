---
layout: post
title: "An LGPL Framework and Proprietary Extensions"
date: 2014-03-13 12:40
comments: true
categories: joomla, lgpl
---

The question of proprietary extensions is a topic continuously used to take this thread off topic. But, since it keeps coming up, I'll try to address it here once and for all, but probably do a poor job of it in someone's estimation, for sure.

An LGPL Framework does not open the door to proprietary extensions any more than it is already open today. Today, if you want to legally create a proprietary extension (for this example we'll assume you want to create a proprietary component), you must do all the work yourself. All the minutiae of an extension, from Controller dispatching and event handling, CMS menu integration, view handling, output, model access, etc. You must do all of that yourself and not depend on any of the CMS implementations that make writing an extension easy. This is already possible today. The only requirement on you to have a component (at this point) is to have a component entry point file with the same name as your component. So if you're building com_licenses, you must have a licenses.php file in your component folder, located at [code]JPATH_ROOT . '/components/com_licenses/licenses.php';[/code]. The CMS includes that file, it gets executed, and any data returned gets put in the [code]<jdoc:include type="component" />[/code] block within your template index.php (or error.php, if you messed up). A GPL codebase which simply includes (in the sense of a PHP include, not meaning distributed with) does not infect that file with it's license. 

Neither will this change if the CMS were rewritten on top of the Framework (as opposed to currently, just including bits of the Framework were necessary). The (proposed) architecture of a future CMS built on the Framework is as follows.

- Framework Base Packages (application, controller, DI, model, view, registry)
- CMS Implementations (Adds CMS specific functionality, to make building extensions easier, on top of the Framework base packages)
- 3rd Party Extensions (Build on the CMS specific functionality, may implement some service-orientated packages, such as GitHub API or Twitter API etc from the Framework)

As you can see, 3rd Party Extensions build on the CMS implementations of the Framework Base Packages, they do not use the Framework themselves. So even if the Framework was LGPL, Extension Developers still must extend / implement GPL code (via the CMS implementations), meaning their extension must be GPL.

Now, there is a way around this, and that is to do everything yourself, just like you have to do now in order to build a proprietary extension. Ignore all of the CMS implementations that ease your life as an extension dev and use no GPL code whatsoever, and you have a legal proprietary extension built for the CMS. As you can see, that's no different than what you have to do now to accomplish the same thing.

Furthermore, the section 3 of the [LGPL license](http://www.gnu.org/licenses/old-licenses/lgpl-2.1.html) states:

<blockquote>
You may opt to apply the terms of the ordinary GNU General Public License instead of this License to a given copy of the Library. To do this, you must alter all the notices that refer to this License, so that they refer to the ordinary GNU General Public License, version 2, instead of to this License. (If a newer version than version 2 of the ordinary GNU General Public License has appeared, then you can specify that version instead if you wish.) Do not make any other change in these notices.

Once this change is made in a given copy, it is irreversible for that copy, so the ordinary GNU General Public License applies to all subsequent copies and derivative works made from that copy.

This option is useful when you wish to copy part of the code of the Library into a program that is not a library.
</blockquote>

In the discussions, the question was raised "what happens when the Framework is included in the CMS? how does that effect the CMS?", and the agreed upon answer was that the Framework, when distributed with the CMS, would be under the GPL license in accordance with section 3 of the LGPL v2.1 license.

So, even if you went through all the trouble of doing everything yourself to write an extension, but based your code on the Framework, your extension would still have to be GPL, because the Framework, when taken with the CMS [i]is[/i] GPL, in accordance with the license.

I sincerely hope we can now put this specific off-topic topic to rest.
