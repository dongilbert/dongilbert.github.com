---
layout: post
title: "A Case for Service Location"
date: 2014-01-03 16:30
comments: true
categories: development, PHP, "service location", "dependency injection"
---

Before you get out your torch and pitchforks, at least read this first paragraph. I know there is a prevailing attitude out there that can be summed up with "Service location is the devil's code!", but I'm here to tell you it's not really that bad; you just need to be very diligent in how you use it. Like everything in this world, even service location has its time and place. In my opinion, that place is in the application layer of your code.

## The Application Layer

It's an obvious point, but we all know that objects must be instantiated before you can use them. In my opinion, that instantiation is done in the application layer, as opposed to the domain layer or presentation layer. Creating your objects in the application layer and then using Dependency Injection to pass those objects into the domain layer as needed offers great flexibility in application design. It makes it very easy to test your domain layer by injecting test doubles where necessary and it also makes for loosely coupled code. The question now boils down to what makes up the application layer.

According to the <a href="http://en.wikipedia.org/wiki/GRASP_(object-oriented_design)#Controller">GRASP Controller Pattern</a>, your controllers are part of your application layer, and as such, must have access to the services required by the domain layer.

If you're worried about tight coupling between your controllers and the application layer, you shouldn't be. This is because controllers are (generally) simple classes which are not really meant for reuse outside of a particular application. (You may disagree here, but that's ok). As has been successfully illustrated by popular PHP micro-frameworks, much of a controllers functionality can be boiled down to simple closures (which proves how unnecessary they can be). Having your controllers as classes instead of closures is really just an OO way of grouping common tasks. You might have a `UserController` that handles all of your user-based tasks, or a `AdminController` to handle your admin tasks. However, in most cases, that's where the commonality ends. What I mean by that is the dependencies of the `UserController::createAction` method probably aren't the same dependencies as the `UserController::loginAction` method. If you were to inject all of the possible dependencies of your controllers into the class constructor, you'd end up with several unneeded dependencies being instantiated and passed around on any given request, and this overhead does come at a cost.

Controllers are meant to instantiate parts of your domain layer and retrieve data from them, and then pass that retrieved data off to your view for processing, then return the processed response. If your controllers don't have access to the services required by the domain layer, then they can't complete their job properly.

So in my opinion, you really have three options:
 - Inject all possible dependencies into the controller, adding more as needed.
 - Instantiate services needed by the domain layer in your application layer, thus cluttering the application layer with instantiation logic.
 - Use service location in your application layer.

I think it's time for a code example. Consider the following base controller:
<pre class="php">
namespace App;

class BaseController
{
	protected $locator;

	public function __construct(ServiceLocator $locator)
	{
		$this->locator = $locator;
	}
}
</pre>
And this user class:
<pre class="php">
namespace App;

class UserController extends BaseController
{
	public function indexAction()
	{
		$db = $this->locator->get('db');
		$model = new UserModel($db);

		$users = $model->getAll();

		$view = new View;
		$view->setLayout('list');
		$view->set('users', $users);

		return $view->render();
	}

	public function loginAction()
	{
		$input = $this->locator->get('input');
		$form = new UserForm;
		$form->set('username', $input->getString('username'));
		$form->set('password', $input->getRaw('password'));

		if ($form->validate())
		{
			// login the user, redirect
		}
		else
		{
			// redirect to login again and display form
			$view = new View;
			$view->setLayout('form');
			$view->set('form', $form);

			return $view->render();
		}
	}
}
</pre>
In the above contrived example, you can see there are different dependencies between the `indexAction` and the `loginAction` methods. The former gets the `db` service from the container in order to pass it into the constructor of the `UserModel` class. In the latter, we see that in this case we only needed to get the input, no need to pass any further dependencies into the domain layer. (The astute will recognize that we are practicing Service Location in the controller, and Dependency Injection in the domain layer.)

It's a very simple example, I know, but I think it gets the point across. Different actions within a controller won't necessarily have the same dependencies, so injecting all possible dependencies quickly becomes an exercise in madness. Given that project scope can and does change, often changing the required dependencies along with it, it is much more simple to use Service Location in your controllers as needed.

## The Domain Layer

Don't even think about it. Service location should _never_ be used in your domain layer. Doing so couples your domain logic to your application, making it harder to test and reuse.

### Closing Thoughts

I believe that most of the negative attitude towards Service Location comes from improper implementations. Just because a pattern is useful or "works" doesn't mean you should use it in every situation. Widespread abuse of a pattern is what generates negative attitudes. 

However, just because it's possible to abuse something doesn't make it bad for every situation either. Swearing off a practice and refusing to use it just because it's possible for it to be abused is a pretty narrow-minded outlook on development (and life).
