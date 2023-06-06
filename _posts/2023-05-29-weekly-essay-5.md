---
title: To-Do App
categories: [Weekly Essay]
tags: []
# math: true
---

After a long time since the last post, we're back! Let's try to explain our
latest project: _A To-Do App_.

You can see the complete code on my personal Git page or in my GitHub account.

|                 Personal Git Page                  |                           GitHub                           |
| :------------------------------------------------: | :--------------------------------------------------------: |
| [Front End](https://cgit.hisiste.com/ToDo-App-FE/) | [Front End](https://github.com/Hisiste/ToDo-App-Front-End) |
| [Back End](https://cgit.hisiste.com/ToDo-App-BE/)  |  [Back End](https://github.com/Hisiste/ToDo-App-Back-End)  |

## Goals and Requirements

As the name implies, this project is about creating and managing to-dos. We have
the following functional requirements for this app:

-   [x] Create a "to-do" specifying the name, a priority, and possibly a due
        date.
-   [x] Ability to edit name, priority and due date for existing "to-do" tasks.
    -   Being able to specify a due date or clear the due date (because you may
        not be interested in when to finish that "to-do").
-   [x] Being able to filter "to-dos" specifying the name (or part of the name),
        the priority and if they are done/undone.
-   [x] Being able to sort the "to-do’s" by priority and/or due date.
    -   For example, be able to sort items where their due date is soon and sort
        them also by priority to see what tasks are more urgent or less urgent.
-   [x] Mark "to-do’s" as done (clicking in a checkbox) or to undo a "to-do".
    -   The undone functionality is just there if there is a mistake. :D
-   [x] Since it is possible that you can have a lot of "to-dos" there's the
        need to paginate the list of "to-dos".
-   [x] Ability to know, in average, the time between creation and done for all
        "to-dos". This should be shown in general for all done "to-dos" and
        also grouped by priority.
    -   This could be a metric to measure performance.

The project was separated into two projects: The _Front End_ part and the _Back
End_ part. Each part has its own details and technologies. Let's discuss all of
them.

### The Front End

We'll follow this markup to design the app:

![](/assets/posts_assets/2023-05-29-weekly-essay-5/UX-UI-markup.png)

1. Search/Filtering Controls.
2. New To Do Button. This should open a modal to type the "to-do" data.
3. Priority column should show in the header the classic up and down arrows to
   allow the user to sort.
4. Due date column should show in the header the classic up and own arrows to
   allow the user to sort.
5. Action column to show actions (links/buttons) to allow the user to delete or
   edit a "to-do".
    - To Edit is ok to show a modal similar to the one to create a "to-do".
6. Pagination control. Showing the pages, its number and the next
   and previous page is enough.
7. Area to show the metrics.

We're restricted to use the following technologies for this project:

-   JavaScript.
-   ReactJS.
-   Redux.

Neither of which I've used before. So this will be an interesting project.

### The Back End

This is further broken into two parts: the _model_ and the _API_. The _model_ is
the information we're storing for every to-do. The _API_ will be our connection
between the Front End and the Back End. Let's detail the requirements for each
part.

#### Model

Every to-do should have:

-   ID. This could be a number or string or a combination. Must be unique.
-   Text (required). Max length is 120 chars.
-   A due date (optional).
-   Done/undone flag.
-   A done date. When the "to-do" is marked as done this date is set.
-   Priority (required). Options: High, Medium and Low.
-   Creation date.

#### API

-   A GET endpoint (/todos) to list "to-dos".
    -   **Include pagination. Pages should be of 10 elements.**
-   Sort by priority and/or due date.
-   Filter by:
    -   The name or part of the name.
    -   Done/Undone.
    -   Priority.
-   A POST endpoint (/todos) to create "to-dos".
    -   Validations included.
-   A PUT endpoint (/todos/{id}) to update the "to-do" name, due date
    and/or priority.
    -   Validations included.
-   A POST endpoint (/todos/{id}/done) to mark "to-do" as done.
    -   This should update the "done date" property.
    -   If "to-do" is already done nothing should happen (no error returned).
-   A PUT endpoint (/todos/{id}/undone) to mark "to-do" as undone.
    -   If "to-do" is already undone nothing should happen.
    -   If "to-do" is done, this should clear the done date.

Finally, we're restricted to using the following technologies:

-   Java.
-   Maven.
-   Spring Boot.

And again, none of which I have experience. Oh, boy.

## What I have learned

A LOT. Not only have I learned enough of each technology to (barely) make a
functional To-Do App from almost scratch in less than 3 weeks, but we're
restricted to **NOT** use a database. At first, I thought "well, I don't know
anything about databases either, so this is something one technology less to
learn." Later I finally understood that this requirement:

> No need to use a database by now, storing data could be in memory using Java
> Collections, and it is ok if data is lost when the application is
> shutdown. But they are asking you to design the persistent layer such that it
> will be somehow easy to switch from in-memory implementation to a database
> implementation.

will cause me a lot of trouble. Well, not that much trouble, but things would
have been smoother if we're allowed to use databases. (Or at least that was my
conclusion.)

So, without further ado, let's summarize everything I've learned during this
whole new project:

### Front End

To use [_Redux_](https://redux.js.org/), we have to understand key-concepts
like:

Functional Programming : Decomposing a problem into a bunch of small and
reusable pieces.

Higher Order Functions : A function that takes a function as an argument and/or
returns a function.

Pure Functions : Where a function with the same arguments yield the same
outputs. No external behavior should alter the output.

And little more.

Redux was heavily used at first. Creating the store and developing the reducers
to store all to-dos was my first move. Not the best, but my first. I learned
about [Bootstrap](https://getbootstrap.com/) and used it to create the UX/UI.
Buttons, a table for the to-dos, containers with borders, and very messy **but
functional** forms for adding a new to-do, editing a to-do and filtering through
to-dos. Every time a reducer worked, I celebrated it. It wasn't very difficult
to make them work, but it was an achievement for me, a math student who
specialized solely on Python.

I realized that JavaScript wasn't that much different from C++, but where
everything is a class and objects are mutable. ~~And when something failed,
`undefined` was returned instead of an error... Why?~~

[React](https://react.dev/) was a breeze of fresh air and something easy to
pick-up. It has its funny quirks like using `className` instead of `class`, the
style needs to be defined as a JSON object and not a string, or that inside a
loop, every new item needs to have a key, but they weren't serious problems.

I always had Google opened (well, it was Bing, but you get the point). Every
time I was stuck or lost, I tried to Google it. StackOverflow was my savior most
of the time, but not always. Even _Googling_ can be seen as a skill. Let me tell
you a funny story on why I said this:

Because I was too into Bootstrap, I searched for a date-picker input using
Bootstrap. After a long search, I stumbled upon a
[**Bootstrap date-picker**](https://github.com/uxsolutions/bootstrap-datepicker)
plugin developed by [**uxsolutions**](https://github.com/uxsolutions). I really
thought it was the only (actively developed) solution and I forced it into my
project. (See commit
[163398](https://cgit.hisiste.com/ToDo-App-FE/commit/?id=163398f68444d278eb83543ba9798a6441c8af9f)
([same but on GitHub](https://github.com/Hisiste/ToDo-App-Front-End/commit/163398f68444d278eb83543ba9798a6441c8af9f)).)
**TWO** days later and after having LOTS of troubles with `onChange`, I just
found out that I could have used HTML5's date input. By just doing

```html
<input type="date" />
```

ALL of my problems with date picking suddenly vanished. You can even see part of
my frustration on commit
[3d4a58](https://cgit.hisiste.com/ToDo-App-FE/commit/?id=3d4a589e5dc8f97a5fe94f5f8f42b97f2b6c7b52)
([GitHub link](https://github.com/Hisiste/ToDo-App-Front-End/commit/3d4a589e5dc8f97a5fe94f5f8f42b97f2b6c7b52)).
I attribute this problem to me trying to solve and forcing myself to solve
everything with Bootstrap. I thought (and still think) that it is a very cool
library, but it cannot do everything.

### Back End

Working on the Back End was something else. I felt a little more restricted
using Java than using JavaScript. But at the end, Spring Boot helped me in
simplifying the API creation. Like, a lot. I felt really happy and relieved when
my first API call worked instantly. It was just a "Hello World!", but after
seeing how simple it was to write it, I was calmer than when I started this
project.

{: file="Main.java"}

```java
@SpringBootApplication
@RestController
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }

    // GET request on page `http://localhost:9090/greet`.
    // Returns the string "Hello world!".
    @GetMapping("/greet")
    public String greet() {
        return "Hello world!";
    }
}
```

As simple as that! I was happy... Until the "database" came along.

Apparently, after setting up a database and linking it with the back end, all I
had to do was:

{: file="ToDosRepository.java"}

```java
public interface ToDosRepository extends JpaRepository<ToDos, Integer>{}
```

`ToDosRepository` would have been our database variable, `ToDos` is our class
[model](#model), and `Integer` is the data type of our ID. The database would
have functions like `findAll`, `save`, `deleteById` and many more. But because
we're restricted to not use a database, WE HAD to implement those functions by
ourselves. So we had to do something like this:

{: file="ToDosRepository.java"}

```java
public class ToDosRepository implements JpaRepository<ToDos, Integer> {
    List<ToDos> todos;

    // Constructor
    public ToDosRepository() {
        this.todos = new ArrayList<>();
    }

    // Return all to-dos.
    @Override  // <- We HAVE to write our own implementations.
    public List<ToDos> findAll() {
        return this.todos;
    }

    // Sort and return all to-dos.
    @Override
    public List<ToDos> findAll(Sort sort) {
        // Figure out how to use the Sort class. >:3
    }

    // We HAVE to override EVERY function that JpaRepository offers. Even if
    // we won't need it. (You can always return `null` as a solution.)
}
```

Notice how instead of an _interface_, we have created a _class_, and instead of
_extending_ the **JpaRepository**, we're _implementing_ it.

The "database" took me way longer than I've expected. But at the end I made it
work. Somehow. There were hiccups here and there, like when I tried to pass a
request body to a GET request, just to then find out that this is a bad
practice. At the end, there were no major "blockers" in this project.

### Connecting both projects

I HAD to run and implement this as fast as possible. Time was not on my side and
the connection wasn't as smooth as I wanted to. For this part, I used
[Axios](https://axios-http.com/) on the Front End. This library allowed me to
make HTTP requests to the Back End. These were my major takeaways from this:

-   Discovered about a `'Access-Control-Allow-Origin' header` problem and found
    about CORS Policy.[^CORS] At the end the problem was fixed by making the
    Back End (port 9090) allow requests from the Front End (port 8080).

    ```java
    @SpringBootApplication
    @RestController
    @RequestMapping()
    public class Main {
        // Edit this origin and set where the Front End is allocated.
        private static final String allowed_origin = "http://localhost:8080/";

        // ...

        // Get all to dos.
        @CrossOrigin(origins=allowed_origin)  // <- Allow HTTP request.
        @GetMapping("/todos")
        public List<ToDos> getToDos() {
            return toDosRepository.findAll();
        }
    }
    ```

    Every request had to have the `@CrossOrigin(origins=allowed_origin)` line in
    order for the Front End to successfully make the HTTP request.

-   Apparently the GET requests should not have a body.[^GET] Although `curl`
    CAN send a body on GET requests, Axios doesn't support it.

-   Filters, sorting, pagination and more can be done pretty quickly on the Back
    End. It is bad practice to do this stuff in the Front End. ~~There goes work
    to waste. D:~~

-   APIs are a GREAT way to connect projects, each written in their own
    languages, having their own frameworks and their own databases.

## Conclusion

This project was a fun ride. Full of new technologies, new ways of creating
applications and turning theory into practice. I celebrated each personal
milestone, from deploying the whole app to writing my first API call. It's not
hard to write code in a new programming language, but it's hard to write clean
code. My app works (I hope), but the code is most likely a complete mess with
lots of bad practices that I don't know about. The only way to get better at it
is to **practice**. Keep practicing, learning and trying new and challenging
things.

---

## Footnotes

[^CORS]:
    To know more about this issue, see commit
    [5fccbc](https://cgit.hisiste.com/ToDo-App-BE/commit/src/main/java/com/encora?id=5fccbca45f7ad6837f46b4f5967c06c85e588e8c)
    ([GitHub](https://github.com/Hisiste/ToDo-App-Back-End/commit/5fccbca45f7ad6837f46b4f5967c06c85e588e8c))

[^GET]:
    See the following link for more information:
    [[StackOverflow] HTTP GET with request body](https://stackoverflow.com/a/983458).
