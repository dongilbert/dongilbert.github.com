---
layout: post
title: "Why E_STRICT Matters"
date: 2014-05-08 12:40
comments: true
categories: php, errors, standards
---

PHP has many [different error levels] [1], a few of which get ignored by a lot of developers I know.
In particular, some of the developers I know don't really care about `E_STRICT`. When developing their
applications, they tend to take care of the errors that halt execution (`E_ERROR`) but don't care that much
about "the little things" like an unmatched method signature or calling a non-static method statically.
What's the big deal, right? The PHP documentation further propogates this idea that `E_STRICT` isn't
that important by saying that `E_STRICT` errors are:

> suggest[ed] changes to your code which will ensure the best interoperability and forward compatibility

If it's just a suggestion, who's to say PHP is right? I mean I know my code and its purposes better than
the PHP parser - why should I take the advice?

Well there's actually good reasons to take care of all `E_STRICT` errors that you see on your site. (You
should even take care of the ones you don't see, hidden ones that only get logged to your `error_log` file.)
The major benefit that I'll focus on today is "forward compatibility".

### Forward Compatibility

Fixing any `E_STRICT` errors helps make your code forward compatible. We've all heard that large projects
should worry about keeping their code "backward compatible" so that others who use it and depend on it don't
have a hard time keeping up with the breaking changes, but what is this about forward compatibility?

In its simplest form, forward compatible code is ready to do its best to take on the future, whatever may
happen. It's being proactive about your code and staying on top of changes in upcoming releases. It's making
sure you're following best practices and suggestions in order to protect yourself from potential backward
compatibility issues. It's taking the information you have available and writing your best code with it in
order to be prepared.

That definitely sounds like "a good thing<sup>tm</sup>", right? While many developers want to achieve those
things, they don't do the things necessary in order to accomplish them. One of the things you can do to better
ensure forward compatibility is to take care of `E_STRICT` errors on your site, rather than brushing them off
as useless suggestions.

For example, you've probably seen the following error before on your site:

> Error: Declaration of YourCustomClass::store() should be compatible with that of ConcreteClassYourExtending::store($updateNulls = false)

What this means is that when you were writing your code, you didn't match the _method signature_ [^2] in your
custom class with that of the parent class. The reason this is a problem is that your custom class becomes
very narrowly focused and is not able to be reused in other places where a class that extended the parent
class would be able to be used.

Furthermore, it prevents progress in the code at the architecture level. As we've already learned, when you
extend a base class and don't match the signature, it's "just an `E_STRICT`" error; application execution
doesn't stop. However, if you wanted to start improving application architecture by starting to
[_program to an interface_] [3], not following method signature can now throw a fatal error. Consider the 
following code.

```php
<?php

class BaseController
{
    public function getAction($id = null)
    {
        // do stuff with the $id
    }
}

class UserController extends BaseController
{
    public function getAction()
    {
        // get the id from a request object instead
        $id = $this->input->get('id');
        
        // do stuff with $id
    }
}
```

With that code, if you had `E_STRICT` enabled for reporting, you'd see the "X doesn't match method signature of Y"
error that I noted earlier. No big deal, it still runs, right? Well let's assume that the `BaseController` presented
above is part of a framework, and the `UserController` is your custom code. So now the framework wants to start
programming to an interface, so it creates `ControllerInterface` and implements that in the `BaseController`.

```php
<?php

interface ControllerInterface
{
    public function getAction($id = null);
}

class BaseController implements ControllerInterface
{
    public function getAction($id = null)
    {
        // do stuff with $id
    }
}
```

When the framework developer runs the unit tests, everything is just fine, because the method signatures match
that of the interface and no errors are thrown. An update is released and you go ahead and apply it, since it
was just a new patch release anyways, nothing should break, only to find that the `E_STRICT` error you had been
ignoring is now an `E_ERROR` that breaks your application. Oh yeah, did I forgot to mention that you applied
this update in production on a Friday afternoon at 4:45 PM, which you now have to fix before you can leave for
the weekend?

All of that could have been avoided if you just fixed the `E_STRICT` error, but you instead chose to ignore it.
Forward compatibility is worth a 2 min code change, don't you think?

[1]: http://www.php.net/manual/en/errorfunc.constants.php "Predefined Error Constants" 
[^2]: Method Signature: A method's signature is the type and number of arguments that are set in the method
declaration. Think of it as the fingerprint of the method. It's always the same, and extending classes
should keep it the same as well.
[3]: http://www.php5dp.com/design-pattern-principles-for-php-program-to-an-interface-not-an-implementation/ "Program to an Interface, not an implementation"
