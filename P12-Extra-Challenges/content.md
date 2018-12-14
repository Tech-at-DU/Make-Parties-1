---
title: "Extra Challenges"
slug: extra-challenges
---

For more challenges read one!


# Async / Await

JavaScript ES7 (2016), a newer version than ES6 (2015), has a really neat feature that you can add to your project called `async`/`await`.

In some places in our code we have what is called [Callback Hell](http://callbackhell.com/). We can fix this once an for all with `async`/`await`. Behold:


```js
app.get('/events/:id', (req, res) => {
  models.Event.findById(req.params.id).then(event => {
    event.getComments({ order: [ ['createdAt', 'DESC'] ] }).then(comments => {
      event.getRsvps({ order: [ ['createdAt', 'DESC'] ] }).then(rsvps => {
        res.render('events-show', { comments: comments, event: event, rsvps: rsvps });
      });
    });
  });
});
```

is the same as:

```js
app.get('/events/:id', async (req, res) => {
  let event = await models.Event.findById(req.params.id)
  let comments = event.getComments({ order: [ ['createdAt', 'DESC'] ] });
  let rsvps = event.getRsvps()
  res.render('events-show', { comments: comments, event: event, rsvps: rsvps });
});
```

Notice that the whole function is called `async`, and the `await` keyword prefaces any function that returns a promise.

Try using this pattern to update one of your own routes.

# Adding Comments

Imagine you get some feedback that people planning and rsvp'd to events would like to comment on the event like a "wall" on facebook.

Comments are a lot like Rsvps, so you should be able to add them by referencing that code. Here are the steps you should take:

1. Add a new comment form to the `events-show` template below the `{{event.desc}}`.
1. Have that form submit to a **nested** comments create action in its own `comments.js` controller.
1. Create a comment model with the attribute `content`, and then add the **foreign key** `EventId` just like Rsvps has. (Reminder - make sure to add the association to the Comment and Event models)
1. See if you can create a comment associated with its parent event.
1. Now get your comments from the event. (Hint - You can use the command `include: { all: true}` to get all models associated with your event, and then access its comments via `event.Comments` like `event.Rsvps`.)
1. Dipslay your comments below by iterating through `event.Comments`.