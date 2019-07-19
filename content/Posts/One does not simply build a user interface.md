---
title: One does not simply build a user interface, our ClojureScript/re-frame app
type: docs
Date: 2014-12-04
author: unbalancedparentheses
---
[Part I of this article](https://notamonadtutorial.com/holiday-ping-how-we-implemented-our-first-open-source-app-with-erlang-and-clojurescript-fad5b66fc325) discussed the motivation to build [HolidayPing](http://holidayping.lambdaclass.com/) and the design and development process of the back end Erlang application. In this follow-up Facundo (the main dev) focuses on the user interface and what we learnt building it.
****
Reach me via twitter at [@unbalancedparen](http://twitter.com/unbalancedparen) if you have any comments or interview request for [This is not a Monad tutorial](https://medium.com/this-is-not-a-monad-tutorial/). Stay tuned!

_Discuss and vote at [lobsters](https://lobste.rs/s/zmqo84/interview_with_brad_chamberlain_about), [reddit](https://www.reddit.com/r/programming/comments/7x2jhp/interview_with_brad_chamberlain_about_a/) and [hn](https://news.ycombinator.com/item?id=16360381)._
*******

![](https://cdn-images-1.medium.com/max/800/1*BdFV-bGWhQ5HiIL6VfY5xg.png)


Before getting into the details, a couple of disclaimers are in order:
1. Me and most of my coworkers have done front end over the years, but we’re all primarily back end developers. Particularly, the last time I worked on a Single-Page Application, Angular 1 was king; I vaguely knew about React ideas, but never came close to using it.
2. Although we threw in a wizard and a fancy yearly calendar view, the application was your typical CRUD. Exactly the kind of thing for which [you usually don’t want to build a Single-Page application](https://medium.freecodecamp.org/why-i-hate-your-single-page-app-f08bb4ff9134). We knew this and we choose to do a SPA anyway because this wasn’t a paid project so we weren’t optimizing for resource efficiency but for learning.

## Language: JavaScript vs ClojureScript vs Elm

The first big decision was the programming language. JavaScript is the default option, but even with a solid knowledge of the language and coming out of a couple of years with Node.js as my daily work platform, becoming a productive front end JavaScript developer in 2017 is a notable feat. The tooling and even the language keeps moving under your feet. For engineers that only get out of the server from time to time, it all feels like throw-away knowledge (just like my previous Angular experience is of little use today). And if you want to do fashionable JavaScript, you end up using transpilers anyway, so why not just use a different language altogether? I mean, we’re using Erlang already, it’s not like we mind throwing some weirdness into the mix.

Elm has always been a very tempting choice for us, but learning an entire language (and one so different from the others I already know) was too much to take on for a side project whose goal was to get familiar with Erlang.

And then there was ClojureScript. I was already fluent in Clojure, had some experience with ClojureScript, my Emacs was already prepared to move parentheses around… [And it delivered its promises](https://www.youtube.com/watch?v=gsffg5xxFQI). _lein new re-frame holiday-ping_ is all it took to setup a workflow with a live REPL, hot code reloading (thanks primarily to [lein-figwheel](https://github.com/bhauman/lein-figwheel/)) and advanced JavaScript compilation. This is not only simpler than all the disparate tools you need to do the same job in JavaScript, but also requires a smaller effort than [setting up a good workflow in JVM Clojure](http://thinkrelevance.com/blog/2013/06/04/clojure-workflow-reloaded).

## Om vs Reagent vs Om.next vs re-frame

Next up was the library or framework to build the UI. We could have gone with a simple DOM manipulation library like [dommy](https://github.com/plumatic/dommy), but we decided — at our own risk — to build a Single-Page App, therefore we looked at React wrappers: [Reagent](http://reagent-project.github.io/) and [Om](https://github.com/omcljs/om). We couldn’t afford to build prototypes with both tools so we have to settle for reviewing the documentation and examples, and getting opinions from friends and the web. As Gandalf once said: _all engineers have to do is to pick a library with the time that is given to them._

Reagent right away seemed more idiomatic Clojure, relying mostly on plain data and functions (and, in the case of re-frame, _pure_ functions) while Om used a lot of objects and protocols, _reify_ and JavaScript interop. Reagent also used the lovely hiccup syntax to describe HTML as data, while Om leaned on functions by default (although there are [libraries to switch to hiccup](https://github.com/r0man/sablono). Lastly, Om seemed to require a better knowledge of the React lifecycle, which added another layer to get familiar with. A quick comparison of [TodoMVC](http://todomvc.com/) code illustrates why my gut feeling was to go with [Reagent](https://github.com/reagent-project/reagent/blob/master/examples/todomvc/src/todomvc/core.cljs#L62-L75) rather than [Om](https://github.com/swannodette/todomvc/blob/gh-pages/labs/architecture-examples/om/src/todomvc/item.cljs#L47-L83).

So I leaned more towards Reagent than Om. But those only provide React wrappers; the real choice had to be between [Om.next](https://github.com/omcljs/om/wiki/Quick-Start-%28om.next%29) and [re-frame](https://github.com/Day8/re-frame), otherwise I’d had have to come up with an architecture myself, and I didn’t have the background to do it effectively. I caught a great explanation of this by Mike Thompson (author of re-frame) in the Clojurians Slack:

>_You can absolutely use Reagent by itself if your application is simple enough. BUT if you just use Reagent by itself then you only have the V bit of an application. As your application starts to get more complicated, you * will * start to create an architecture which adds_ control logic _and_ state management — _the M and C parts (even if you don’t think you are, you are). So then the question becomes: is your architecture better than re-frame’s or not? And some people prefer their own architectures and would answer “yes” :-) Fair enough._
>
>_I think the only danger arises if this process is not conscious — if someone creates a dog’s breakfast of an architecture and doesn’t even know they’ve done it. I’ve had MANY people privately admit that’s what happened to them… and then they swapped to re-frame to get some structure back. So, my advice is… if your application is a little more complicated, be sure to make a conscious choice around architecture, because one way or another you’ll be using one._

## CSS framework

For styles we went with [Bulma](https://bulma.io/): we wanted something lighter than Bootstrap, specifically no JavaScript. Bulma is simple, easy to use, feels lightweight and looks good. In combination with hiccup it meant I was pulling off a beautiful UI without HTML, CSS or JavaScript; all my components were conveniently built by moving Clojure data around Emacs.

Perhaps the one downside was that there aren’t many pre-baked fancy components like Bootstrap has. We kind of made that choice anyway by using ClojureScript in the first place; when we wanted a complex component we either coded it ourselves (we did with tag list inputs) or we avoided them (e.g. no type-ahead selects).

## Developing a Single-Page App with ClojureScript and re-frame

I just needed to read the first [few re-frame tutorials](https://github.com/Day8/re-frame/tree/master/docs) to get started. It felt like re-frame built on the concepts I was already familiar from Clojure and I became reasonably productive in a matter of days, which was amazing considering I hardly knew the first thing about React. [This guide](https://purelyfunctional.tv/guide/re-frame-building-blocks/) provides a good overview of the framework.

As mentioned, the workflow is amazing: you write pure functions, describing HTML components as Clojure data literals, and figwheel auto reloads them; if you want to play around with a dependency or interop with JavaScript and the browser APIs, just switch to the tab where the REPL is running. Instant gratification all the way.

At first, the re-frame structure can be a little overwhelming: you have events, subscriptions, effects, co-effects… Specially it took me a while to find the justification for subscription handlers, since most of the times they were trivial reads of the database. I think it helped when I read enough times that a design goal of re-frame was to keep views as dumb and decoupled from state as possible; under that light all the pieces started to fall into place: re-frame handles the dirty work and side-effects, you just declare what needs to be done through functions that return data (with the bonus that your code becomes easier to test).

The one thing I don’t quite like about re-frame is how all definitions (reg-event, reg-sub, reg-fx, reg-cofx) set up some hidden application state. I’d prefer to just have standalone functions and pass a map or some other data structure into a single re-frame initialization step. As it is, it forces you to require namespaces just for the side effects they produce, which doesn’t feel very idiomatic.

While re-frame is a very opinionated framework and provides a lot structure, there still are some design aspects left to the programmer. Following are some notes of decisions I had to make; a lot of it I figured out along the way and there may be better ways to do it. Critiques and suggestions are certainly welcome.

## Routing and navigation

Perhaps the most complex part of the front end application was properly handling navigation. It’s also the place where you pay the penalty of using a SPA framework to build an application that requires browser-like logic (i.e. multiple multiple entry points based on the url, working back/forward buttons, etc.).

Initially it was simple, I just followed [the re-frame documentation](https://github.com/Day8/re-frame/blob/master/docs/Navigation.md): you add an _:active-panel_ value to your application state, update it when the user produces a navigation event (e.g. select a tab) and make your main view show the proper component based on the _:active-panel_ current value.

>(def panels {:panel1 [panel1]
>    :panel2 [panel2]})
>
>(defn high-level-view 
  []
  (let [active (re-frame/subscribe [:active-panel])]
    (fn []
      [:div
       [:div.title   "Heading"]
       (get panels @active)])))

This was enough to get me going, but my first concern was how to fetch different data from the server based on the selected panel (i.e. when I’m on the channel list, GET /api/channels; when I’m on the calendar for channel _my-channel_, GET /api/channels/my-channel/holidays; etc.). The first idea that came to mind was to just trigger a request event inside the component view function, but this was directly against the philosophy of keeping logic out of the views.

Asking in the re-frame Slack channel, I got the suggestion of just fetching all the required data when the user enters the application, load it in the in-memory app-db and use that as the source of truth instead of synchronizing with the server on every step. I wasn’t entirely convinced by this, especially as the application grew bigger, but it was better than polluting view functions with logic and, again, enough to move forward.

Eventually, though, the API reached a point where this approach was impractical and inefficient: we had multiple endpoints which more or less mapped to different views in the UI, we couldn’t just load all the data to cover every possible navigation path. Moreover, with very distinct views it was time to bring URLs and the expected browser behavior: proper routing, interacting with the history API. I turned to [bidi](https://github.com/juxt/bidi) and [pushy](https://github.com/kibu-australia/pushy), based on a [tutorial by J. Pablo Fernández](https://pupeno.com/2015/08/26/no-hashes-bidirectional-routing-in-re-frame-with-bidi-and-pushy/).

Now we needed some way to specify what data was to be fetch for each route, without cluttering the views; I came up with a generic event handler, using multimethods:

>(re-frame/reg-event-fx
 :navigate
 (fn [{:keys [db]} [_ new-view]]
   {;; trigger the load-view event in case data from the server is required
    :dispatch    [:load-view new-view] 
    ;; update the browser history API and switch to the given view
    :set-history new-view
    ;; set the current view in the app-db so the dom is updated
    :db          (assoc db
                        :loading-view? true
                        :current-view new-view)}))
>
>(defmulti load-view
  "Create a multimethod that will implement different event handlers based on the
   view keyword."
  (fn [cofx [view]] view))
>
>(re-frame/reg-event-fx
 :load-view
 (fn [cofx [_ new-view]]
   ;; delegate event handling to the proper multimethod
   (load-view cofx [vector new-view])))
>
>(defmethod load-view 
  :default
  [{:keys [db]} _]
  ;; by default don't load anything
  {:db (assoc db :loading-view? false)})
>
>(defmethod load-view
  :channel-list
  [_ _]
  ;; when navigating to the channel list, fetch the channels
  {:http-xhrio {:method     :get
                :uri        "/api/channels"
                :on-success [:channel-list-success]}})

Even though this felt a bit hacky, seeing the view functions become cleaner somewhat validated the approach. For such an opinionated framework, though, I wished there was a recommended way to handle this use case, which is probably a frequent one (maybe there is and I just didn’t find it).

## Forms and validations

There must be few tasks as repetitive as writing forms in a web application, specially if there are many CRUD components. After writing a couple of them, I obviously started looking for ways to build a reusable component that would create them based on a specification. I managed to do so, again by resorting to multimethods, such that each method would render differently based on the input type (text, password, select, etc.); the API ended up looking like:

>[forms/form-view {:submit-text "Register"
                  :on-submit   [:register-submit]
                  :fields      [{:key      :email
                                 :type     "email"
                                 :validate :valid-email?
                                 :required true}
                                {:key      :password
                                 :type     "password"
                                 :required true}
                                {:key      :password-repeat
                                 :type     "password"
                                 :label    "Repeat password"
                                 :validate :matching-passwords?
                                 :required true}]}]

The form-view maintains a local state atom, and when the submit button is clicked, the :on-submit event is triggered passing the current input values.

It took me a couple of iterations to properly integrate these from components with the rest of the app, specially when I needed to pre-populate them with data coming from other views or from back end responses. This was the one area where I had to get a bit more familiar with React internals, for example learning about [controlled and uncontrolled components](https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/). After introducing the load-view handler described in the previous section this kind of issues went away.

Finally, I needed a way to introduce reusable validations to the form component, without breaking the “no logic in views” rule. I found that encapsulating validation in subscription handlers worked really well. I could specify validations as part of the form specification, subscribe to them to change the style of erroneous inputs and disable submit buttons, and also use the same validations outside of the forms:

>(re-frame/reg-sub
 :valid-required?
 (fn [db [_ value]]
   (if (string/blank? value)
     [false "This field is required."]
     [true])))
>
>(re-frame/reg-sub
 :valid-form?
 (fn [[_ form fields]]
   (->> fields
        (get-validation-subs form)
        (map re-frame/subscribe)
        (doall)))
>
>(fn [validations _]
   (every? true? (map first validations))))

## Closing thoughts

This was a modest project and as such didn’t reveal any higher truth. Instead, it reinforced some ideas and reminded us about things we tend to forget. I liked that it gave us a better perspective than what we’re used to, working mainly as back end engineers: this time we had to write a front end to our APIs and be a user to our interfaces.

A good design should model the domain of the problem it is trying to solve, rather than accommodate to immediate client needs. But as engineers we have to expect the user experience to reveal aspects of the domain that we missed, were unforeseeable or plainly didn’t exist at design stage. You need a UI to be able to poke around with the application to see what feels right and what doesn’t, and change the model accordingly. In our case, as soon as we started to use the front end, we realized that the relationships between our entities had to be turned around. What was more interesting: once we made those changes the code became simpler in ways we hadn’t considered.

This is no news, but front end takes a lot of work. We were aiming for a very simple UI, one that covered a fraction of the application surface, and it still required more work and way more trial and error than the back end. The dumbest front end application takes a non trivial amount of work to get right and provide an adequate user experience; traditional server-side applications, on the other hand, are becoming easier and easier to put together, to the point that they are arguably becoming extinct.
