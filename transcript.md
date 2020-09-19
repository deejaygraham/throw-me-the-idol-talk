# Throw me the Idol

Introduction...


As software people, we know that the things we need are constantly in need of change. The world changes around the code and our expectations of what it could and should do also change. If they could be written once and never looked at again, we wouldn't have half the headaches we have. 

So code needs to change how often and how quickly that's possible depends on how well prepared we are for those changes. We would like it to be quick, painless and happy.

In lots of cases, it can be long, error prone and painful.

This is where I mention the dreaded word "legacy". Legacy for me means that code is useful or valuable, it's doing a worthwhile job for someone and it's something we want to change, tweak or add to without breaking what's currently there. It doesn't mean (but it can) an old COBOL system piled high with dust of the ages. (Knight) It could be a reliable utility or report generator that everyone uses and depends on. It could be built on the most modern framework available but it's a spike that got written over two weeks to prove a technology but that was 3 months and a lot of coffee ago so the definition of legacy is more like just some code that you don't fully understand, trust or even have a ghost of a working model for.

INDIANA 

You as a developer might find yourself in a new job where the last person left leaving you in charge of this awesome system. You may have moved teams or the legacy code you inherit may have even been from your past self who was stressed and in a hurry to meet a deadline and didn't have time for naming and tidying code and documenting and explaining to future self.  

Over the years I've found myself in that situation repeatedly, almost everytime I've moved jobs despite being assured in the interview that this was a greenfield project and there was no old code. Except that as soon as you start to write any code, you are building a legacy which is why for the developer, tests are an important part of keeping a handle on design, coupling, cohesion and all those good things. Often the kind of code we are talking about doesn't have any tests and has usually been proven to work just by some manual inspection in the fist instance and then by having the edges knocked off by minor revisions based on user feedback. But to have confidence in making changes to your new inherited code, you need to have some form of testing in place.

Any time you try to search for how do I change this code? Mostly the search results are "just write some tests for it". Which is great advice a bit like this:

Draw the rest of the owl

Or, to link back to the theme of my talk, at the beginning of Raiders of the Lost Ark, as they are trying to escape from the rapidly crumbling temple, Alfred Molina offers to trade Indie the Idol for his whip. A classic standoff which is just like that advice. Write tests the advice goes, before you can change anything. But to write the test you have to change something. So how do you get started?

## <small>Start</small>

To begin with, don't try to change everything all at once, if we have a request to change something, let's only consider the code around that area. Either directly what we need to change or perhaps at a slightly wider scope, depending on how easy a chunk will be to allow for testing. The majority of the code doesn't need to change so for now let's pretend it doesn't exist and can be ignored.

## Be aware of your environment

sourcers apprentice

Or rather be aware of the environment that the code is running in. The definition of a unit test (from Feathers) is that the code doesn't touch the network, a database, the file system, system environment in general (clock, date).

I once worked for a company that did Barcoding Solutions and one of our customers had a huge inventory of medical equipment in warehouses all over the world, connected by the internet and managed by handheld scanners. We wrote the code that ran on the scanners and tests that ran in that environment could easily ship actual stuff in and out of warehouses and generate mountains of barcode shipping labels.

Coloplast - AS400 stock control system, barcode scanners

Another company even earlier we worked on motion control software for the glass industry and that was even more dangerous because accidentally forgetting to disconnect our code meant we could be pushing super hot glass around the factory with some serious consequences for safety.

Mocking and Stubbing is your friend.

It's important to do some discovery about the application context and work out how you can isolate your code, or if that's not possible yet in your journey to make the code testable, where are the boundaries where the code talks to the outside world and how can we shut those off? This could be as simple as pulling out network cables or turning off features in configuration or even misconfiguring to point to less critical resources under your control. If that kind of approach won't work, it may be that you need to be a bit more radical to begin with. Emulators at the edges of a system can be a good way forward. A new job I found myself working in was for a control system for an x-ray QA machine for the semiconductor industry. 

(IBM)

IP means I can't share any photos from that time so here's one of my old security passes. The whole system could run from a laptop but really really wanted a network connection, a set of motors and an x-ray source to talk to. When a fully loaded container of semiconductor wafers was worth around $250,000 retail, it really wasn't a good idea to work with live systems. 

## Switches 

The first stage in getting a handle on that code was in investigating where the edges of the system were and installing some switches and some logging.

(railway)  

This meant we could switch between live and fake instances using configuration and the logging meant that we could record the conversation of data going in and out of the system so that for an given event, our fake system could reply with a canned response or manufacture a plausible answer that would keep the system running.  

This can work out even as a long term strategy because shortly after we got all this fakery in place, we were asked to adapt the system to a whole new manufacturing control system which we had never seen before. And we had to do it live on a customer site and we had to make it work in a couple of days. 

FOUP delivery system 

Of course it didn't work first time, the timing, the protocol was mostly right but just different enough that it wouldn't work to the satisfaction of this new system we were talking to. Logging to the rescue. We turned that off, tried again and then disappeared to check the logs and look for what was different. Some tiny code changes later we were able to emulate this new system and debug where our program was going wrong. 

## Disengaged

Once we are satisfactorily disengaged from the outside world, we can work on understanding more about the code and making more changes.


## Text is your friend
Tests are your friend

Boy Scout rule - make things a little better through naming and re-arrangement

Extract Method or class - make that testable then embed in original place

Acceptance tests


Debugger

## Code Coverage
Not for metrics but to see whether tests are covering what you think they are.

## DIY Test framework

## Approval Testing

Michael feathers - Legacy Code

Martin Fowler

Make tiny changes every time you touch the code.

Smallest Change possible  - move variable, group better, run tests
rename variable.

Refactor not rewrite - 

Refactor - use the built in refactorings that are there because they are reliable and stable so you can't really make a mess. 

Unit Tests, longer acceptance tests, Integration tests, 
