![Last Cat Standing](/img/LastCatStanding.png "IPUMS Last Cat Standing")

[Link to Tailwind Game](//www.dreamfreely.xyz/ipums/tailwind)

# Last Cat Standing

This is a solution to a coding exam that asked me to create a single-elimination tournament for 16 photos obtained via API endpoint, using HTML, CSS & JavaScript.

After accomplishing the initial goals, as well as the extra points; I decided to use the project as an opportunity to compare the CSS frameworks [Bootstrap](https://getbootstrap.com/) & [TailwindCSS](https://tailwindcss.com/); with two additional stretch goals of creating the SPA into a Vue app, and then adding a Ruby on Rails server, to maintain persistent winning GIFs. These last two goals were not completed, as I preferred to focus on writing this write-up and cleaning the code.

That said, in this write-up I will first explain my process, after which I'll cover a few difficulties, then I'll do a shallow comparison of Bootstrap and Tailwind.

I chose to use the jQuery library to prioritize the demonstration of basic coding abilities, DOM comprehension, and this write-up, rather than attemptting to dazzle with library selections, etc.

And so we continue ...

## Build Process

Upon receiving the assignment, my first step was to create a user path; the question for our tournament being, *what happens once one of the two pictures is chosen?*

Using JavaScript's jQuery library, an onClick event is triggered by the choice:

1) One photo will be discarded
2) One photo will be saved
3) Get the next set of two photos
4) Repeat until no more photos
5) Move winners to staging array

Pen and paper was used to record this, after which it was improved to describe the following:

1) Consume API > get all photos (post in staging array)
2) Display first two photos
3) Select 1 photo (post in the winner's array)
4) Last photo set > swap winners & staging array
5) Empty the winner's list and repeat
6) if len(winner's list)==1 ?

![First Steps](/img/paper.jpg "First Steps")

> A quick note on web-design *best practices* ... each framework and domain seems to have their own ideas wrt to what "best practices" actually are ... my goal was to be clear with my code, and keep it relatively clean. To these ends I have separate HTML, CSS and JavaScript files. I did attempt to be considerate of a11y; while regarding the JavaScript code, I listed the variables first, followed by functions, followed by operational code.

> I decided to make a single page application, rather than a multi-page application, due to time and preferred focus. The creation of a Vue app, with the addition of persistant database, feels more appropriate for a multi-page application. Both the competition and gallery space are able to be accessibly displayed on the same-page, and that seemed reasonable enough for this exam.

### Initial Coding Build

Boiler plate HTML, CSS and JavaScript were implemented that import jQuery and Bootstrap.

Next, the onClick event was coded with the photo URLs hardcoded. Then, the winner array was created and tested. After which the API endpoint was connected; and the tournament mechanics fine-tuned to achieve their goal.

Now that the mechanics of the application were established, code optimization took centerstage; the first step was moving the *processVote*, *roundChange*, and *winRound* mechanics out of the onClick event, and into their own functions. 

Next was cleaning up the CSS, a combination of classes and ids were used to identify DOM elements and coordinate their changes, in accordance with gameplay. These names were condensed where possible.

Lastly, I added preliminary flourishes, and the extra-credit component, that would help going forward.

 1) Background-color for the competition space
 2) Preparing Local Storage to retain winning images
 3) Adding a headline to track competition rounds.

 I am not a designer, and feel I have just enough sense to get by; so I did not focus on making a fancy UI, just something that is, at the least, presentable.

## Taking a break ...

All of this work was accomplished on Friday, and so I felt entitled to a tasty beverage and food with a friend that evening ... who happened to be playing live music in St Paul ... though still being considerate of the task at hand, and my mind primed for the work ... 

I did not want to let my bus ride go to waste, and so using the Android application [JuiceSSH](https://juicessh.com/), I was able to ssh into my server on which I am serving and maintaining a copy of the code ... I continued to make updates (mainly aesthetic / CSS stuff) to the code using Vi ... 

![JuiceSSH](/img/juicessh.jpg "Juice SSH")

### Complications

I knew that there would be issues with normalizing image sizes, as well as centering images/objects, across screensizes. I did not attempt to remedy the image size issue due to the constraints/focus of the exam.

Next, when implementing Local Storage; after clearing the local storage the page needs to be refreshed (`window.location.reload()`) immediately, otherwise the Local Storage won't be cleared.

And while not exactly a complication, I did not write any error messages ... I'm familiar with Promises in JavaScript; though with regards to this project ... things were so straightforward that error-messages don't feel entirely applicable. 

I may be wrong, and am open to learning where they might best be placed. Perhaps if the API call fails, I suppose ... I'll put a `.catch()` there quick :p

# Bootstrap VS TailwindCSS

Onwards to our comparison ...

Bootstrap was initially used for basic formatting (background colors, buttons, etc). I've always had difficulty centering objects with Bootstrap; and this time was no different.

So I removed all of the Bootstrap CSS, and copied the barebones of the SPA into the `basicapp` folder. Then I rebuilt the aesthetic aspects using Tailwind CSS.

## Shallow Comparison

**Bootstrap** == straightforward, accessible syntax; difficulty centering.

**Tailwind** == explicit syntax, better grid/centering control - more divs required though; need to create `btn` classes separately.

I really like Tailwind personally, so let's review the negatives of the framework first.

1) Decorator tags are "meaningless" i.e. `<h1 class="text-2xl">` and `<h3 class="text-4xl">` are both mechanically accceptable tags and attributes.
2) You will need to create your own `button` classes, as well as classes for alerts and messages; Tailwind does not provide these out of the box.

There is another glitch that I've noticed with Bootstap, and Tailwind as well; sometimes when setting parameters for varying screensizes, the setting will not display correctly, even though the code is written correctly. *More on this later.*

Now for **what I don't like about Bootstrap** ... I've been using it since it was released ... and it feels clunky (it feels like Legos, and I want Technics!). The buttons and designs are wonderful, the aesthetic is pleasing, and the implementation is often fairly straight-forward. It's the centering of objects that always frustrates me; and exactly why I've been leaning more on Tailwind lately. (I like symmetry.)

The *issue* with Tailwinds' method of centering, is that objects are centered both horizontally AND vertically; you cannot seem to choose one or the other. This means that more `<div>` tags are required to position all objects in the layout accordingly. These additional `<div>` tags enable sections to be quarantined, and aligned without affecting other aspects of the layout.

And this is most helpful when trying to create a UI that will function in predictable, and aesthetically pleasing ways, across platforms. Granted, the additional `<div>` tags aren't horrible, because they can also be used to denote aspects of the page for accessibility purposes as well.

## What I do like

As mentioned, Bootstrap is boilerplate basic; easy to get an idea of what you're building. Tailwind allows you to fine-tune via class names as you go though!

As well, Tailwind's ability to create a configuration file that can be carried from one project to the next is very appealing.

### More Info on Screen Size Complications

One thing I've noticed with both frameworks is how breakpoints and media-sizes can vary with implementation.

The best example being the following line of code:

`<div class="grid lg:grid-cols-2 md:grid-cols-1  place-items-center">`

The first iteration of this line in Tailwind was as follows:

`<div class="grid md:grid-cols-2 sm:grid-cols-1  place-items-center">`

During attempts to adjust centering, this code was *not* changed; and yet it became unresponsive. I attempted to revert the changes, but the page was still unresponsive (not converting to a single column for mobile.) It was not until I changed the code to `lg:grid-cols-2 md:grid-cols-1` that the columns became responsive.

> This may have been due to zooming options in the browser.

As well, I noticed that the SPA displays differently in Firefox, than it does in Brave; this was noticed more clearly when the `.stage` and `#champions` sections had background colors; the sections spanned to full width of the page on mobile with Firefox. 

It may be more difficult to notice now.

# Stretch Goals

While the stretch goal of using Vue was not completed, the boilerplate code was created, and Vue-Router included to make a separate page for Persistent Champions. The Ruby on Rails server was not even started. Though both goals may be pursued in order to refresh myself on the frameworks.

# Conclusion

The project was incredibly enjoyable. My thought was that the exam focused on two objectives: demonstrate a working knowledge of HTML, CSS, and JavaScript, while demonstrating the ability to communicate one's work and decision paths.

Feeling confident that I could construct the basic project, I attempted to take an additional step of using the project to do a shallow comparison of two CSS frameworks; hopefully to add further utility to the solution.

An added bonus was being able to see the application without any design CSS; and from this barebones structure being able to build the same application with varying frameworks; and eventually into a Vue app.

Admittedly, I did not research Bootstrap to resolve the centering as much I could have ... and this may be due to bias on my part. It was great to refresh myself with Tailwind though, in both cases I used CDN access. 

Tailwind *does* have some unique configuration options that can be implemented when building from code, which can remedy some of the aforementioned complications. But the game plays!

Again, in conclusion, fun project, thank you for the chance to create this solution!