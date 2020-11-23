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
Now before we return the HTML to the user, we want to perform a substitution. Where we find the variable {{name}}, we should replace it with the user's name, which our server should know.
There are many different libraries that do this, often through Javascript. But in Rust, it turns out the Rocket library has a couple easy templating integrations. One option is Tera, which was specifically designed for Rust. Another option is Handlebars, which is more native to Javascript, but also has a Rocket integration. The substitutions in this article are simple, so there’s not actually much of a difference for us.
