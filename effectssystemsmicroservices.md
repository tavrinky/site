A Microservice is a Centralized Effects System

EDIT: A friend explained to me that while this is a common use of microservices, it's better to just use a library. I hadn't much experience with microservices, as they were thought, rather microservices as they are made, which seems to be more of an expression of Conway's law rather than programming theory. 

Epistemic status: likely not particularly novel

If you're running a Haskell backend with a non-trivial amount of microservices, you're probably using an effect system too. You're probably using MTL (although you might not be, I think what I'm about to say is just as true of polysemy and fused-effects), and when you're instantiating your monad stack, even with the same constraints, across microservices, you're probably using different transformers, or interpreting them differently. Perhaps this is intentional, which is fine, but likely some of them have come out of sync due to bitrot, or different decisions made when greenfielding a new service.

How would you keep them in sync? Another microservice.

Say many of your services call from a centralized library, all of which impose a MonadLogger constraint on the call. Different services implement and peel it differently, maybe some have structured logging, some use katip while some use monad-logger, either way, they come out of sync, and refactoring them to use the same transformer/runner is then linear in your services. If you've got a lot of services and little time, refactoring will only come one at a time, maybe a service a month, until after you've refactored all of them up to a certain standard, a new standard has shown its strengths.

How do you make the refactoring time constant in number of services? Build yet another service. If you just turn the methods into endpoints, you've just built yourself a centralized effect system.

Now, what happens if you want to try out a new way of doing {logging, database retrieval, finding out what time it is*}? Well, that's the fundamental tension between distribution and centralization. 

*Please for the love of god, don't do this. Yes, with Servant it's just 5 lines of code, but do you enjoy the sound of angels weeping? (Serious answer: link the library statically not over HTTPS) 