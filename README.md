<a name="top" ></a>

<img src="http://raw2.github.com/stefaan-coussement/Tab/master/doc/img/tab-logo128.png" alt="Tab logo" align="left" style="float:left; margin-top:-8px;" height="87" /><img src="http://raw2.github.com/stefaan-coussement/Tab/master/doc/img/1x1.png" align="left" style="float:left;" height="79" width="20" />
# [Tab][top]
Get help with callbacks, pipelines, streams, promises, lazy evaluation and concurrent computing.
<br />

## [The Project][project]

This project was started out of an interest in javascript promises.  However, in our view, a lot of the designs we looked into have some short-comings.  

Then came CommonJS modules...

And then came an article about Microsoft's reactive extensions...  

What if we would combine some of the aspects of promises, modules and reactive extensions?  So the end result is an attempt to put a slightly different slant on asynchronous programming.

Because this API seriously differs from the promises defined in other projects, we avoided a name that is closely linked to previous implementations such as *promises*, *futures*, *deferreds*, *vows*, *delays*, *eventuals*.  We also wanted to avoid the '-able' suffix like in *Observables*.  We eventually went for *Tabs*.

>**tab** (noun)
> A projection, flap, or short strip attached to an object to facilitate opening, handling, or identification.  
> [http://www.thefreedictionary.com/tab]
>
>**keep tabs on** (idiom)
> To observe carefully.  To maintain a watch over, record activities of.  
> [http://www.thefreedictionary.com/tab]

The general goal is to help with callbacks, pipelines, streams, promises, lazy evaluation and concurrent computing.

>**callback** (programming)
> In computer programming, a callback is a piece of executable code that is passed as an argument to other code, which is expected to call back (execute) the argument at some convenient time. The invocation may be immediate as in a synchronous callback or it might happen at later time, as in an asynchronous callback.  
> [http://en.wikipedia.org/wiki/Callback_(computer_programming)]
>
>**pipeline** (programming)
> In software engineering, a pipeline consists of a chain of processing elements (processes, threads, coroutines, etc.), arranged so that the output of each element is the input of the next; the name is by analogy to a physical pipeline. Usually some amount of buffering is provided between consecutive elements. The information that flows in these pipelines is often a stream of records, bytes or bits, and the elements of a pipeline may be called filters; this is also called the pipes and filters design pattern. Connecting elements into a pipeline is analogous to function composition.  
> [http://en.wikipedia.org/wiki/Pipeline_(software)]
>
> When a programming language is originally designed without any syntax to nest function calls, pipeline programming is a simple syntax change to add it. The programmer connects notional program modules into a flow structure, by analogy to a physical pipeline carrying reaction products through a chemical or other plant.  
> [http://en.wikipedia.org/wiki/Pipeline_programming]
>
>**stream** (programming)
> In computer science, a stream is a sequence of data elements made available over time. A stream can be thought of as a conveyor belt that allows items to be processed one at a time rather than in large batches.  
> Streams are processed differently from batch data – normal functions cannot operate on streams as a whole, as they have potentially unlimited data, and formally, streams are codata (potentially unlimited), not data (which is finite). Functions that operate on a stream, producing another stream, are known as filters, and can be connected in pipelines, analogously to function composition. Filters may operate on one item of a stream at a time, or may base an item of output on multiple items of input, such as a moving average.  
> [http://en.wikipedia.org/wiki/Stream_(computing)]
>
> In computing, reactive programming is a programming paradigm oriented around data flows and the propagation of change. This means that it should be possible to express static or dynamic data flows with ease in the programming languages used, and that the underlying execution model will automatically propagate changes through the data flow.  
> [http://en.wikipedia.org/wiki/Reactive_programming]
>
>**promise** (programming)
> In computer science, future, promise, and delay refer to constructs used for synchronizing in some concurrent programming languages. They describe an object that acts as a proxy for a result that is initially unknown, usually because the computation of its value is yet incomplete.  
> [http://en.wikipedia.org/wiki/Promise_(programming)]
>
> A promise is the final item in a stream. A stream itself is a way to shorten a very long sequence into a short one, and request only the items needed at the moment.  
> [http://en.wikipedia.org/wiki/Promise_(computing)]
>
>**lazy evaluation** (programming)
> In programming language theory, lazy evaluation, or call-by-need is an evaluation strategy which delays the evaluation of an expression until its value is needed (non-strict evaluation) and which also avoids repeated evaluations (sharing). The sharing can reduce the running time of certain functions by an exponential factor over other non-strict evaluation strategies, such as call-by-name.  
> [http://en.wikipedia.org/wiki/Lazy_evaluation]
>
>**concurrent computing** (programming)
>Concurrent computing is a form of computing in which several computations are executing during overlapping time periods – concurrently – instead of sequentially (one completing before the next starts). This is a property of a system – this may be an individual program, a computer, or a network – and there is a separate execution point or "thread of control" for each computation ("process"). A concurrent system is one where a computation can make progress without waiting for all other computations to complete – where more than one computation can make progress at "the same time".  
>[http://en.wikipedia.org/wiki/Concurrent_computing]



### Check it out

* [A Basic Tab][topic-a-basic-tab]  
  creating and using a basic Tab object.

* [A Basic Callback][topic-a-basic-callback]  
  using a Tab object to handle callbacks.

* [A Basic Pipeline][topic-a-basic-pipeline]  
  using Tab objects for pipelining.

* [A Basic Stream][topic-a-basic-stream]  
  using a Tab object to work with sequence of data elements.

* [A Basic Promise][topic-a-basic-promise]  
  using a Tab object as a promise.

* [Basic Lazy Evaluation][topic-basic-lazy-evaluation]  
  using a Tab object for lazy evaluation.

* [Basic Concurrent Computing][topic-basic-concurrent-computing]  
  using a Tab object to handle concurrent computing.



### Goals

The following is a maintained, and thus regularly updated and re-organized list of goals (in no particular order).  Some may seem a bit obscure without proper explanation.  Where needed and when time allows, we will create [topics][topics] to discuss them in more detail.  

1.  Basic Tabs

    1. :ok: provide the means to capture a future value in a tab and notify its observers when the value becomes available.
    
    1. :ok: provide the means to capture special values in a tab, such as `undefined`, `null` and other tabs.
    
    1. :ok: provide the means to capture a future error in a tab and notify its observers when the error becomes available.
    
    1. :ok: provide the means to work with progress events, including recoverable exceptions (reject brings a promise into a final and non-mutable state that we cannot recover from).
    
    1. provide the means to work with foreign tabs (tabs created by another tab-like constructor).
    
    1. provide the means to work with remote tabs (via a message interface).
    
    1. provide the means to cancel a tab, both its subscription for the events from other tabs and its ongoing action undertaken as a consequence of such past events.
    
1.  Callbacks

    1. :ok: provide the means to use a tab to capture the arguments for a callback, without executing the original callback, and then use the original callback function as the processor of the tab's notification.
    
    1. :ok: provide the means to capture a callback's result, without needing to change the callback's signature.
    
    1. :ok: provide the means to capture a callback's error in a tab, without needing to change the callback's signature.
    
1.  Pipelines

    1. :ok: use a fluid API wherever it makes sense (a Deferred object is not chainable).
    
    2. :ok: provide the means to asynchronously process notifications by pushing them into a pipeline of processing tabs.
    
    3. provide the means to encapsulate a pipeline of tabs in a function that then can be used as a module to compose longer pipelines.
    
1.  Streams

    1. provide the means to generate a stream of data elements, without caching or recording the generated data elements.
    
    1. :ok: provide the means to generate a stream of data elements, caching the last generated data element for observers that register after that data element was generated.
    
    1. provide the means to generate a stream of data elements, recording the sequence of generated data elements for observers that register after the data elements were generated.
    
    1. provide the means to play back a cached data element or a recorded sequence of data elements using an reactive push-model.
    
    1. provide the means to play back a cached data element or a recorded sequence of data elements using a interactive pull model.
    
1.  Promises

    1. provide the means to settle a tab with a value or error, i.e. a final and non-mutable data element, and notify its observers.
    
    1. provide the means to solve security concerns (isolation of provider and consumer) without penalizing other aspects of the API (solving security concerns is a primary goal in the promise design, in some ways limiting other API methods).
    
    1. keep compatibility with the [Promise/A+](http://promises-aplus.github.io/promises-spec/) project where possible.
    
    1. keep interoperability with [Q](https://github.com/kriskowal/q) promises where possible.
    
    1. keep interoperability with [jQuery](http://jquery.com/) promises where possible.
    
    1. keep interoperability with [NodeJS](http://nodejs.org/) promises where possible.
    
1.  Lazy Evaluation

    1. provide the means to define a tab without effectively executing the definition until the tab's result is needed (promises don't support lazy evaluation).
    
    1. provide the means for a tab to define dependencies on other tabs without triggering their evaluation (lazy evaluation).
    
1.  Concurrent Computing

    1. provide the means to combine multiple streams into a single tab.
    
    1. provide the means to combine these streams in a number of different ways:
        * wait for the value of all tabs to calculate a result (similar to boolean logic)
        * wait for the value of the first tabs in sequence to calculate a result (similar to javascript `||` and `&&`)  
        * wait for the value of the first tabs in time to calculate a result
        
    
1.  Miscellaneous
    
    1. :ok: provide the means to use a tab to capture the subject and arguments of a method, for instance for use during testing.
    
    1. :ok: provide the means to use a tab to capture the subject, arguments and result or error of a method, for instance for use during testing.
    
    1. provide the means to augment a tab with contextual information that stays accessible through the chain of tabs that are created when handling events (f.i. using the .try method).
    
    1. provide the means to throttle the execution of event handlers (f.i. only four outstanding http requests allowed at one time).
    
    1. provide the means to work with tabs in an ES3 environment.
    
    1. provide an experimental version that uses *ES.next* weak maps instead of closures.
    
    1. investigate if we can provide an experimental interface to set and get the value of a tab, using *ES.next* proxies.  It is however impossible to provide full proxying on a tab.  The prototype methods defined for Tab cannot be trapped and applied to the contained object, unless **all** of the current prototype methods are moved to a corresponding constructor method, essentially breaking the requirement of a fluid API.  Instead, we may introduce a special `.value` attribute to serve as a full proxy for the contained object? ...



### Influences

There are a lot of other projects that were (and still are) influencing this project, many more than I am able to mention, but here are a couple of the most important ones:

* [Q](https://github.com/kriskowal/q)  

* [CommonJS](http://wiki.commonjs.org/wiki/CommonJS), especially the work on Modules and Promises  
<a title="Visit CommonJS" href="http://wiki.commonjs.org/wiki/CommonJS">  
  <img src="http://wiki.commonjs.org/images/b/bc/Wiki.png" height="80"
 alt="CommonJS logo">
</a>

* [Promise/A+](http://promises-aplus.github.io/promises-spec/)  
<a title="Visit Promises/A+" href="http://promises-aplus.github.com/promises-spec">
    <img src="http://promises-aplus.github.com/promises-spec/assets/logo-small.png" height="80" alt="Promises/A+ logo" />
</a>

* [RxJS, The Reactive Extensions For Javascript](http://rxjs.codeplex.com/)  
<a title="Visit RxJS" href="http://rxjs.codeplex.com/">
    <img src="http://download-codeplex.sec.s-msft.com/Download?ProjectName=rxjs&DownloadId=530910&Build=20865" height="80" alt="RxJS logo" >
</a>



<br /> Back to [Top] | [Project] | [Topics] | [Reference] <br />
