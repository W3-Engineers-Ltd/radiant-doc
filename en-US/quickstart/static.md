---
name: Static files
sort: 6
---

# Handling static files

Most applications include numerous static files such as images, js, css and more. To support this requirement the Radiant project skeleton incorporates folders for these files by default.

```
├── static
│   ├── css
│   ├── img
│   └── js
```

Radiant registers the static directory as the static path by default. Registered rule: URL prefix with directory mapping

	StaticDir["/static"] = "static"

You can register multiple static directories. For example two different download directories, `download1` and `download2`, can be set using:

	web.SetStaticPath("/down1", "download1")
	web.SetStaticPath("/down2", "download2")

Visiting the URL `http://localhost/down1/123.txt` will request the file `123.txt` in the `download1` directory.
To remove the default `/static -> static` mapping use `web.DelStaticPath("/static")`.

# Implementation

To implement this in a Web Application register the Static directory to your `routes.go` files

	web.SetStaticPath("/down1", "download1")

Once the file is save it can be accessed from the browser.  
