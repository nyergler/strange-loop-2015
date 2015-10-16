# Building Isomorphic Web Apps with React

http://github.com/elyseko/iso-react-demo
@sfdrummerjs
drummerjs.com

Web team lead @ Vevo

Isomorphic generally means things have equal form. But with javascript
environments, we can't get all code to run in all places, but you can
get close. And that's what we're talking about.

Why?

* SEO Crawlers

  Vevo _needs_ to be crawled, and it's important to have good
  SEO. Previously used a service that took page snapshots, dumped them
  in S3 buckets, and served them to crawlers. Incredibly brittle.

* Performance

  If you do it right, the browser only has to do a paint to get to the
  perceived "loaded" state.

* Developer benefits

  Code re-use [really?]

[ Syntax Overview ]

Non-React Isomorphic Stuff

* Asana [ugh]
* Derby MVC Framework
* Meteor

React Isomorphic frameworks

* Pellet (Vevo)
* React Nexus
* Relay (Facebook)

React makes isomorphic easier, because it relies on the virtual DOM,
and not the entire browser stack. And because views are functional,
it's easy to run in different contexts (dependencies are explicit).

Folder structure separates server and client entry points, and
potentially routes (since the react routing is [...]). But most of the
code is in the shared directory.

Webpack lets you have a build for each environment, and include
environment specific code. It has a ton of power that you can also
leverage -- "feature flags" via environment variables, chunking for
download, etc.

React Router

In the client code, createElement hook lets you populate data and
inject shared routes.

Declaring data needs

Need a way for a component to say what data it needs. [Presumably
because on client it's HTTP driven, on Node.js it's from a function or
something.]

In this example it's a static method on the component.

Rehydrating Data

Can either hold onto the data and pop it in as props, or rehydrate
your Cache in the browser (in which case your API calls "run", but
check the cache first, and return synchronously).

Cons of building isomorphic

* renderToString performance blows on the server; not optimized at
  all.

* Lots of complexity with rehydration; if your whole app is behind a
  login, consider carefully if your performance requirements mean you
  need isomorphic.


Future

* Relay
* Virtual DOM showing up elsewhere (ie, Ember)
* Meteor now works with React
