---
layout: post
title: "Looking backward"
author: "Neil"
---

Matt asked me the other day "What's on your 'learn this in 2012' list?" It got me thinking that rather than tell someone and not have it recorded anywhere, I should be able to be held accountable for what I said I'd do. Welcome to the fruit of that particular bit of thinking. Rather than dive straight in looking forward I thought it'd be helpful to take a look back at 2011 and make sense of what I've picked up during a pretty hectic year.

### Where we started
We started January in the middle of rewriting two of our most important services, at the same time. Our team are responsible for the metadata within Nokia Entertainment. We maintain six services that combine to provide storage, indexing and search functionality which underpins our entertainment platform.

We hadn't deployed anything more than a hotfix to live for a few months and I get the feeling we were viewed by those around us as a team that weren't really doing anything of value. It took six months of toil to get those services out there but the lessons learned along the way were so helpful and have permeated almost every aspect of our work since. Like those awful motivational posters say: "Learning is a journey, not a destination", or something similarly twee.

### Real developers ship
Being a developer and not having anything deployed for users to actually use is a frustrating feeling. Our main reason for doing the rewrite was to add the new functionality that was needed for the continued expansion of our platform. Our original code was written as we were all learning Java and we had made some mistakes that we decided would be too time-consuming to unpick. It would also see us join the 21st century and get on board with our continuous delivery strategy. This would see us going from 'big-bang' deploys of months of work by non-developers to us pushing the button on all of our releases when we decided it was time for a deployment.

I remember one of the team working on our continuous delivery service asking me what my expectations were. I said that if we were able to have an idea on Monday and have it in production by Friday I'd be a happy little developer. It turns out my wishes more than came true. We went from having no control over deployments to being able to write code and get it in front of users within twenty minutes. To say that this was empowering would be a monumental understatement. The bugs that are inevitable in our job can be fixed-forward rather than rolled-back, we can tweak configuration values within minutes if something isn't behaving and most of all we, as developers, are in control.

This is by no means a new idea, but it's helped us stop thinking in terms of cramming as much as we can into big releases. We can make small iterations and we don't forget what we're about to deploy because it was written weeks ago.

### Acceptance testing
When we were starting out we decided to write our acceptance tests using [Uncle Bob](https://twitter.com/unclebobmartin)'s excellent [FitNesse](http://fitnesse.org/). We had the mistaken belief that our product owners might want to write our acceptance tests for us. This wasn't the case. We also made the mistake of wiring our services up to run our acceptance tests (more like integration tests) which made our tests complicated and brittle.

Based on those lessons we now do our acceptance testing with [JUnit](https://junit.org) against an instance of our service running against (in most cases) mocked dependencies. This approach and the libraries we wrote to help test our REST services spawned my first real experience of open source programming with [REST-driver](https://github.com/rest-driver/rest-driver). We're now at the point where someone new to a project can run the tests with two commands (`git clone` and `mvn verify`). Using JUnit means that we're writing our tests in the same language we use to write our services and the tests are easy to run (meaning there's no excuse for not running them).

Our FitNesse tests now form an impressive regression suite that we run daily over our services. It's a bit of a chore to edit and even more of a chore to run, down to the way we've used it not FitNesse itself. It will be retired when the versions of our APIs that it tests are finally deprecated.

### Performance testing isn't a silver bullet
I spent the early part of 2011 banging on to anyone who'd listen about the importance of performance testing our services. As a team, we must have spent a month or so solidly working on performance tests for the rewritten services. [JMeter](https://jmeter.apache.org/) was recruited for hitting the service, [Groovy](http://www.groovy-lang.org) was used for some extra scripting and [Protovis](https://mbostock.github.io/protovis/) was used for customised graph creation. I expected our performance test graphs to be flat lines with a little jump when someone committed some code which was slow and then promptly flattened back out as we spotted the bottleneck and fixed it. What we got was a completely different story. Most of our lines were jumping all over the place. We then proceeded to let our performance tests decay and I don't think they've been run for months.

We got some confidence that our services were at least comparable speed-wise to the previous versions but it turns out performance testing wasn't as important as I thought it was.

### Being able to see what's going on
What actually gave us more confidence was the introduction of metrics to our services. Previously to have any inkling of how our services were performing we were limited to:

* [Ganglia](http://ganglia.sourceforge.net/) which gave us 'metal' stats for our servers
* Getting someone to jump on our live boxes and let us browse the log files or asking someone to send us the log files
* A single email notification that someone had scripted for us as a cron job
* Our internal logging service (which has an interface which makes it difficult to use)

Following on from the disappointment of our performance testing and spurred on by a proof-of-concept created by one of the team we found [Coda Hale](https://codahale.com/)'s [Metrics](https://github.com/codahale/metrics) library. **I can't emphasise enough how important this little library has been to us.** We've gone from a position of having virtually no visibility of how our services are acting to having a group of metrics so detailed and up-to-date that we are beginning to get to a position of predicting when something is about to go wrong, or diagnosing problems early.

We now begin to make performance decisions based on these metrics. Rather than performing outside-in performance testing (such as described above) where our service is called and we measure total response times we're able to get all this and more by building the measurements into our service. Again, this is by no means ground-breaking in the grand scheme of things but, for us, it's an epic win which has changed the way we work and how we are able to indicate to our stakeholders what is going on.

This metrics-based approach is being adopted by other teams so we're able to overlay the behaviour of communicating services on top of each other, allowing us to interpret trends and help improve the experience for our customers.

### Making stuff visible
It seems strange now to look back at the position we were in a year ago and not be able to get 'live' stats from our services. We had an information radiator which showed our CI status but that was it. We now feed our metrics into [Graphite](https://graphiteapp.org) and have a comprehensive set of stats and graphs that rotate throughout the day on a big screen in the middle of our team. In some cases spotting impending doom has been as simple as watching for a sudden change in the lines of a graph. All of this data is available to our stakeholders which means that they can hold us to account on how we're doing as well.

### Going 'off-piste' every so often
A by-product of our job is that the truly interesting tasks aren't necessarily what we spend most of our time doing. I'm grateful my job throws up more than enough interesting challenges but sometimes you'll find something shiny that you just have to get your grubby hands on there and then. As a result of some of this 'shiny-following' I've had the chance to scratch my JavaScript and data visualisation itches. I've seen others doing similar things and more often than not this stuff ends up helping with work eventually. We've all got jobs that we're paid for but a little distraction can fire-up some enthusiasm or spark something new.

### Meeting people
The change of approach mentioned above hasn't just occurred as a result of browsing the web or coming up with it internally. In March a number of people attended [QCon](https://qconlondon.com/) and we learned that, as a company, we weren't far off the industry leaders in terms of our continuous delivery strategy but there was plenty of inspiration for how we structure our services and monitor them. This inspiration came back to the office and started to sneak into the things we were doing. I can't really remember any developers attending conferences before QCon 2011 but it has got people keeping an eye out for events to attend.

Getting out, meeting new people and ranting with them is a great way to start solving problems or to spot problems you might be wandering blindly towards. Plus, it's always nice to go somewhere and geek out with a bunch of like-minded people.

### Finally, some small ones
* Use [Git](https://git-scm.com) - branches are a fantastic way to prevent toe-treading and I'm still amazed by its ability to smoosh things together smoothly.
* Get an SSD - sitting in front of your machine while it boots or Eclipse fires up is no fun.
* Read [Twitter](https://twitter.com/) - I've found that my focus is moving from the RSS feeds I track using [Google Reader](https://www.google.com/reader) to Twitter. I've got lucky and started following a group of people who are feeding me interesting links all the time, and I don't even have to pay them!

So, with that out the way, I can start looking forward to what I hope 2012 has in store.
