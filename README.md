# vertx-push
Send push notifications asynchronously in your [vertx](http://vertx.io/) application with [OneSignal](https://onesignal.com/).

#### example
```
//Create a client. You get the APP_ID and API_KEY from the OneSignal-dashboard
PushClient.create(Vertx.vertx(), "YOUR_APP_ID", "YOUR_API_KEY").
                //setup the content of the message on the serverside
                withContent(new JsonObject().put("en", "English Content.").put("de","Deutscher Titel.")).
                //all users should receive this
                targetBySegments(Segments.ALL).
                sendNow(
                        h -> {
                            if (h.succeeded()) {
                                System.err.println(h.result().encodePrettily());
                            } else {
                                h.cause().printStackTrace();
                            }
                        });
```

More examples can be found in the ``Examples``-class.

#### why would I use this library over writing my own CURL?
Sure you can do this. What this library gives you on top is validation. When writing your own request it is absolutely
fine to both target users by segments and filters. This library makes sure that you only use one targeting parameter.
Also the content of a message can either be defined by using templates _or_ setting the content like in the example above, this
library only lets you set on of the ways to deliver messages.

**Filters**

Targeting users using filters can be tricky when writing your own CURL call. You have to remember which relation can be used
for which filter. This library only lets you use the relations that are allowed for a given filter.

Example:
```
//using a filter to target only users that haven't used the app for 24 hours
Filters.lastSession().greater(24);  //OK
Filters.lastSession().less(24);     //OK
Filters.lastSession().exists(24);   //Compiler-error

//Combining filters using OR
Filters.lastSession().greater(24).or(Filters.sessionCount().equal(1));
...
```

# requirements
To use this library you need to create a [OneSignal](https://onesignal.com/)-account and configure your clients (web or mobile) accordingly.

# disclaimer
The author is not linked to the companies behind OneSignal or Vertx. This library also comes without any warranty - just take
it or leave it.