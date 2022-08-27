---: Displaying Data | CRONje.ME
label: Displaying Data
order: 12
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
tags: [dev,tools,js,frontend,backend,devtools,php,laravel]
---
# Displaying Data

Now that we have made the `users` available to our view, we can display them like so:

```php
    @extends('layout')

    @section('content')
        @foreach($users as $user)
            <p>{{ $user->name }}</p>
        @endforeach
    @stop
```

You may be wondering where to find our `echo` statements. When using Blade, you may echo data by surrounding it with double curly braces. It's a cinch. Now, you should be able to hit the `/users` route and see the names of your users displayed in the response.

This is just the beginning. In this tutorial, you've seen the very basics of Laravel, but there are so many more exciting things to learn. Keep reading through the documentation and dig deeper into the powerful features available to you in [Eloquent](/docs/4.2/eloquent) and [Blade](/docs/4.2/templates). Or, maybe you're more interested in [Queues](/docs/4.2/queues) and [Unit Testing](/docs/4.2/testing). Then again, maybe you want to flex your architecture muscles with the [IoC Container](/docs/4.2/ioc). The choice is yours!
