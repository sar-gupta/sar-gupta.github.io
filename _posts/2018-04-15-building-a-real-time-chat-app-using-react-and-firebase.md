---
title: "Building a real-time chat app using React and Firebase"
date: 2018-04-15 12:00:00
permalink: /posts/2018/04/15/building-a-real-time-chat-app-using-react-and-firebase
tags: 
  - web
  - react
  - firebase
---
<p align="center"><img src="/images/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/01.png" alt="image"></p>

> App is being served at this link, go chek it out!
> [https://spacedev.herokuapp.com/](https://spacedev.herokuapp.com/)
> Join the chatroom `test` for connecting with fellow devs!

For those of you interested in just the code, you can dive right in 🏊‍♂️
Instructions for running locally have been provided in the [README](https://github.com/sar-gupta/space/blob/master/README.md).

> [CODE IS HERE](https://github.com/sar-gupta/space)

If you want to read a little bit about the project before getting your hands dirty with code, continue on 😄 I’ve tried to make this post fun for the reader by including memes and gifs, hope you enjoy it!

SO! Hello everyone. I recently learned React and Firebase, so I was looking for some sort of project where I could practice and gain experience with them. After going through a ton of ideas, I decided to build a chat application. Easy peasy lemon squeezy 🍋

<!-- ![oh well….](/img/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/02.jpeg) -->
<p align="center"><img src="/images/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/02.jpeg" alt="image"></p>
<p align="center">
*oh well….*
</p>
While building it, I ran into many roadblocks (way too many). Fortunately for you, I’m writing about my experience building this app, so that hopefully you don’t run into as many problems if you decide to build something on your own. But getting bugs in you code is not bad. It’s actually good, since you get to learn something new! Also, consider this: You’re up at 4 A.M. thinking about how to solve a problem, black coffee your only companion, and suddenly it just clicks. You make the changes and see your code working correctly. I can personally verify; it’s a different feeling altogether!

<!-- ![](/img/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/03.gif) -->
<p align="center"><img src="/images/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/03.gif" alt="image"></p>

Yup, that’s how my last week went: waking up till late, squashing bugs, and then running into even more bugs 🤐😂

But finally, the app was working 🎉 SO, let’s dive right into it.

## All about the code 👨‍💻

The app uses **React** and **Redux** to manage the front-end and **Firebase** to manage the backend. React is a framework that render the application on screen according to the application “state”. Managing this “state” becomes very difficult with complex apps. Enter Redux. Redux is a state-manager for React. And finally, firebase provides backend-as-a-service; super-convenient!

The app is structured as follows:

<!-- ![App Structure](/img/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/04.jpeg) -->
<p align="center"><img src="/images/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/04.jpeg" alt="image"></p>
<p align="center">
App Structure
</p>
All of our React code sits inside the src folder. [Babel](https://babeljs.io) compiles everything for us, and outputs it into public/dist/bundle.js

The server serves the public/ folder.

I won’t bore you with all the configuration files, you can have a look at them on [Github](https://github.com/sar-gupta/space).

You can look at how to set up a firebase project in the [README](https://github.com/sar-gupta/space/blob/master/README.md).

I provided the Login with Github option, so that I can later use the Github API to integrate some cool features for developers.

So, we want to make a real-time app, right. There’s two things we need to do a ton of times over and over again. We need to update the database, and after that, we need to dispatch actions to update the application state. If I want to summarize the app, the previous line would be it.

<!-- ![](/img/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/05.gif) -->
<p align="center"><img src="/images/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/05.gif" alt="image"></p>

As you can probably guess, the database has mainly 2 parts: rooms (chat-rooms) and users.

Now, whenever you want the app to re-render, you just change the state by dispatching actions. Some of the actions are like creating room, joining room, leaving room, sending a message, updating the number of unread messages… You get the general idea.

These actions are synchronous, meaning that they run code line by line, one after the other. If there’s any asynchronous code, they don’t wait for it to get the result back. There’s no provision of a callback or promise. Since we have to write data to firebase (which is asynchronous), we will use a redux middleware called redux-thunk. It provides us with the capability to dispatch asynchromous actions. The general pattern for all actions is pretty similar:
> We update the database, and in the callback, we dispatch the boring old synchronous action to update the application state.

<!-- ![Oh Shelly, stop it, will ya?](/img/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/06.gif) -->
<p align="center"><img src="/images/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/06.gif" alt="image"></p>
<p align="center">
*Oh Shelly, stop it, will ya?*
</p>

An important action is to **set the start state**. Whenever the user opens your app, you want to show the app to him just like he left (with updates about new messages). You don’t want to user to face a black screen every time he opens the app. So, whenever the app refreshes, we set the start state for the user by reading it off the database.

Now, the tricky part of the app is **how to make it real-time?**

Each user has their own machine, and **the state is local to their browser window**. Knowing that the state is local is important for building any React app. By local, what I mean is that state changes of the app on one machine will not affect the state of the app on any other machine.

We want that when one user sends a message, the state changes on all users’ machines who are in the same room, and the freshly sent message is displayed. So, how do we do that?

![](/img/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/07.gif)

Well, it’s not as complicated as you might think it is. Our good old friend Firebase is here to help!

For reading data from the database, firebase broadly provides two methods:

* **.once()** : This will give a snapshot of the database at the time when it is called. It executes just once, and then leaves the database alone, until it is called again.

* **.on()** : Now this is what we want for the job at hand. This method starts listening to changes on some part of the database, and runs a callback function every time that part gets updated.

The documentation for these methods can be found [here](https://firebase.google.com/docs/database/web/read-and-write) in the [firebase docs](https://firebase.google.com/docs/).

So, when one user sends a message, the following things happen:

1. The database gets updated with the new message.

1. Local state of the user is changed by dispatching an action in the callback of the asynchronous send message action.

1. All users of that room have been listening to changes for new messages in that room using the .on() method provided by firebase. As soon as the new message gets written to the database, the callback function of .on() is called. We use this function to dispatch an action which updates the local state of the user. This callback is called simultaneously for all users’ machines who’ve been listening to changes for a new message in that particular room.

( 2 and 3 occur almost simultaneously, since both get fired immediately after data has been written to the database).

Understanding the concept of asynchronous and synchronous apps is essential for building **any** web application. I ran into bugs a ton of times just because I forgot about the asynchronous nature of some event or another. This type of mistake in your code is difficult to detect, because it doesn’t give any error. It just doesn’t do what it’s expected to do, without any explanation of why it’s happening.

I hope you enjoyed the article and learnt something new along the way!

<!-- ![That’s all, folks!](/img/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/08.gif)*That’s all, folks!* -->
<p align="center"><img src="/images/2018-04-15-building-a-real-time-chat-app-using-react-and-firebase/08.gif" alt="image"></p>

### The app is far from perfect, but you help it get closer to that goal, and simultaneously add another project (as a contributor) to your resume!

Feel free to fork the [github repository](https://github.com/sar-gupta/space) and open issues and pull requests.

In case you’re feeling lazy about scrolling all the way up for the code, I got you covered:

> [CODE IS HERE](https://github.com/sar-gupta/space)