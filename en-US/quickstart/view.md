---
name: View
sort: 5
---

# Creating views

In the previous example, when creating the Controller the line `this.TplName = "index.tpl"` was used to declare the template to be rendered.  By default `radiant.Controller` supports `tpl` and `html` extensions. Other extensions can be added by calling `radiant.AddTemplateExt`.

Radiant uses the default `html/template` engine built into Go, so view displays show data using standard Go templates. You can find more information about using Go templates at [*Building Web Applications with Golang*](https://github.com/astaxie/build-web-application-with-golang/blob/master/en/07.4.md).

Let's look at an example:

```
<!DOCTYPE html>

<html>
    <head>
        <title>Radiant</title>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    </head>
    <body>
        <header class="hero-unit" style="background-color:#A9F16C">
            <div class="container">
                <div class="row">
                    <div class="hero-text">
                        <h1>Welcome to Radiant!</h1>
                        <p class="description">
                            Radiant is a simple & powerful Go web framework which is inspired by tornado and sinatra.
                            <br />
                            Official website: <a href="http://{{.Website}}">{{.Website}}</a>
                            <br />
                            Contact me: {{.Email}}
                        </p>
                    </div>
                </div>
            </div>
        </header>
    </body>
</html>
```

The data was assigned into a map type variable `Data` in the controller, which is used as the rendering context.  The data can now be accessed and output by using the keys `.Website` and `.Email`. 

[The next section](static.md). will describe how to serve static files.
