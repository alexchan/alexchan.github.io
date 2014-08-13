Title: Task Queue Routing in App Engine
Date: 2014-08-12 09:33
Author: alex
Category: Technology
Slug: task-queue-routing-in-app-engine

Introduction
-----
When working with App Engine's task queues, it may be confusing to understand where and how a
particular task is being executed.  If working with different versions and modules, things can
get confusing fast.  Fortunately task queue processing is fairly simple if you keep a few things
in mind.  There are two main components with using task queues; the queuing of the task with
arguments and the execution of the task.  In general, this article will focus primarily on push queues.

Enqueue Task
----
Push-tasks are queued by using the taskqueue.add() function or by creating a Task object and
calling its add function.  A push task almost has the same properties as a request object with some
additional routing information.  When you enqueue a task, you define all the parameters of 
the task such headers, payload, method, url and params; properties you would find in a request object.
In addition, you can also specify other properties such as eta, countdown and whether the task is transactional. 
For routing, you can specify a task target when calling the .add() function.  This parameter
will be prepended to the hostname when the task is created.  For example: if your
application name is "my-app" and you specify a target value of "my-version", the url that will be
used to execute the task will be:

http://my-version.my-app.appspot.com

To target a specifc version of a module, you would set a target value of "my-version.my-module" and
the url that will be used in the task will end up being:

http://my-version.my-module.my-app.appspot.com

*Please keep in mind that if you are queuing a task defined in queue.yaml and the named queue in
 queue.yaml is configured with a target property, your specified target value when you make the
 .add() function call will be ignored.*

Task Execution
----
The key thing to remember about tasks in push queues are that they are simply requests to your
application's task endpoint.  This means that they must follow the same rules as any other request
including abiding by dispatch.yaml routing rules and instance.version.module hostname conventions.

If a target is not specifed when a task is added to a queue, the task will be executed in the same
module/version where the task was enqueued unless it becomes rerouted by queue.yaml or dispatch.yaml.

References
-----
For additional information and details, please check out the link below:

- [https://developers.google.com/appengine/docs/python/taskqueue/](https://developers.google.com/appengine/docs/python/taskqueue/)
- [https://developers.google.com/appengine/docs/python/modules/routing](https://developers.google.com/appengine/docs/python/modules/routing)

