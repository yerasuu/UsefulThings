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
```hbs{{var_name}}```

To to use a loop you this:
```hbs
{{#each items}}
{{/each}}
```
