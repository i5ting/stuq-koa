// Let's first see how to inject JS logic into your JADE templates
// JADE supports its own constructs for conditionals and loops to make your templates cleaner :) Let's see it in action!

- var users = [{name: 'foo', role: 'admin'}, {name: 'bar', role: 'manager'}, {name: 'baz', role: 'technician'}]

h2 Users

// Neat! There's another construct called `each`
// Also there is `unless` which is equivalent to if (!expr)
// Let's use that and swap a bit of code

each user, index in users
	unless user.role === 'admin'
		p #{user.name} is not an "admin"
	else
		p #{user.name} is an "admin"

// Let's take a look at `case` statements now

h3 case

case users[2].name
	when 'admin'
		p User is an admin
	when 'manager'
		p User is a manager
	when 'technician'
		p User is a technician
	default
		p User is a customer!

h3 mixins

// mixins are pieces of code that you can reuse as functions. they may or may not accept arguments

mixin fruits
	ul
		li Apple
		li Banana
		li Orange

h2 Fruits
mixin fruits

mixin users(name, role)
	li(attributes) #{name} has a role of #{role}

// Another way to call mixins that provides with a few extra features that you're going to see in a second.
// The attributes, id, class have been added to the li's. You can style them from CSS now.

+users(users[0].name, users[0].role)#admin
+users(users[1].name, users[1].role).manager
+users(users[2].name, users[2].role)(title="technician")

// Mixins are excellent for dropdowns

mixin user_list(name)
	option(value="#{name}") #{name}

select
	for user in users
		+user_list(user.name)

// Great! Now finally let's take a look at "filters". filters must be prefixed with ':', for example ':coffeescript' or ':markdown'. Jade currently supports :stylus, :less, :markdown, :cdata and :coffeescript. Let's see how to use the coffee filter.

:coffeescript
	(->
		document.querySelectorAll('.manager')[0].style.color = 'blue'
	)()

// Some random piece of code but you get the idea :)
// That's all!
