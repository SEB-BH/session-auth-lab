<h1>
  <span class="headline">Session Auth Lab</span>
  <span class="subhead">Exercise</span>
</h1>

# Part 1 - Create the Entry Model

Create a model called **Entry**.

Your model should contain the following fields:

| Field     | Type     | Validation                 |
| --------- | -------- | -------------------------- |
| title     | String   | Required, `maxLength: 100` |
| entryBody | String   | `maxLength: 350`           |
| isPublic  | Boolean  | Default: `false`           |
| owner     | ObjectId | `ref: "User"`              |

---

# Part 2 - Create Controller and mount it in server.js

1. Create a `entries.controllers.js` file. import the router from express and export it at the bottom of the file. (HINT: The `index.controllers.js` file has the code inside)
2. Inside the `server.js` import the `entries.controllers.js` where you see `// controller Imports` (HINT: It should look almost exactly the same as the other 2 controller imports)
3. Where you see `// Routes go here` add `app.use()` and mount your routes on `/entries`. (HINT: should look almost exactly the same as the `/auth` routes mounting)

# Part 3 - New Entry Page

Create the **GET** route that displays the form for creating a new journal entry on the `/entries/new` route.

Use the **is-signed-in** middleware so that **only logged in users** can access this page.

### Checkpoint

Try visiting this page without being logged in.

You should be redirected to the **Sign In** page.

---

# Part 4 - Create an Entry

1. Create the **POST** route that receives the form submission.

2. Protect this route using the **is-signed-in** middleware.

3. Inside the route, for now just `console.log()` the req.session.user and notice that the value is the logged in user we saved in the session:

```javascript
console.log(req.session.user)
```

Notice that `req.session.user` contains the **currently logged in user**.

4. Now in the `post` route make sure that when an entry is created the owner is the person who is logged in. REMEMBER from the previous step req.session.user is the object that contains the logged in user. Look at the Entry model and ask yourself "How are we saving the owner? What is the data type of that field?" **HINT**: Its ObjectId


Now save the new journal entry with the logged in user as its `owner`.

---

# Part 5 - Public Entries

Create a route that displays **all entries marked as public**. This route should be at `/entries`. REMEMBER in the model we have a field `isPublic` that is a boolan so when using `.find()` you should ONLY get the entries with `{isPublic: true}`

Render them inside the `all-entries.ejs` page and loop through them.

***CHECKPOINT***: Create a new entry with isPublic as false and come back to this page and check if it appears in `all-entries.ejs`

---

# Part 6 - My Entries

Create a route called:

```
/my-entries
```

This route should return **only the entries created by the currently logged in user.**

**Hint:** "You" refers to the user who is currently logged in. **EXTRA HINT**: If you add the middleware to the route you will have access to `req.session.user._id` which contains the id of the currently logged in user

Inside the EJS page, loop through the entries and display them.

### Checkpoint

Verify that:

* You only see entries created by the logged in user.
* Every entry has the logged in user's `_id` saved as the `owner`.

---

# Part 7 - Navigation

- Add a link to **My Entries** in the navigation bar.

- Make sure **only logged in users** can see this link. 

---

# Part 8 - Ownership

- On the **All Entries** page, conditionally display **Edit** and **Delete** buttons.

- A user should only see these buttons for entries that **they created**.

**Hint:** In your EJS templates, you already have access to the logged in user's `_id` `<% user._id %>. Use it to determine whether the current user owns the entry.

---



# Big Bonus: Admin panel

For this bonus we want to build a functioning admin panel. The admin should be able to go to this page, see all the users and be able to toggle a user from normal user to admin

1. In the user model add an extra field for admin. It can be `isAdmin` as a boolean or `role` with an enum. The important part is user can be created and by default he will NOT be admin
2. Now in your sign up route make sure that the user is signed up as normal user and not admin
3. Now in the sign in route make sure the session is created with the role
4. Create a isAdmin middleware that will show the user the route if he is admin and will take him back to homepage if he's not
5. Create a `/admin` route and add the `isAdmin` middleware to it that we built in the previous step. This route should show the admin all the users in our application, if they are admin or not. and by each users name there should be a button to switch them to admin or not admin
6. Create the `/toggle-admin` route that should be a POST route. When a request is sent we need to check if the requester is an admin and if they are then the user should be switched from admin to normal user or normal user to admin depending on their current role.
7. Create another button and route in the admin panel for deleting a user. When this button is pressed a POST request should be sent to the server that deletes the user clicked on. Of course make sure only admins can send this request.
6. Now add a link to the `/admin` route in the navbar but it should only show for admins


## Bigger bonus: Soft Delete

Notice that now all admins can delete users. the trust in an admin has to be very strong. now we want it that an admin can "delete" a user but that users data should remain in the database

1. In your user model as a `isDeleted` field that is a `boolean`. By default it should be `false`
2. Now when the user logs in we want to check that the user is in the database and not deleted
3. Now have it that the `/admin` route only returns users who have the `isDeleted` as false
4. Change the functionality for deleting a user in the admin panel `POST` route to not actually deleting the user but just updating their `isDeleted` field to `true`