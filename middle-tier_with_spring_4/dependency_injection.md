---
# This section is used to configure this lesson on Newline.
# `token` can not be changed; however, the rest of the
# information can be edited as needed. Changes you make
# here will overwrite the existing content on Newline
# when you push to the master branch of this path's GitHub repo.

# Begin the body of the lesson below the final `---`.

token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJjb250ZW50X2lkIjoyNzUzNCwiY29udGVudF90eXBlIjoiTGVzc29uIn0.FtETazSDhQRCeTNGJVU5iKGfxfZ7oCVTZMT8wxsBzWU

# Everything below this line can be edited.
title: >-
  Dependency Injection
description: >-
  A short treatise about the use of dependency injection.
---

Dependency Injection (DI) is probably one of the most dead simple implementation
choices I know but also one of the most difficult to explain, as well. I hope
this accessible example provides insight to you about the architectural
decision to use DI in your application.

## An Example

I'm going to use the example of a Web application to frame this introduction. I
haven't included anything obtuse; you just need to have some understanding of
object-oriented programming.

HTTP is a stateless protocol. Developers of Web applications want to track a
user's session, so the cookie was invented. As part of the HTTP standard, the
cookie gets sent with every request that match the domain for which the cookie
is set. Web frameworks have included session-tracking libraries and
functionality as part of their offering to ease the development of "stateful"
Web applications.

### Creating a "Stateful" Controller

Let's say that a controller class in our Web application requires the use of a
session. In its constructor, we could create an instance of some session
manager like this.

```java
@Controller
public class UserController {
  private SessionManager sessionManager;

  public UserController() {
    sessionManager = new InMemorySessionManagerImpl();
  }

  // Methods that use the session manager
}
```

Everything works great for a while. Then, one day, we decide to build another
Web application and reuse our well-received session manager. Unfortunately, the
requirements dictate that we use a different cookie name than we previously
used. So, we make that configurable through a constructor parameter and go back
to our controller to make the necessary changes.

```java
@Controller
public class UserController {
  private SessionManager sessionManager;

  public UserController() {
    sessionManager = new InMemorySessionManagerImpl("WEB_APP_SESSION");
  }

  // Methods that use the session manager
}
```

Again, everyting is fine, until someone posts about our Web application on
Reddit. All of a sudden, we get 100,000 new users and we need to scale our Web
servers to meet the new demand. The in-memory implmentation of the session
manager that we had now needs to store the session in a shared repository so
any Web server that handles the request can access the session manager.

In response to this new need, we could change the controller to look like this.

```java
@Controller
public class UserController {
  private SessionManager sessionManager;

  public UserController() {
    sessionManager = new RedisSessionManagerImpl("WEB_APP_SESSION");
  }

  // Methods that use the session manager
}
```

But, maybe Redis isn't the correct solution long-term. Maybe we want to put it
in the database one day. The crux of the problem is this:

> We are asking the `UserController` to make an infrastructure decision.

The class `UserController` should do one thing, if it follows the [Single
Responsibility
Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle) (SRP),
and one thing only: *manage interactions of user information*. The decision of
where to store sessions lies outside that scope of responsibility.

A way to solve this is to provide the session manager to the `UserController`
at the time of its creation, like this.

```java
@Controller
public class UserController {
  private SessionManager sessionManager;

  public UserController(SessionManager sessionManager) {
    this.sessionManager = sessionManager;
  }

  // Methods that use the session manager
}
```

The `UserController` now has a dependency on some implementation of
`SessionManager`. The code that instantiates the `UserController` class now must
"inject" an instance of `SessionManager` into the `UserController`. This
prevents the violation of the SRP by moving the decision of what kind of
session manager to create to the part of the application that hooks together
everything which, ostensibly, is that portion of the software's responsibility.

### A Side-Effect: Testing a "Stateful" Controller

Now that we have moved the decision of what kind of `SessionManager` to use
outside of the `UserController` class, we can now test the `UserController`
with any kind of `SessionManager` we have available to us.

More importantly, we can create something like `TestSessionManagerImpl` which
implements the `SessionManager` interface and use that test session manager to
ensure that the `UserController` invokes its methods in a way that we expect it
to invoke them. The `TestSessionManagerImpl` class doesn't need a network or a
database or a file system to pretend to supply the functionality desired by the
`UserController`. Testing based on DI gives us the flexibility to run our tests
"in isolation", that is, without the need to access any resources beyond the
current process.

## The Rise of the DI Container

With developers adopting this new style of programming, it became obvious that
most applications would have to have code to "wire up" the application similar
the following code snippet.

```java
// Create the session manager
OracleJdbcConnection cnx = new OracleJdbcConnection("localhost:sessions");
OracleSessionManagerAdapter adapter = new OracleSessionManagerAdapter(cnx);
SessionManager manager = new DatabaseSessionManagerImpl(adapter);

// Create controller instances
UserController userController = new UserController(manager);
ShoppingCartController cartController = new ShoppingCartController(manager);
CatalogController catalogController = new CatalogController(manager);
```

Manually creating all of the instances of dependencies and providing them to the
`new` statements for objects that depend on the instances was going to prove
antithetical to the developer's natural instinct that the less typed is better.

Realizing that all of this could be managed through metadata about the classes
and that the metadata already existed in the runtime type information, some
smart developers got together and decided to build a library that acted like a
factory, one of the Gnag of Four creational patterns. This library would figure
out the dependency hierarchy and satisfy it when it was asked for the instance
of a specific class or name.

This changes the code above to, conceptually, something like this.

```java
// Create the DI container
DIContainer container = new DIContainer();

// Register the depenedencies by instance and class
contianer.RegisterInstance(new OracleJdbcConnection("localhost:sessions"));
container.Register(OracleSessionManagerAdapter.class);
container.Register(SessionManager.class, DatabaseSessionManagerImpl.class);

// Create controller instances
UserController controller = (UserController) container.Resolve(UserController.class);
```

A really nice benefit of this comes from the automated dependency satisfaction
provided by the DI container. If the `UserController` constructor changes to
need a new dependency, then all we have to do is register something that
fulfills that depenency with the DI cotainer. The following code snippet shows
an example of that.

```java
@Controller
public class UserController {
  private SessionManager sessionManager;
  private Logger logger;

  public UserController(SessionManager sessionManager, Logger logger) {
    this.sessionManager = sessionManager;
    this.logger = logger;
  }

  // Methods that use the session manager
}
```

And, in the DI registration:

```java
// Create the DI container
DIContainer container = new DIContainer();

// Register the depenedencies by instance and class
contianer.RegisterInstance(new OracleJdbcConnection("localhost:sessions"));
container.Register(OracleSessionManagerAdapter.class);
container.Register(SessionManager.class, DatabaseSessionManagerImpl.class);
container.Register(Logger.class, new FileLogger("/var/log/app.log"));

// Create controller instances
UserController controller = (UserController) container.Resolve(UserController.class);
```
And, thats how and why of DI.

[callout-cool]
**You**: Hey, Alberto, I have a question. \
**Alberto**: What's up? \
**You**: I wanted to know about nested states in ui-router.
[/callout-cool]
