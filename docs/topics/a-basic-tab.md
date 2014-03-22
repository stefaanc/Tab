<a name="top" ></a>

<img src="../img/tab-logo128.png" alt="Tab logo" align="left" style="float:left; margin-top:-22px;" height="66" /><img src="../img/1x1.png" align="left" style="float:left;" height="44" width="20" />
## [A Basic Tap][topic-a-basic-tab]

````javascript
var id, interval = 0,
    counter = new Tab();

id = setInterval(function () {
    try {
        interval += 1;
        counter.return(interval);
    }
    catch (e) {
        counter.throw(e);
    }  
}, 1000);

counter
.try(function (value) {
    if (value <= 3600) {
        console.log(value % 2 === 1 ? "tick" : "tock"); 
    }
    if (value === 3600) {
        clearInterval(id);
        console.log("cuckoo");
    }
})
.catch(function (error) {
    console.log(error);
});
````

In this example:

* [new Tab()][ref-new-tab] creates a new Tab object.    Alternatively, we can also use [Tab.construct()][ref-tab.construct].
* [counter.return()][ref-tab.prototype.return] updates `counter` with a new value and notifies all its subscribers. 
* [counter.throw()][ref-tab.prototype.throw] sets `counter` to the failed state (this is probably not going to be necessary in this very simple example) and notifies all its subscribers.
* [.try( onReturnedProcessor )][ref-tab.prototype.try] subscribes to all notifications from `counter` and creates a new Tab object for further processing.  When an 'returned' notification is received, the processor function is executed.  When a 'thrown' notification is received, the notification is passed on to all subscribers of the new tab created by this method.   
* [.catch( onThrownProcessor )][ref-tab.prototype.catch] subscribes to all notifications from the tab created by `.try()`.  When a 'throw' notification is received, the processor function is executed.

If we receive an object from some other piece of code, and we don't know if that object is a Tab object or not then we may want to test if it is a Tab.

````javascript
function log(object) {
    if (Tab.isTab(object)) {
        object
        .try(function (value) {
            console.log("object = " + JSON.stringify(value));
        })
        .catch(function (error) {
            console.log("error for 'object' = " + JSON.stringify(error));
        });
    }
    else {
        console.log("object = " + JSON.stringify(object));
    }
}
````

In this example:

* [Tab.isTab()][ref-tab.is-tab] returns `true` when `object` was created with the `new Tab()` constructor.

Alternatively, we can also make our life easier by converting the object to a tab.

````javascript
function log(object) {
    Tab(object)
    .try(function (value) {
        console.log("object = " + JSON.stringify(value));
    })
    .catch(function (error) {
        console.log("error for 'object' = " + JSON.stringify(error));
    });
}
````

In this example:

* [Tab()][ref-tab] returns the original `object` if it is a Tab object, otherwise a new tab is created and its value is set to `object`.  Alternatively, we can also use [Tab.convert()][ref-tab.convert].

However, we can even do better in this very simple example.

````javascript
function log(object) {
    try {
        console.log("object = " + JSON.stringify(object.valueOf()));
    }
    catch (error) {
        console.log("error for 'object' = " + JSON.stringify(error));
    }
}
````

In this example:

* [object.valueOf()][ref-tab.prototype.value-of] returns the value of `object`, no matter if it's a Tab object or not, otherwise if `object` is a Tab object in the failed state then its error is thrown.

There are a lot of javascript methods that will implicitly call this `.valueOf()` method.  Try the following.

````javascript
var i = Tab.newReturn(1);

log(i + 1); // > 2
````

In this example:

* [Tab.newReturn( 1 )][ref-tab.new-return] returns a new Tab object and is getting the value 1.  This is a *sugar* method for `Tab.construct().return(1)`.
* when `i` is accessed to add 1, `.valueOf()` is applied by the addition operator.  This is true for some javascript methods, but not for all.  For instance `console.log` will log the object without applying the `.valueOf()` method.  So when in doubt... and you don't have time to read (and understand) the ECMA specification, and you don't have time to experiment with lots of different browsers, then it's probably best to apply it explicitly.

Beware that the following will fail.

````javascript
var i = Tab.newReturn(1);

log(++i); // throws
````

Supporting this would need the *Proxy* object that is on the program for *ES.next*.  We may try to implement some experimental version at some point in time.  So watch this space.

<br />

---
### Methods introduced in this topic (and some related methods)

* [new Tab()][ref-new-tab]  
* [Tab()][ref-tab]  
<br />
* [Tab.construct()][ref-tab.construct]  
* [Tab.convert()][ref-tab.convert]  
* [Tab.isTab()][ref-tab.is-tab]  
* [Tab.newReturn()][ref-tab.new-return]  
* [Tab.newThrow()][ref-tab.new-throw]  
<br />
* [.catch()][ref-tab.prototype.catch]  
* [.finally()][ref-tab.prototype.finally]  
* [.hasReturned()][ref-tab.prototype.has-returned]  
* [.hasThrown()][ref-tab.prototype.has-thrown]  
* [.return()][ref-tab.prototype.return]  
* [.throw()][ref-tab.prototype.throw]  
* [.toString()][ref-tab.prototype.to-string]  
* [.try()][ref-tab.prototype.try]  
* [.valueOf()][ref-tab.prototype.value-of]  



<br /> Back to [Top] | [Project] | [Topics] / [The Basics][topic-the-basics] | [Reference] <br />
<-- ##### start of links ##### --> 

[top]:       #top                        "back to the top of this page."
[project]:   /doc/project.md#the-project "back to the 'Project' section."
[topics]:    /doc/topics.md#topics       "back to the 'Topics' section."
[reference]: /doc/reference.md#reference "back to the 'Reference' section."



[topic-the-basics]:                              /doc/topics.md#the-basics                                  "more topics under 'The Basics'"
[topic-where-are-tabs-helping]:                  /doc/topics.md#where-are-tabs-helping                      "more topics under 'Where Are Tabs Helping?'"
[topic-where-are-tabs-lacking]:                  /doc/topics.md#where-are-tabs-lacking                      "more topics under 'Where Are Tabs Lacking?'"
[topic-advanced-topics]:                         /doc/topics.md#advanced-topics                             "more topics under 'Advanced Topics'"

[topic-a-basic-tab]:                             /doc/topics/a-basic-tab.md#top                             "A Basic Tab: creating and using a basic Tab object."
[topic-a-basic-callback]:                        /doc/topics/a-basic-callback.md#top                        "A Basic Callback: using a Tab object to handle callbacks."
[topic-a-basic-pipeline]:                        /doc/topics/a-basic-pipeline.md#top                        "A Basic Pipeline: using Tab objects for pipelining."
[topic-a-basic-stream]:                          /doc/topics/a-basic-stream.md#top                          "A Basic Stream: using a Tab object to work with sequence of data elements."
[topic-a-basic-promise]:                         /doc/topics/a-basic-promise.md#top                         "A Basic Promise: using a Tab object as a promise."
[topic-basic-lazy-evaluation]:                   /doc/topics/basic-lazy-evaluation.md#top                   "Basic Lazy Evaluation: using a Tab object for lazy evaluation."
[topic-basic-concurrent-computing]:              /doc/topics/basic-concurrent-computing.md#top              "Basic Concurrent Computing: using a Tab object to handle concurrent computing."

[topic-clean-function-signatures]:               /doc/topics/clean-function-signatures.md#top               "Clean Function Signatures: using function signatures without callbacks."
[topic-shallow-callback-nesting]:                /doc/topics/shallow-callback-nesting.md#top                "Shallow Callback Nesting: avoiding deeply nested functions."
[topic-top-down-control-flow]:                   /doc/topics/top-down-control-flow.md#top                   "Top-Down Control Flow: turning inverted control-flow back around."
[topic-predictable-execution-order]:             /doc/topics/predictable-execution-order.md#top             "Predictable Execution Order: avoiding issues with immediate callbacks."
[topic-modular-decomposition]:                   /doc/topics/modular-decomposition.md#top                   "Modular Decomposition:  ..."
[topic-aspect-oriented-programming]:             /doc/topics/aspect-oriented-programming.md#top             "Aspect Oriented Programming: ..."
[topic-exception-style-error-propagation]:       /doc/topics/exception-style-error-propagation.md#top       "Exception Style Error Propagation: letting errors propagate through a sequence of processing tabs."

[topic-debugging-asynchronous-events]:           /doc/topics/debugging-asynchronous-events.md#top           "Debugging Asynchronous Events: ..."

[topic-scheduling]:                              /doc/topics/scheduling.md#top                              "Scheduling: ..."
[topic-streaming-caching-queuing-collecting]:    /doc/topics/streaming-caching-queuing-collecting.md#top    "Streaming, Caching, Queuing, Collecting: ..."
[topic-extending-tab]:                           /doc/topics/extending-tab.md#top                           "Extending Tab: ..."



[ref-tab-object]:                    /doc/reference.md#tab-object                        "more attributes and methods under 'Tab Object'"
[ref-tab-constructor]:               /doc/reference.md#tab-constructor                   "more attributes and methods under 'Tab Constructor'"
[ref-tab-constructor-attributes]:    /doc/reference.md#tab-constructor-attributes        "more attributes under 'Tab Constructor Attributes'"
[ref-tab-constructor-methods]:       /doc/reference.md#tab-constructor-methods           "more methods under 'Tab Constructor Methods'"
[ref-tab-prototype-methods]:         /doc/reference.md#tab-prototype-methods             "more methods under 'Tab Prototype Methods'"
[ref-tab-instance-methods]:          /doc/reference.md#tab-instance-methods              "more methods under 'Tab Instance Methods'"
[ref-tab.schedulers-object]:         /doc/reference.md#tabschedulers-object              "more attributes and methods under 'Tab.Schedulers Object'"
[ref-tab.schedulers-attributes]:     /doc/reference.md#tabschedulers-attributes          "more attributes and methods under 'Tab.Schedulers Attributes'"
[ref-tab.schedulers-methods]:        /doc/reference.md#tabschedulers-methods             "more attributes and methods under 'Tab.Schedulers Methods'"
[ref-tab.x-object]:                  /doc/reference.md#tabx-object                       "more attributes and methods under 'Tab.X Object'"
[ref-tab.x-attributes]:              /doc/reference.md#tabx-attributes                   "more attributes and methods under 'Tab.X Attributes'"
[ref-tab.x-methods]:                 /doc/reference.md#tabx-methods                      "more attributes and methods under 'Tab.X Methods'"

[ref-new-tab]:                       /doc/reference/new-tab.md#top                       "new Tab(): construct a new tab, encapsulate a given tab if requested."
[ref-tab]:                           /doc/reference/tab.md#top                           "Tab(): convert to a tab, create a new tab if required."

[ref-tab.context]:                   /doc/reference/tab.context.md#top                   "Tab.context: the processing context for a processor function."
[ref-tab.version]:                   /doc/reference/tab.version.md#top                   "Tab.version: the version of this Tab library."

[ref-tab.construct]:                 /doc/reference/tab.construct.md#top                 "Tab.construct(): construct a new tab, encapsulate a given tab if requested."
[ref-tab.convert]:                   /doc/reference/tab.convert.md#top                   "Tab.convert(): convert to a tab, create a new tab if required."
[ref-tab.defer]:                     /doc/reference/tab.defer.md#top                     "Tab.defer(): create a function that uses a given tab to store another function's result."
[ref-tab.defer-fulfill]:             /doc/reference/tab.defer-fulfill.md#top             "Tab.deferFulfill(): create a function that updates the value of a given tab and silently blocks any further updates."
[ref-tab.defer-reject]:              /doc/reference/tab.defer-reject.md#top              "Tab.deferReject(): create a function that puts a given tab in the failed state and silently blocks any further updates."
[ref-tab.defer-return]:              /doc/reference/tab.defer-return.md#top              "Tab.deferReturn(): create a function that updates the value of a given tab."
[ref-tab.defer-settle]:              /doc/reference/tab.defer-settle.md#top              "Tab.deferSettle(): create a function that silently blocks any further updates for a given tab."
[ref-tab.defer-throw]:               /doc/reference/tab.defer-throw.md#top               "Tab.deferThrow(): create a function that puts a given tab in the failed state."
[ref-tab.defer-with]:                /doc/reference/tab.defer.md#top                     "Tab.deferWith(): create a function that uses a given tab to store another function's result, using the new function's subject as a first argument for the other function."
[ref-tab.is-tab]:                    /doc/reference/tab.is-tab.md#top                    "Tab.isTab(): was the given object created by this Tab constructor?"
[ref-tab.new-fulfill]:               /doc/reference/tab.new-fulfill.md#top               "Tab.newFulfill(): create a new tab that is initialized with a given value and silently block any further updates."
[ref-tab.new-reject]:                /doc/reference/tab.new-reject.md#top                "Tab.newReject(): create a new tab that is put in the failed state and silently block any further updates."
[ref-tab.new-return]:                /doc/reference/tab.new-return.md#top                "Tab.newReturn(): create a new tab that is initialized with a given value."
[ref-tab.new-settle]:                /doc/reference/tab.new-settle.md#top                "Tab.newSettle(): create a new tab and silently block any further updates."
[ref-tab.new-throw]:                 /doc/reference/tab.new-throw.md#top                 "Tab.newThrow(): create a new tab that is put in the failed state."

[ref-tab.prototype.cancel]:          /doc/reference/tab.prototype.cancel.md#top          "Tab.prototype.cancel(): cancel all subscriptions and all scheduled processors for this tab."
[ref-tab.prototype.do-return]:       /doc/reference/tab.prototype.do-return.md#top       "Tab.prototype.doReturn(): update the value of this tab (for ES3 environments)."
[ref-tab.prototype.do-throw]:        /doc/reference/tab.prototype.do-throw.md#top        "Tab.prototype.doThrow(): put this tab in the failed state (for ES3 environments)."
[ref-tab.prototype.fulfill]:         /doc/reference/tab.prototype.fulfill.md#top         "Tab.prototype.fulfill(): update the value of this tab and silently block any further updates."
[ref-tab.prototype.has-returned]:    /doc/reference/tab.prototype.has-returned.md#top    "Tab.prototype.hasReturned(): has this tab returned a value?"
[ref-tab.prototype.has-thrown]:      /doc/reference/tab.prototype.has-thrown.md#top      "Tab.prototype.hasThrown(): has this tab thrown an error?"
[ref-tab.prototype.is-cancelled]:    /doc/reference/tab.prototype.is-cancelled.md#top    "Tab.prototype.isCancelled(): are all subscriptions for this tab cancelled?"
[ref-tab.prototype.is-settled]:      /doc/reference/tab.prototype.is-settled.md#top      "Tab.prototype.isSettled(): are any further updates for this tab blocked?"
[ref-tab.prototype.on-cancelled]:    /doc/reference/tab.prototype.on-cancelled.md#top    "Tab.prototype.onCancelled(): execute a processor when this tab is cancelled."
[ref-tab.prototype.on-returned]:     /doc/reference/tab.prototype.on-returned.md#top     "Tab.prototype.onReturned(): execute a processor when this tab has returned a value."
[ref-tab.prototype.on-settled]:      /doc/reference/tab.prototype.on-settled.md#top      "Tab.prototype.onSettled(): execute a processor when this tab is settled."
[ref-tab.prototype.on-thrown]:       /doc/reference/tab.prototype.on-thrown.md#top       "Tab.prototype.onThrown(): execute a processor when this tab has thrown an error."
[ref-tab.prototype.reject]:          /doc/reference/tab.prototype.reject.md#top          "Tab.prototype.reject(): put this tab in the failed state and silently block any further updates."
[ref-tab.prototype.return]:          /doc/reference/tab.prototype.return.md#top          "Tab.prototype.return(): update the value of this tab."
[ref-tab.prototype.settle]:          /doc/reference/tab.prototype.settle.md#top          "Tab.prototype.settle(): silently block any further updates for this tab."
[ref-tab.prototype.throw]:           /doc/reference/tab.prototype.throw.md#top           "Tab.prototype.throw(): put this tab in the failed state."
[ref-tab.prototype.to-string]:       /doc/reference/tab.prototype.to-string.md#top       "Tab.prototype.toString(): get a string representation for this tab."
[ref-tab.prototype.value-of]:        /doc/reference/tab.prototype.value-of.md#top        "Tab.prototype.valueOf(): get the principal value of this tab."

[ref-tab.schedulers.tick]:           /doc/reference/tab.schedulers.tick.md#top           "Tab.Schedulers.es5: a sequence number incremented in every turn"

[ref-tab.schedulers.schedule-first]: /doc/reference/tab.schedulers.schedule-first.md#top "Tab.Schedulers.defer(): schedule a given callback before all other callbacks in the next turn."
[ref-tab.schedulers.schedule-last]:  /doc/reference/tab.schedulers.schedule-last.md#top  "Tab.Schedulers.defer(): schedule a given callback after all other callbacks."
[ref-tab.schedulers.schedule-next]:  /doc/reference/tab.schedulers.schedule-next.md#top  "Tab.Schedulers.notify(): schedule a given callback in the next turn."
[ref-tab.schedulers.schedule-now]:   /doc/reference/tab.schedulers.schedule-now.md#top   "Tab.Schedulers.subscribe(): call the callback immediately."

[ref-tab.x.es5]:                     /doc/reference/tab.x.es5.md#top                     "Tab.X.es5: is this library running in an ES5 environment?"

[ref-tab.x.defer]:                   /doc/reference/tab.x.defer.md#top                   "Tab.X.defer(): the basic method to create deferred functions."
[ref-tab.x.notify]:                  /doc/reference/tab.x.notify.md#top                  "Tab.X.notify(): the basic method to send notifications for a tab."
[ref-tab.x.subscribe]:               /doc/reference/tab.x.subscribe.md#top               "Tab.X.subscribe(): the basic method to subscribe to notifications from a tab."
<-- ##### start of links ##### --> 

[top]:       #top                        "back to the top of this page."
[project]:   /doc/project.md#the-project "back to the 'Project' section."
[topics]:    /doc/topics.md#topics       "back to the 'Topics' section."
[reference]: /doc/reference.md#reference "back to the 'Reference' section."



[topic-the-basics]:                              /doc/topics.md#the-basics                                  "more topics under 'The Basics'"
[topic-where-are-tabs-helping]:                  /doc/topics.md#where-are-tabs-helping                      "more topics under 'Where Are Tabs Helping?'"
[topic-where-are-tabs-lacking]:                  /doc/topics.md#where-are-tabs-lacking                      "more topics under 'Where Are Tabs Lacking?'"
[topic-advanced-topics]:                         /doc/topics.md#advanced-topics                             "more topics under 'Advanced Topics'"

[topic-a-basic-tab]:                             /doc/topics/a-basic-tab.md#top                             "A Basic Tab: creating and using a basic Tab object."
[topic-a-basic-callback]:                        /doc/topics/a-basic-callback.md#top                        "A Basic Callback: using a Tab object to handle callbacks."
[topic-a-basic-pipeline]:                        /doc/topics/a-basic-pipeline.md#top                        "A Basic Pipeline: using Tab objects for pipelining."
[topic-a-basic-stream]:                          /doc/topics/a-basic-stream.md#top                          "A Basic Stream: using a Tab object to work with sequence of data elements."
[topic-a-basic-promise]:                         /doc/topics/a-basic-promise.md#top                         "A Basic Promise: using a Tab object as a promise."
[topic-basic-lazy-evaluation]:                   /doc/topics/basic-lazy-evaluation.md#top                   "Basic Lazy Evaluation: using a Tab object for lazy evaluation."
[topic-basic-concurrent-computing]:              /doc/topics/basic-concurrent-computing.md#top              "Basic Concurrent Computing: using a Tab object to handle concurrent computing."

[topic-clean-function-signatures]:               /doc/topics/clean-function-signatures.md#top               "Clean Function Signatures: using function signatures without callbacks."
[topic-shallow-callback-nesting]:                /doc/topics/shallow-callback-nesting.md#top                "Shallow Callback Nesting: avoiding deeply nested functions."
[topic-top-down-control-flow]:                   /doc/topics/top-down-control-flow.md#top                   "Top-Down Control Flow: turning inverted control-flow back around."
[topic-predictable-execution-order]:             /doc/topics/predictable-execution-order.md#top             "Predictable Execution Order: avoiding issues with immediate callbacks."
[topic-modular-decomposition]:                   /doc/topics/modular-decomposition.md#top                   "Modular Decomposition:  ..."
[topic-aspect-oriented-programming]:             /doc/topics/aspect-oriented-programming.md#top             "Aspect Oriented Programming: ..."
[topic-exception-style-error-propagation]:       /doc/topics/exception-style-error-propagation.md#top       "Exception Style Error Propagation: letting errors propagate through a sequence of processing tabs."

[topic-debugging-asynchronous-events]:           /doc/topics/debugging-asynchronous-events.md#top           "Debugging Asynchronous Events: ..."

[topic-scheduling]:                              /doc/topics/scheduling.md#top                              "Scheduling: ..."
[topic-streaming-caching-queuing-collecting]:    /doc/topics/streaming-caching-queuing-collecting.md#top    "Streaming, Caching, Queuing, Collecting: ..."
[topic-extending-tab]:                           /doc/topics/extending-tab.md#top                           "Extending Tab: ..."



[ref-tab-object]:                    /doc/reference.md#tab-object                        "more attributes and methods under 'Tab Object'"
[ref-tab-constructor]:               /doc/reference.md#tab-constructor                   "more attributes and methods under 'Tab Constructor'"
[ref-tab-constructor-attributes]:    /doc/reference.md#tab-constructor-attributes        "more attributes under 'Tab Constructor Attributes'"
[ref-tab-constructor-methods]:       /doc/reference.md#tab-constructor-methods           "more methods under 'Tab Constructor Methods'"
[ref-tab-prototype-methods]:         /doc/reference.md#tab-prototype-methods             "more methods under 'Tab Prototype Methods'"
[ref-tab-instance-methods]:          /doc/reference.md#tab-instance-methods              "more methods under 'Tab Instance Methods'"
[ref-tab.schedulers-object]:         /doc/reference.md#tabschedulers-object              "more attributes and methods under 'Tab.Schedulers Object'"
[ref-tab.schedulers-attributes]:     /doc/reference.md#tabschedulers-attributes          "more attributes and methods under 'Tab.Schedulers Attributes'"
[ref-tab.schedulers-methods]:        /doc/reference.md#tabschedulers-methods             "more attributes and methods under 'Tab.Schedulers Methods'"
[ref-tab.x-object]:                  /doc/reference.md#tabx-object                       "more attributes and methods under 'Tab.X Object'"
[ref-tab.x-attributes]:              /doc/reference.md#tabx-attributes                   "more attributes and methods under 'Tab.X Attributes'"
[ref-tab.x-methods]:                 /doc/reference.md#tabx-methods                      "more attributes and methods under 'Tab.X Methods'"

[ref-new-tab]:                       /doc/reference/new-tab.md#top                       "new Tab(): construct a new tab, encapsulate a given tab if requested."
[ref-tab]:                           /doc/reference/tab.md#top                           "Tab(): convert to a tab, create a new tab if required."

[ref-tab.context]:                   /doc/reference/tab.context.md#top                   "Tab.context: the processing context for a processor function."
[ref-tab.version]:                   /doc/reference/tab.version.md#top                   "Tab.version: the version of this Tab library."

[ref-tab.construct]:                 /doc/reference/tab.construct.md#top                 "Tab.construct(): construct a new tab, encapsulate a given tab if requested."
[ref-tab.convert]:                   /doc/reference/tab.convert.md#top                   "Tab.convert(): convert to a tab, create a new tab if required."
[ref-tab.defer]:                     /doc/reference/tab.defer.md#top                     "Tab.defer(): create a function that uses a given tab to store another function's result."
[ref-tab.defer-fulfill]:             /doc/reference/tab.defer-fulfill.md#top             "Tab.deferFulfill(): create a function that updates the value of a given tab and silently blocks any further updates."
[ref-tab.defer-reject]:              /doc/reference/tab.defer-reject.md#top              "Tab.deferReject(): create a function that puts a given tab in the failed state and silently blocks any further updates."
[ref-tab.defer-return]:              /doc/reference/tab.defer-return.md#top              "Tab.deferReturn(): create a function that updates the value of a given tab."
[ref-tab.defer-settle]:              /doc/reference/tab.defer-settle.md#top              "Tab.deferSettle(): create a function that silently blocks any further updates for a given tab."
[ref-tab.defer-throw]:               /doc/reference/tab.defer-throw.md#top               "Tab.deferThrow(): create a function that puts a given tab in the failed state."
[ref-tab.defer-with]:                /doc/reference/tab.defer.md#top                     "Tab.deferWith(): create a function that uses a given tab to store another function's result, using the new function's subject as a first argument for the other function."
[ref-tab.is-tab]:                    /doc/reference/tab.is-tab.md#top                    "Tab.isTab(): was the given object created by this Tab constructor?"
[ref-tab.new-fulfill]:               /doc/reference/tab.new-fulfill.md#top               "Tab.newFulfill(): create a new tab that is initialized with a given value and silently block any further updates."
[ref-tab.new-reject]:                /doc/reference/tab.new-reject.md#top                "Tab.newReject(): create a new tab that is put in the failed state and silently block any further updates."
[ref-tab.new-return]:                /doc/reference/tab.new-return.md#top                "Tab.newReturn(): create a new tab that is initialized with a given value."
[ref-tab.new-settle]:                /doc/reference/tab.new-settle.md#top                "Tab.newSettle(): create a new tab and silently block any further updates."
[ref-tab.new-throw]:                 /doc/reference/tab.new-throw.md#top                 "Tab.newThrow(): create a new tab that is put in the failed state."

[ref-tab.prototype.cancel]:          /doc/reference/tab.prototype.cancel.md#top          "Tab.prototype.cancel(): cancel all subscriptions and all scheduled processors for this tab."
[ref-tab.prototype.do-return]:       /doc/reference/tab.prototype.do-return.md#top       "Tab.prototype.doReturn(): update the value of this tab (for ES3 environments)."
[ref-tab.prototype.do-throw]:        /doc/reference/tab.prototype.do-throw.md#top        "Tab.prototype.doThrow(): put this tab in the failed state (for ES3 environments)."
[ref-tab.prototype.fulfill]:         /doc/reference/tab.prototype.fulfill.md#top         "Tab.prototype.fulfill(): update the value of this tab and silently block any further updates."
[ref-tab.prototype.has-returned]:    /doc/reference/tab.prototype.has-returned.md#top    "Tab.prototype.hasReturned(): has this tab returned a value?"
[ref-tab.prototype.has-thrown]:      /doc/reference/tab.prototype.has-thrown.md#top      "Tab.prototype.hasThrown(): has this tab thrown an error?"
[ref-tab.prototype.is-cancelled]:    /doc/reference/tab.prototype.is-cancelled.md#top    "Tab.prototype.isCancelled(): are all subscriptions for this tab cancelled?"
[ref-tab.prototype.is-settled]:      /doc/reference/tab.prototype.is-settled.md#top      "Tab.prototype.isSettled(): are any further updates for this tab blocked?"
[ref-tab.prototype.on-cancelled]:    /doc/reference/tab.prototype.on-cancelled.md#top    "Tab.prototype.onCancelled(): execute a processor when this tab is cancelled."
[ref-tab.prototype.on-returned]:     /doc/reference/tab.prototype.on-returned.md#top     "Tab.prototype.onReturned(): execute a processor when this tab has returned a value."
[ref-tab.prototype.on-settled]:      /doc/reference/tab.prototype.on-settled.md#top      "Tab.prototype.onSettled(): execute a processor when this tab is settled."
[ref-tab.prototype.on-thrown]:       /doc/reference/tab.prototype.on-thrown.md#top       "Tab.prototype.onThrown(): execute a processor when this tab has thrown an error."
[ref-tab.prototype.reject]:          /doc/reference/tab.prototype.reject.md#top          "Tab.prototype.reject(): put this tab in the failed state and silently block any further updates."
[ref-tab.prototype.return]:          /doc/reference/tab.prototype.return.md#top          "Tab.prototype.return(): update the value of this tab."
[ref-tab.prototype.settle]:          /doc/reference/tab.prototype.settle.md#top          "Tab.prototype.settle(): silently block any further updates for this tab."
[ref-tab.prototype.throw]:           /doc/reference/tab.prototype.throw.md#top           "Tab.prototype.throw(): put this tab in the failed state."
[ref-tab.prototype.to-string]:       /doc/reference/tab.prototype.to-string.md#top       "Tab.prototype.toString(): get a string representation for this tab."
[ref-tab.prototype.value-of]:        /doc/reference/tab.prototype.value-of.md#top        "Tab.prototype.valueOf(): get the principal value of this tab."

[ref-tab.schedulers.tick]:           /doc/reference/tab.schedulers.tick.md#top           "Tab.Schedulers.es5: a sequence number incremented in every turn"

[ref-tab.schedulers.schedule-first]: /doc/reference/tab.schedulers.schedule-first.md#top "Tab.Schedulers.defer(): schedule a given callback before all other callbacks in the next turn."
[ref-tab.schedulers.schedule-last]:  /doc/reference/tab.schedulers.schedule-last.md#top  "Tab.Schedulers.defer(): schedule a given callback after all other callbacks."
[ref-tab.schedulers.schedule-next]:  /doc/reference/tab.schedulers.schedule-next.md#top  "Tab.Schedulers.notify(): schedule a given callback in the next turn."
[ref-tab.schedulers.schedule-now]:   /doc/reference/tab.schedulers.schedule-now.md#top   "Tab.Schedulers.subscribe(): call the callback immediately."

[ref-tab.x.es5]:                     /doc/reference/tab.x.es5.md#top                     "Tab.X.es5: is this library running in an ES5 environment?"

[ref-tab.x.defer]:                   /doc/reference/tab.x.defer.md#top                   "Tab.X.defer(): the basic method to create deferred functions."
[ref-tab.x.notify]:                  /doc/reference/tab.x.notify.md#top                  "Tab.X.notify(): the basic method to send notifications for a tab."
[ref-tab.x.subscribe]:               /doc/reference/tab.x.subscribe.md#top               "Tab.X.subscribe(): the basic method to subscribe to notifications from a tab."
<-- ##### start of links ##### --> 

[top]:       #top                        "back to the top of this page."
[project]:   /doc/project.md#the-project "back to the 'Project' section."
[topics]:    /doc/topics.md#topics       "back to the 'Topics' section."
[reference]: /doc/reference.md#reference "back to the 'Reference' section."



[topic-the-basics]:                              /doc/topics.md#the-basics                                  "more topics under 'The Basics'"
[topic-where-are-tabs-helping]:                  /doc/topics.md#where-are-tabs-helping                      "more topics under 'Where Are Tabs Helping?'"
[topic-where-are-tabs-lacking]:                  /doc/topics.md#where-are-tabs-lacking                      "more topics under 'Where Are Tabs Lacking?'"
[topic-advanced-topics]:                         /doc/topics.md#advanced-topics                             "more topics under 'Advanced Topics'"

[topic-a-basic-tab]:                             /doc/topics/a-basic-tab.md#top                             "A Basic Tab: creating and using a basic Tab object."
[topic-a-basic-callback]:                        /doc/topics/a-basic-callback.md#top                        "A Basic Callback: using a Tab object to handle callbacks."
[topic-a-basic-pipeline]:                        /doc/topics/a-basic-pipeline.md#top                        "A Basic Pipeline: using Tab objects for pipelining."
[topic-a-basic-stream]:                          /doc/topics/a-basic-stream.md#top                          "A Basic Stream: using a Tab object to work with sequence of data elements."
[topic-a-basic-promise]:                         /doc/topics/a-basic-promise.md#top                         "A Basic Promise: using a Tab object as a promise."
[topic-basic-lazy-evaluation]:                   /doc/topics/basic-lazy-evaluation.md#top                   "Basic Lazy Evaluation: using a Tab object for lazy evaluation."
[topic-basic-concurrent-computing]:              /doc/topics/basic-concurrent-computing.md#top              "Basic Concurrent Computing: using a Tab object to handle concurrent computing."

[topic-clean-function-signatures]:               /doc/topics/clean-function-signatures.md#top               "Clean Function Signatures: using function signatures without callbacks."
[topic-shallow-callback-nesting]:                /doc/topics/shallow-callback-nesting.md#top                "Shallow Callback Nesting: avoiding deeply nested functions."
[topic-top-down-control-flow]:                   /doc/topics/top-down-control-flow.md#top                   "Top-Down Control Flow: turning inverted control-flow back around."
[topic-predictable-execution-order]:             /doc/topics/predictable-execution-order.md#top             "Predictable Execution Order: avoiding issues with immediate callbacks."
[topic-modular-decomposition]:                   /doc/topics/modular-decomposition.md#top                   "Modular Decomposition:  ..."
[topic-aspect-oriented-programming]:             /doc/topics/aspect-oriented-programming.md#top             "Aspect Oriented Programming: ..."
[topic-exception-style-error-propagation]:       /doc/topics/exception-style-error-propagation.md#top       "Exception Style Error Propagation: letting errors propagate through a sequence of processing tabs."

[topic-debugging-asynchronous-events]:           /doc/topics/debugging-asynchronous-events.md#top           "Debugging Asynchronous Events: ..."

[topic-scheduling]:                              /doc/topics/scheduling.md#top                              "Scheduling: ..."
[topic-streaming-caching-queuing-collecting]:    /doc/topics/streaming-caching-queuing-collecting.md#top    "Streaming, Caching, Queuing, Collecting: ..."
[topic-extending-tab]:                           /doc/topics/extending-tab.md#top                           "Extending Tab: ..."



[ref-tab-object]:                    /doc/reference.md#tab-object                        "more attributes and methods under 'Tab Object'"
[ref-tab-constructor]:               /doc/reference.md#tab-constructor                   "more attributes and methods under 'Tab Constructor'"
[ref-tab-constructor-attributes]:    /doc/reference.md#tab-constructor-attributes        "more attributes under 'Tab Constructor Attributes'"
[ref-tab-constructor-methods]:       /doc/reference.md#tab-constructor-methods           "more methods under 'Tab Constructor Methods'"
[ref-tab-prototype-methods]:         /doc/reference.md#tab-prototype-methods             "more methods under 'Tab Prototype Methods'"
[ref-tab-instance-methods]:          /doc/reference.md#tab-instance-methods              "more methods under 'Tab Instance Methods'"
[ref-tab.schedulers-object]:         /doc/reference.md#tabschedulers-object              "more attributes and methods under 'Tab.Schedulers Object'"
[ref-tab.schedulers-attributes]:     /doc/reference.md#tabschedulers-attributes          "more attributes and methods under 'Tab.Schedulers Attributes'"
[ref-tab.schedulers-methods]:        /doc/reference.md#tabschedulers-methods             "more attributes and methods under 'Tab.Schedulers Methods'"
[ref-tab.x-object]:                  /doc/reference.md#tabx-object                       "more attributes and methods under 'Tab.X Object'"
[ref-tab.x-attributes]:              /doc/reference.md#tabx-attributes                   "more attributes and methods under 'Tab.X Attributes'"
[ref-tab.x-methods]:                 /doc/reference.md#tabx-methods                      "more attributes and methods under 'Tab.X Methods'"

[ref-new-tab]:                       /doc/reference/new-tab.md#top                       "new Tab(): construct a new tab, encapsulate a given tab if requested."
[ref-tab]:                           /doc/reference/tab.md#top                           "Tab(): convert to a tab, create a new tab if required."

[ref-tab.context]:                   /doc/reference/tab.context.md#top                   "Tab.context: the processing context for a processor function."
[ref-tab.version]:                   /doc/reference/tab.version.md#top                   "Tab.version: the version of this Tab library."

[ref-tab.construct]:                 /doc/reference/tab.construct.md#top                 "Tab.construct(): construct a new tab, encapsulate a given tab if requested."
[ref-tab.convert]:                   /doc/reference/tab.convert.md#top                   "Tab.convert(): convert to a tab, create a new tab if required."
[ref-tab.defer]:                     /doc/reference/tab.defer.md#top                     "Tab.defer(): create a function that uses a given tab to store another function's result."
[ref-tab.defer-fulfill]:             /doc/reference/tab.defer-fulfill.md#top             "Tab.deferFulfill(): create a function that updates the value of a given tab and silently blocks any further updates."
[ref-tab.defer-reject]:              /doc/reference/tab.defer-reject.md#top              "Tab.deferReject(): create a function that puts a given tab in the failed state and silently blocks any further updates."
[ref-tab.defer-return]:              /doc/reference/tab.defer-return.md#top              "Tab.deferReturn(): create a function that updates the value of a given tab."
[ref-tab.defer-settle]:              /doc/reference/tab.defer-settle.md#top              "Tab.deferSettle(): create a function that silently blocks any further updates for a given tab."
[ref-tab.defer-throw]:               /doc/reference/tab.defer-throw.md#top               "Tab.deferThrow(): create a function that puts a given tab in the failed state."
[ref-tab.defer-with]:                /doc/reference/tab.defer.md#top                     "Tab.deferWith(): create a function that uses a given tab to store another function's result, using the new function's subject as a first argument for the other function."
[ref-tab.is-tab]:                    /doc/reference/tab.is-tab.md#top                    "Tab.isTab(): was the given object created by this Tab constructor?"
[ref-tab.new-fulfill]:               /doc/reference/tab.new-fulfill.md#top               "Tab.newFulfill(): create a new tab that is initialized with a given value and silently block any further updates."
[ref-tab.new-reject]:                /doc/reference/tab.new-reject.md#top                "Tab.newReject(): create a new tab that is put in the failed state and silently block any further updates."
[ref-tab.new-return]:                /doc/reference/tab.new-return.md#top                "Tab.newReturn(): create a new tab that is initialized with a given value."
[ref-tab.new-settle]:                /doc/reference/tab.new-settle.md#top                "Tab.newSettle(): create a new tab and silently block any further updates."
[ref-tab.new-throw]:                 /doc/reference/tab.new-throw.md#top                 "Tab.newThrow(): create a new tab that is put in the failed state."

[ref-tab.prototype.cancel]:          /doc/reference/tab.prototype.cancel.md#top          "Tab.prototype.cancel(): cancel all subscriptions and all scheduled processors for this tab."
[ref-tab.prototype.do-return]:       /doc/reference/tab.prototype.do-return.md#top       "Tab.prototype.doReturn(): update the value of this tab (for ES3 environments)."
[ref-tab.prototype.do-throw]:        /doc/reference/tab.prototype.do-throw.md#top        "Tab.prototype.doThrow(): put this tab in the failed state (for ES3 environments)."
[ref-tab.prototype.fulfill]:         /doc/reference/tab.prototype.fulfill.md#top         "Tab.prototype.fulfill(): update the value of this tab and silently block any further updates."
[ref-tab.prototype.has-returned]:    /doc/reference/tab.prototype.has-returned.md#top    "Tab.prototype.hasReturned(): has this tab returned a value?"
[ref-tab.prototype.has-thrown]:      /doc/reference/tab.prototype.has-thrown.md#top      "Tab.prototype.hasThrown(): has this tab thrown an error?"
[ref-tab.prototype.is-cancelled]:    /doc/reference/tab.prototype.is-cancelled.md#top    "Tab.prototype.isCancelled(): are all subscriptions for this tab cancelled?"
[ref-tab.prototype.is-settled]:      /doc/reference/tab.prototype.is-settled.md#top      "Tab.prototype.isSettled(): are any further updates for this tab blocked?"
[ref-tab.prototype.on-cancelled]:    /doc/reference/tab.prototype.on-cancelled.md#top    "Tab.prototype.onCancelled(): execute a processor when this tab is cancelled."
[ref-tab.prototype.on-returned]:     /doc/reference/tab.prototype.on-returned.md#top     "Tab.prototype.onReturned(): execute a processor when this tab has returned a value."
[ref-tab.prototype.on-settled]:      /doc/reference/tab.prototype.on-settled.md#top      "Tab.prototype.onSettled(): execute a processor when this tab is settled."
[ref-tab.prototype.on-thrown]:       /doc/reference/tab.prototype.on-thrown.md#top       "Tab.prototype.onThrown(): execute a processor when this tab has thrown an error."
[ref-tab.prototype.reject]:          /doc/reference/tab.prototype.reject.md#top          "Tab.prototype.reject(): put this tab in the failed state and silently block any further updates."
[ref-tab.prototype.return]:          /doc/reference/tab.prototype.return.md#top          "Tab.prototype.return(): update the value of this tab."
[ref-tab.prototype.settle]:          /doc/reference/tab.prototype.settle.md#top          "Tab.prototype.settle(): silently block any further updates for this tab."
[ref-tab.prototype.throw]:           /doc/reference/tab.prototype.throw.md#top           "Tab.prototype.throw(): put this tab in the failed state."
[ref-tab.prototype.to-string]:       /doc/reference/tab.prototype.to-string.md#top       "Tab.prototype.toString(): get a string representation for this tab."
[ref-tab.prototype.value-of]:        /doc/reference/tab.prototype.value-of.md#top        "Tab.prototype.valueOf(): get the principal value of this tab."

[ref-tab.schedulers.tick]:           /doc/reference/tab.schedulers.tick.md#top           "Tab.Schedulers.es5: a sequence number incremented in every turn"

[ref-tab.schedulers.schedule-first]: /doc/reference/tab.schedulers.schedule-first.md#top "Tab.Schedulers.defer(): schedule a given callback before all other callbacks in the next turn."
[ref-tab.schedulers.schedule-last]:  /doc/reference/tab.schedulers.schedule-last.md#top  "Tab.Schedulers.defer(): schedule a given callback after all other callbacks."
[ref-tab.schedulers.schedule-next]:  /doc/reference/tab.schedulers.schedule-next.md#top  "Tab.Schedulers.notify(): schedule a given callback in the next turn."
[ref-tab.schedulers.schedule-now]:   /doc/reference/tab.schedulers.schedule-now.md#top   "Tab.Schedulers.subscribe(): call the callback immediately."

[ref-tab.x.es5]:                     /doc/reference/tab.x.es5.md#top                     "Tab.X.es5: is this library running in an ES5 environment?"

[ref-tab.x.defer]:                   /doc/reference/tab.x.defer.md#top                   "Tab.X.defer(): the basic method to create deferred functions."
[ref-tab.x.notify]:                  /doc/reference/tab.x.notify.md#top                  "Tab.X.notify(): the basic method to send notifications for a tab."
[ref-tab.x.subscribe]:               /doc/reference/tab.x.subscribe.md#top               "Tab.X.subscribe(): the basic method to subscribe to notifications from a tab."
<!-- ##### start of links ##### -->

[top]:       #top                        "back to the top of this page."
[project]:   /docs/project.md#the-project "back to the 'Project' section."
[topics]:    /docs/topics.md#topics       "back to the 'Topics' section."
[reference]: /docs/reference.md#reference "back to the 'Reference' section."



[topic-the-basics]:                              /docs/topics.md#the-basics                                  "more topics under 'The Basics'"
[topic-where-are-tabs-helping]:                  /docs/topics.md#where-are-tabs-helping                      "more topics under 'Where Are Tabs Helping?'"
[topic-where-are-tabs-lacking]:                  /docs/topics.md#where-are-tabs-lacking                      "more topics under 'Where Are Tabs Lacking?'"
[topic-advanced-topics]:                         /docs/topics.md#advanced-topics                             "more topics under 'Advanced Topics'"

[topic-a-basic-tab]:                             /docs/topics/a-basic-tab.md#top                             "A Basic Tab: creating and using a basic Tab object."
[topic-a-basic-callback]:                        /docs/topics/a-basic-callback.md#top                        "A Basic Callback: using a Tab object to handle callbacks."
[topic-a-basic-pipeline]:                        /docs/topics/a-basic-pipeline.md#top                        "A Basic Pipeline: using Tab objects for pipelining."
[topic-a-basic-stream]:                          /docs/topics/a-basic-stream.md#top                          "A Basic Stream: using a Tab object to work with sequence of data elements."
[topic-a-basic-promise]:                         /docs/topics/a-basic-promise.md#top                         "A Basic Promise: using a Tab object as a promise."
[topic-basic-lazy-evaluation]:                   /docs/topics/basic-lazy-evaluation.md#top                   "Basic Lazy Evaluation: using a Tab object for lazy evaluation."
[topic-basic-concurrent-computing]:              /docs/topics/basic-concurrent-computing.md#top              "Basic Concurrent Computing: using a Tab object to handle concurrent computing."

[topic-clean-function-signatures]:               /docs/topics/clean-function-signatures.md#top               "Clean Function Signatures: using function signatures without callbacks."
[topic-shallow-callback-nesting]:                /docs/topics/shallow-callback-nesting.md#top                "Shallow Callback Nesting: avoiding deeply nested functions."
[topic-top-down-control-flow]:                   /docs/topics/top-down-control-flow.md#top                   "Top-Down Control Flow: turning inverted control-flow back around."
[topic-predictable-execution-order]:             /docs/topics/predictable-execution-order.md#top             "Predictable Execution Order: avoiding issues with immediate callbacks."
[topic-modular-decomposition]:                   /docs/topics/modular-decomposition.md#top                   "Modular Decomposition:  ..."
[topic-aspect-oriented-programming]:             /docs/topics/aspect-oriented-programming.md#top             "Aspect Oriented Programming: ..."
[topic-exception-style-error-propagation]:       /docs/topics/exception-style-error-propagation.md#top       "Exception Style Error Propagation: letting errors propagate through a sequence of processing tabs."

[topic-debugging-asynchronous-events]:           /docs/topics/debugging-asynchronous-events.md#top           "Debugging Asynchronous Events: ..."

[topic-scheduling]:                              /docs/topics/scheduling.md#top                              "Scheduling: ..."
[topic-streaming-caching-queuing-collecting]:    /docs/topics/streaming-caching-queuing-collecting.md#top    "Streaming, Caching, Queuing, Collecting: ..."
[topic-extending-tab]:                           /docs/topics/extending-tab.md#top                           "Extending Tab: ..."



[ref-tab-object]:                    /docs/reference.md#tab-object                        "more attributes and methods under 'Tab Object'"
[ref-tab-constructor]:               /docs/reference.md#tab-constructor                   "more attributes and methods under 'Tab Constructor'"
[ref-tab-constructor-attributes]:    /docs/reference.md#tab-constructor-attributes        "more attributes under 'Tab Constructor Attributes'"
[ref-tab-constructor-methods]:       /docs/reference.md#tab-constructor-methods           "more methods under 'Tab Constructor Methods'"
[ref-tab-prototype-methods]:         /docs/reference.md#tab-prototype-methods             "more methods under 'Tab Prototype Methods'"
[ref-tab-instance-methods]:          /docs/reference.md#tab-instance-methods              "more methods under 'Tab Instance Methods'"
[ref-tab.schedulers-object]:         /docs/reference.md#tabschedulers-object              "more attributes and methods under 'Tab.Schedulers Object'"
[ref-tab.schedulers-attributes]:     /docs/reference.md#tabschedulers-attributes          "more attributes and methods under 'Tab.Schedulers Attributes'"
[ref-tab.schedulers-methods]:        /docs/reference.md#tabschedulers-methods             "more attributes and methods under 'Tab.Schedulers Methods'"
[ref-tab.x-object]:                  /docs/reference.md#tabx-object                       "more attributes and methods under 'Tab.X Object'"
[ref-tab.x-attributes]:              /docs/reference.md#tabx-attributes                   "more attributes and methods under 'Tab.X Attributes'"
[ref-tab.x-methods]:                 /docs/reference.md#tabx-methods                      "more attributes and methods under 'Tab.X Methods'"

[ref-new-tab]:                       /docs/reference/new-tab.md#top                       "new Tab(): construct a new tab, encapsulate a given tab if requested."
[ref-tab]:                           /docs/reference/tab.md#top                           "Tab(): convert to a tab, create a new tab if required."

[ref-tab.context]:                   /docs/reference/tab.context.md#top                   "Tab.context: the processing context for a processor function."
[ref-tab.version]:                   /docs/reference/tab.version.md#top                   "Tab.version: the version of this Tab library."

[ref-tab.construct]:                 /docs/reference/tab.construct.md#top                 "Tab.construct(): construct a new tab, encapsulate a given tab if requested."
[ref-tab.convert]:                   /docs/reference/tab.convert.md#top                   "Tab.convert(): convert to a tab, create a new tab if required."
[ref-tab.defer]:                     /docs/reference/tab.defer.md#top                     "Tab.defer(): create a function that uses a given tab to store another function's result."
[ref-tab.defer-fulfill]:             /docs/reference/tab.defer-fulfill.md#top             "Tab.deferFulfill(): create a function that updates the value of a given tab and silently blocks any further updates."
[ref-tab.defer-reject]:              /docs/reference/tab.defer-reject.md#top              "Tab.deferReject(): create a function that puts a given tab in the failed state and silently blocks any further updates."
[ref-tab.defer-return]:              /docs/reference/tab.defer-return.md#top              "Tab.deferReturn(): create a function that updates the value of a given tab."
[ref-tab.defer-settle]:              /docs/reference/tab.defer-settle.md#top              "Tab.deferSettle(): create a function that silently blocks any further updates for a given tab."
[ref-tab.defer-throw]:               /docs/reference/tab.defer-throw.md#top               "Tab.deferThrow(): create a function that puts a given tab in the failed state."
[ref-tab.defer-with]:                /docs/reference/tab.defer.md#top                     "Tab.deferWith(): create a function that uses a given tab to store another function's result, using the new function's subject as a first argument for the other function."
[ref-tab.is-tab]:                    /docs/reference/tab.is-tab.md#top                    "Tab.isTab(): was the given object created by this Tab constructor?"
[ref-tab.new-fulfill]:               /docs/reference/tab.new-fulfill.md#top               "Tab.newFulfill(): create a new tab that is initialized with a given value and silently block any further updates."
[ref-tab.new-reject]:                /docs/reference/tab.new-reject.md#top                "Tab.newReject(): create a new tab that is put in the failed state and silently block any further updates."
[ref-tab.new-return]:                /docs/reference/tab.new-return.md#top                "Tab.newReturn(): create a new tab that is initialized with a given value."
[ref-tab.new-settle]:                /docs/reference/tab.new-settle.md#top                "Tab.newSettle(): create a new tab and silently block any further updates."
[ref-tab.new-throw]:                 /docs/reference/tab.new-throw.md#top                 "Tab.newThrow(): create a new tab that is put in the failed state."

[ref-tab.prototype.cancel]:          /docs/reference/tab.prototype.cancel.md#top          "Tab.prototype.cancel(): cancel all subscriptions and all scheduled processors for this tab."
[ref-tab.prototype.do-return]:       /docs/reference/tab.prototype.do-return.md#top       "Tab.prototype.doReturn(): update the value of this tab (for ES3 environments)."
[ref-tab.prototype.do-throw]:        /docs/reference/tab.prototype.do-throw.md#top        "Tab.prototype.doThrow(): put this tab in the failed state (for ES3 environments)."
[ref-tab.prototype.fulfill]:         /docs/reference/tab.prototype.fulfill.md#top         "Tab.prototype.fulfill(): update the value of this tab and silently block any further updates."
[ref-tab.prototype.has-returned]:    /docs/reference/tab.prototype.has-returned.md#top    "Tab.prototype.hasReturned(): has this tab returned a value?"
[ref-tab.prototype.has-thrown]:      /docs/reference/tab.prototype.has-thrown.md#top      "Tab.prototype.hasThrown(): has this tab thrown an error?"
[ref-tab.prototype.is-cancelled]:    /docs/reference/tab.prototype.is-cancelled.md#top    "Tab.prototype.isCancelled(): are all subscriptions for this tab cancelled?"
[ref-tab.prototype.is-settled]:      /docs/reference/tab.prototype.is-settled.md#top      "Tab.prototype.isSettled(): are any further updates for this tab blocked?"
[ref-tab.prototype.on-cancelled]:    /docs/reference/tab.prototype.on-cancelled.md#top    "Tab.prototype.onCancelled(): execute a processor when this tab is cancelled."
[ref-tab.prototype.on-returned]:     /docs/reference/tab.prototype.on-returned.md#top     "Tab.prototype.onReturned(): execute a processor when this tab has returned a value."
[ref-tab.prototype.on-settled]:      /docs/reference/tab.prototype.on-settled.md#top      "Tab.prototype.onSettled(): execute a processor when this tab is settled."
[ref-tab.prototype.on-thrown]:       /docs/reference/tab.prototype.on-thrown.md#top       "Tab.prototype.onThrown(): execute a processor when this tab has thrown an error."
[ref-tab.prototype.reject]:          /docs/reference/tab.prototype.reject.md#top          "Tab.prototype.reject(): put this tab in the failed state and silently block any further updates."
[ref-tab.prototype.return]:          /docs/reference/tab.prototype.return.md#top          "Tab.prototype.return(): update the value of this tab."
[ref-tab.prototype.settle]:          /docs/reference/tab.prototype.settle.md#top          "Tab.prototype.settle(): silently block any further updates for this tab."
[ref-tab.prototype.throw]:           /docs/reference/tab.prototype.throw.md#top           "Tab.prototype.throw(): put this tab in the failed state."
[ref-tab.prototype.to-string]:       /docs/reference/tab.prototype.to-string.md#top       "Tab.prototype.toString(): get a string representation for this tab."
[ref-tab.prototype.value-of]:        /docs/reference/tab.prototype.value-of.md#top        "Tab.prototype.valueOf(): get the principal value of this tab."

[ref-tab.schedulers.tick]:           /docs/reference/tab.schedulers.tick.md#top           "Tab.Schedulers.es5: a sequence number incremented in every turn"

[ref-tab.schedulers.schedule-first]: /docs/reference/tab.schedulers.schedule-first.md#top "Tab.Schedulers.defer(): schedule a given callback before all other callbacks in the next turn."
[ref-tab.schedulers.schedule-last]:  /docs/reference/tab.schedulers.schedule-last.md#top  "Tab.Schedulers.defer(): schedule a given callback after all other callbacks."
[ref-tab.schedulers.schedule-next]:  /docs/reference/tab.schedulers.schedule-next.md#top  "Tab.Schedulers.notify(): schedule a given callback in the next turn."
[ref-tab.schedulers.schedule-now]:   /docs/reference/tab.schedulers.schedule-now.md#top   "Tab.Schedulers.subscribe(): call the callback immediately."

[ref-tab.x.es5]:                     /docs/reference/tab.x.es5.md#top                     "Tab.X.es5: is this library running in an ES5 environment?"

[ref-tab.x.defer]:                   /docs/reference/tab.x.defer.md#top                   "Tab.X.defer(): the basic method to create deferred functions."
[ref-tab.x.notify]:                  /docs/reference/tab.x.notify.md#top                  "Tab.X.notify(): the basic method to send notifications for a tab."
[ref-tab.x.subscribe]:               /docs/reference/tab.x.subscribe.md#top               "Tab.X.subscribe(): the basic method to subscribe to notifications from a tab."