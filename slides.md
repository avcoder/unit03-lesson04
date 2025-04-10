---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Node.js - part 4/12
Back-End Development
- [ ] Express.js
- [ ] Handle a GET HTTP request by sending HTML
- [ ] Create an API route sending JSON

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
TODO: fill in anchor href above to point to github repo for these slides
-->

---
transition: slide-left
---

# Exercise1 : Understand what Express is, Setup an Express server
(30 min) What is Express? 

```js
1. create a new folder and go into it via `cd folder`
2. Type `npm init -y` // review - what does this do again?
3. `npm install express` // what changes were made in your folder now?
4. `npm install nodemon` // research in npmjs.com what nodemon does
5. create an index.js file with the following code:
```
```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('🚚 Welcome to the Food Truck!');
});

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```
Goto next slide...

<!--
- Express = open-source, standard API framework for Node.js
- handles implementation details for us
- can create a full-stack application using the Model View Controller (MVC) pattern
- or can create a REST API 
-->

---
transition: slide-left
---

# Exercise 1 continued: Express.js

```js
6. test run your code via `node index.js` 
   - Pain point: but if you make a file change, have to Ctrl+C to stop server then restart
7. Similar to how Live Server restarts upon a file change, let us use nodemon to do the same
   - edit package.json so that inside the "scripts" block enter new line: "start": "nodemon index.js"
   - remember: previous line has to end in a comma ,
   - save file, then test via `npm run start` > does it still work? does it restart on file change?
8. Stretch goal: Remove the hard-coded value of the port number to a .env file.
   - Try different status code `res.status(200).send('🚚 Welcome to the Food Truck!')`
   - Implement `process.env`; See last week slide https://unit03-lesson02.netlify.app/14 
```
Goto next slide...

---
transition: slide-left
---

# Exercise 1 continued: Express.js

Matching Game - Below is jumbled.  Match the left with the correct answer on the right

| Code Line        | What it Does           |
| ------------- |:-------------:|
| 1. `const express = require('express')`    | A. Tells the server what to do when someone visits the home page (/) |
| 2. `const app = express()` | B. Starts listening for requests on a specific port |
| 3. `app.get('/', (req, res) => {})` | C. Imports the Express framework |
| 4. `app.listen(3000, () => { ... })` | D. Creates an Express application instance |

Go to next slide...

<!--
-->

---
transition: slide-left
---

# Exercise 1 continued

Discuss:
-  Why would you use a framework like Express instead of Node.  compare [last week slide](https://unit03-lesson02.netlify.app/presenter/4)?
- Share one cool thing you learned or a mistake you fixed?
- If we wanted to add a 'Contact Us' page, what part of this code would we need to change?

Save and push your code to GitHub; ignore any folders/files that don't need to be uploaded

---
transition: slide-left
---

# Exercise 2: GET requests
(50 min) Create Express app that responds to GET request. Use routing to display a custom message

```js
1. Include a /hours route that sends '🕒 We are open from 11am to 7pm!'
// Hours route
app.get('/hours', (req, res) => {
  res.send('🕒 We are open from 11am to 7pm!');
});

2. Include a /contact route that sends '📞 Contact us at 555-FOOD or hello@foodtruck.com'
   - stretch goal: send message via HTML with inline CSS
3. Include a /menu route via:
      res.send(`<html><head><title>Menu</title></head><body>
            <h1>🥙 Our Menu</h1>
            <ul>
              <li>Tacos</li>
              <li>Quesadillas</li>
              <li>Smoothies</li>
            </ul>
          </body></html>`);
```

<!--
-->

---
transition: slide-left
---

# Exercise 2 continued
Remember: save code to your GitHub

Discuss:
- What would happen if we didn't include `app.listen()` at the bottom of our file?
- Can you see any limitations (especially in terms of developer experience DX) of sending hard-coded HTML as a string using `res.send()`?
- Why do we use different routes instead of putting everything on the homepage?
- What might happen if two routes are accidentally the same?
- How does the server know which route to respond to?
- How would you update your `/hours` route to serve dynamic content that changes daily?


Stretch goal: Implement a wild card route to show a custom 404 message via:
```js
    app.use((req, res) => {
      res.status(404).send("404 Not Found");
    });
```

<!--

-->


---
layout: image-right
transition: slide-left
image: /assets/traversy.png
backgroundSize: 500px 250px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- ▶️ [Express Crashcourse](https://www.youtube.com/watch?v=CnH3kAXSrmU)
- ⚙️ [Install/setup Express JS](https://www.youtube.com/watch?v=huc9RWb0yX4)
- 📦 [npmjs](https://www.npmjs.com/)

<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

<!-- 
- take attendance
-->

---
transition: slide-left
---

# Exercise 3: Create API that sends JSON
(50 min) 

```js
1. Create a new backend endpoint in your index.js to /api/user

const axios = require('axios'); // If not already installed: npm i axios

app.get('/api/user', async (req, res) => {
  try {
    const response = await axios.get('https://randomuser.me/api/?results=1');
    const user = response.data.results[0];

    const data = {
      name: `${user.name.first} ${user.name.last}`,
      email: user.email,
      location: `${user.location.city}, ${user.location.country}`,
      picture: user.picture.medium
    };

    res.json(data);
  } catch (err) {
    res.status(500).json({ error: 'Failed to fetch user' });
  }
});
```

<!--
- https://randomuser.me/documentation#multiple
-->

---
transition: slide-left
---

# Exercise 4: Connect your backend endpoint to frontend

- To download index.html from link:  Shift+right-click > Save Link As [index.html](/assets/index.html)
- Review: make frontend fetch from your backend api endpoint to display the person/data dynamically
- Stretch goal: fetch 3 people instead of 1 and display all 3
- Stretch goal: make the frontend continually fetch every 30 seconds (ex: to emulate getting the newest data)(hint: setInterval)
- Stretch goal: Pretend Senior Leadership wants to trim down the amount of JS sent to the wire.  Refactor the axios code and replace it with fetch.

Go to next slide...

---
transition: slide-left
---

# Exercise 4 continued

Discuss:
- What happens behind the scenes when the frontend does a GET request to the server? 
- How might this structure be more secure than simply having the frontend fetch the Random User API directly?
- If the frontend isn't receiving the expected data, what are some debugging steps you can take to identify the issue?
- If you wanted to extend this app to display multiple users or add a search feature, what changes would you need to make in both the backend and frontend?



---
transition: slide-left
---

# Homework

- Assignment 2 due this Apr. 13 midnight EST
