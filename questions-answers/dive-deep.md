Dive Deep
Leaders operate at all levels, stay connected to the details, audit frequently, and are skeptical when metrics and anecdote differ. No task is beneath them.
•	Not pass the buck on unwanted tasks, demonstrate hustle and a 'do what it takes' attitude to get things done, even if that means being hands-on?
•	Stay closely connected to the details of projects/business, knowing when to get involved without micromanaging?
•	Frequently "audit' by drilling down into projects/business, question and providing feedback, quickly assessing progress and risk, and hold employees accountable for results?
•	Drill down on fuzzy information, refusing to accept generalizations or lightweight responses?
•	Have a firm grasp of the details of your work in order to deeply discuss it?
•	Frequently "audit' your work by checking accuracy, facts and assumptions?
 
Interview Questions
1	Tell me about a time you were trying to understand a problem on your team and you had to go down several layers to figure it out. Who did you talk with and what information proved most valuable? How did you use that information to help solve the problem?
S: Eventually Consistent Event Driven programming a problem that I felt like digging into was a never ending tunnel of layers after layers.
T: I wanted to understand a full stack and it finally took my writing my own full system, as a prototype to internalize the benefits. Of a append only, write only service, than could then expose a read only store.
Users initiate Command, these fire one or more events, event listeners queue system modifications that update the data store.
Queries are read only agains the data store
Logging a command stream and and “Event Stream” offers so much
A: I understood conceptually, but I needed to better understand the inner workings by writing some throw away code.
R: So many benefits to writing a system like this. Not only is the system repeatable and can recreate itself, it has built in auditing and reporting. Not only reporting on the system data, but this offers reporting on the inner workings of the system.
How often are specific features used. Correlated with other features?
What is the history of a piece of data? Where did it come from, when was it last updated, by who, why, when, what were the previous values.
Learning: Also introduced snapshotting for speed in recovery.
This system also let’s you easily create and recreate testable scenarios.
1	Tell me about a problem you had to solve that required in-depth thought and analysis? How did you know you were focusing on the right things?
S: I was recently asked to create a system around translation of content for an event website. It’s important to draw a line that separates the actually website and marketing copy, with the dynamic content of the website. In this case our dynamic content were sessions inside the event.
T: Create a system that understood and processed changes as existing data changed, without having to retranslate content that did not change.
A: I built a Machine Translation system that could offer almost instant results, backed up with Human Translation that would verify and fix translated data after the fact. Tricks here were how to surface the data changes as individual jobs, instead of redoing all the work again.
R: I kicked up a single mass initial translation, and loaded results back into our system in a single batch. Later as session titles, descriptions, and other data were updated, these were treated as individual change orders, on a per language basis, since the human translation teams were in different regions of the world.
Learning: This process could mature over time. I was unable to get a single change request or event (think web hook?)for data changes. Instead I received regular full data dumps, so I had to then create a diff gram on my own to inspect data changes. My data dump was in excel, a binary format not easily used for tracking changes. I built an API that opened the excel file, ensured sorting was properly set by repeatable session id. Then I exported this data to JSON and relied on github diff grams to expose change orders.
1	Tell me about a time when you linked two or more problems together and identified an underlying issue? Were you able to find a solution?
S: Often problems are surfaced as an issue, but are caused by an undercurrent that is not immediately visible. I have recently automated a series of transformers that would translate data from an excel file, into a textual {JSON} representation. This {JSON} was then translated/localized into many languages, and then transformed back into Excel. Excel would open, but would not display data in some languages. The data was properly placed in cells, but the visual representation would be unreadable unicode.
T: What was happening that seemingly corrupted this file?
A: Finally I figured out to try to match a source file, with a destination file. English Excel>JSON>excel worked, but English Excel> Json > Korean JSON > Excel failed. So I think this is an encoding issue.
I took a working Korean Excel file > JSON > Excel and was able to proof the encoding was accurate.
Now it's reduced to something with the file format.
Using  coworker lead me down the path of BOM's for UTF-8 and 16 encoding
This lead me down the path of stack overflow and to better understand a BOM ( byte order mark )
Ref: https://stackoverflow.com/questions/12308509/how-to-output-byte-order-mark-when-writing-to-textwriter
Ref: https://docs.microsoft.com/en-us/globalization/encoding/byte-order-mark
R: Using this code, I was able to include a byte order marker in my excel writer, and thereafer Excel was able to open and properly display all translated languages and special characters.
1	Can you tell me about a specific metric you have used to identify a need for a change in your department? Did you create the metric or was it already available? How did this and other information influence the change?
S: Baseline metrics are important to understand what's happening today, and level set on what could be improved in the future. When creating a new Learn TV service, we did not have a way to collect metrics. Our first goal was to start broadcasting the programming. Once programming was in place we could start capturing metrics.
T:  What metrics would matter the make informed decisions? Viewership is important but even more important Is interactions. How do we know the user is doing anything.
A: I invented a new metric that could corelate a logged in user on Learn TV, with other activity inside docs.microsoft.com, especially our learn modules. This would let us know if our television content was driving learn modules.
R: To our surprise and delight our content that focused on learning more, via learn modules were working great, and content that did not have calls to action for Learn Modules performed at zero.
Learning:  While this was not a show stopper (literally) it was a high mark to set, where every show was highly encouraged to have matching learn modules. This helped the content scale into a self paces learning program.
1	Walk me through a big problem or issue in your organization that you helped to solve. How did you become aware of it? What information did you gather, what information was missing and how did you fill the gaps? Did you do a postmortem analysis and if you did what did you learn?
a	See #6
2	Give me a situation in which it took you asking why five times to get to the root cause.
a	WHY ARE THE ANALYTIC Results DIFFERENT?
S: Where are these analytics coming from, and how do we know they are complete? If they are incomplete, are the results repeatable. Metrics for video come from many many sources. There is page load event, but that is from the web page container, not the video player. Does the video player capture and bubble events from the payer IFRAME to the parent web page, and are these events captured? If they are captured, are they then duplicate events? The video is playing via a stream source over CDN. The CDN fires metrics
T: What metrics are valid, duplicate, important, repeatable?
A: Early in the project we now define tracking metrics and safely avoid tracking PII ( personally identifiable information ).
R: Once our metrics are defined and trustworthy, we are able to use them in experiments and trust the results. It's easy to assume metrics are reliable, because you're looking at a big pile of data. But like any magician, the metrics can easily obscure the facts and present false data.
Learning:
1	As a manager, how do you stay connected to the details while focusing on the strategic, bigger picture issues? Tell me about a time when you were too far removed from a project one of your employees was working on and you ended up missing a goal. (Manager)
S: I like to witness others doing code reviews. I'll simply sit in on code reviews to inspect what is happening, where, and why. Once in a while, I'll ask for a request/response step-by-step that walks me through the codes many layers of abstractions. There have been many demo's that I am not familiar with the internal workings, because these were demo's not meant to be used in production scenarios. We have open sourced many demo's that do act as foundations to what other people build upon. I have recently discovered a portion of a project that is off track. I didn't miss a goal because of this, but I was able to course correct.
T: The developer was starting to reinvent a wheel that had already been invented many times. The area here is around reusable package management.
A: I was able to course correct and even through it took a little longer to implement a private nuget hive, we were then later able to fully take advantage of the entire nuget ecosystem, instead of relying on a home grown package manager.
R: I love to read code, see others code review, and even write code, though I know this is not my strength.
