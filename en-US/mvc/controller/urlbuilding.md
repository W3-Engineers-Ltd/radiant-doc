---
name: URL Building
sort: 7
---

## URL Building
If it can match URLs, can Radiant also generate them? Of course it can. To build a URL to a specific function you can use the URLFor() function. It accepts the name of the function of Controller as first argument and a number of keyword arguments, each corresponding to the variable part of the URL rule. Unknown variable parts are appended to the URL as query parameters. Here are some examples:

Here is the controller definition:

```
type TestController struct {
	web.Controller
}

func (this *TestController) Get() {
	this.Data["Username"] = "astaxie"
	this.Ctx.Output.Body([]byte("ok"))
}

func (this *TestController) List() {
	this.Ctx.Output.Body([]byte("i am list"))
}

func (this *TestController) Params() {
	this.Ctx.Output.Body([]byte(this.Ctx.Input.Params["0"] + this.Ctx.Input.Params["1"] + this.Ctx.Input.Params["2"]))
}

func (this *TestController) Myext() {
	this.Ctx.Output.Body([]byte(this.Ctx.Input.Param(":ext")))
}

func (this *TestController) GetUrl() {
	this.Ctx.Output.Body([]byte(this.URLFor(".Myext")))
}
```

This is how you register the router:

```
web.Router("/api/list", &TestController{}, "*:List")
web.Router("/person/:last/:first", &TestController{})
web.AutoRouter(&TestController{})
```

This is how you generate the url:

```
web.URLFor("TestController.List")
// Output /api/list

web.URLFor("TestController.Get", ":last", "xie", ":first", "asta")
// Output /person/xie/asta

web.URLFor("TestController.Myext")
// Output /Test/Myext

web.URLFor("TestController.GetUrl")
// Output /Test/GetUrl
```

## This is how you use it in a template
radiant has already registered the template function `urlfor`. You can use it like this:

```
{{urlfor "TestController.List"}}
// Output /api/list

{{urlfor "TestController.Get" ":last" "xie" ":first" "asta"}}
// Output /person/xie/asta
```
	
Why would you want to build URLs instead of hard-coding them into your templates? There are three good reasons for this:

1. Reversing is often more descriptive than hard-coding the URLs. More importantly, it allows you to change URLs in one go, without having to remember to change URLs all over the place.
2. URL building will handle escaping of special characters and Unicode data transparently for you, so you don???t have to deal with them.
