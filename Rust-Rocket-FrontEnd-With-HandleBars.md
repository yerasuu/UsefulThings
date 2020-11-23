## Templating Basics

First i need to explain basics of templates, at this, i supose the you know the basics of html templates.

```hbs
<html>
  <head></head>
  <body>
    <p> Welcome to the site {{name}}!</p>
  </body>
</html>
```

When you use handlebars you have many basics you need know, loops, insertions and writers.

To write the value of a var into the template you use this style:
```hbs
{{var_name}}
```

To to use a loop you this:
```hbs
{{#each items}}
{{/each}}
```

## How To Return A Template

To return a template web use this basic:

```rust
use rocket_contrib::templates::Template;

// the get function of the macro refer the route this function will work,
//you need to insert this inside the routes macro of the main function too.
#[get("/")]
fn index() -> Template {
    let context: HashMap<&str, &str> = [("name", "Jonathan")]
        .iter().cloned().collect();
    Template::render("index", &context)
}
// in this example we use an hashmap to insert the values, and web send it to the Template render as we see above.

fn main() {
  rocket::ignite()
    .mount("/", routes![index]) //<- insert here the function name only,
                                //you can add more than one if you have multiple routes of it receive params
    .attach(Template::fairing())
    .launch();
}
```
