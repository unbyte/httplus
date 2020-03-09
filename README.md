# HTTPLUS (http plus)

![go version](https://img.shields.io/badge/Go-1.14-blue?style=flat-square)

> HTTPLUS provides is compatible with the original `net/http` but provides more functions

## Usage

Just replace origin `net/http` with this library.

And replace this library with [this](https://github.com/unbyte/httplus/tree/origin) to return to the original.

## Functions

### Custom Status Code and Text

#### Globally

```go
http.AddGlobalCustomStatus(code int,text string)
```

Global custom status override any defined status, including standard status

##### Example
```go
func main() {
	g := gin.Default()
	http.AddGlobalCustomStatus(200, "nice")
	g.GET("/test", func(context *gin.Context) {
		context.String(200, "")
	})
	g.Run(":12398")
}
```

#### Single Response

first you should enable the rule for custom status of single response

```go
http.EnableSingleCustomRule(true)
```

then set custom status

```go
http.AddCustomStatus(code int, text string)
```

and the rules are as follows

- for status in `[100..102]` or `[200..207]` or `[300..307]` or `[500..510]` or `[600]`
   
  you should add 20 to the original status.
  
  e.g. `220` means use status code `200` and corresponding custom text

- status in `[400..451]`
 
  you should add 240 to the original status.
  
  e.g. `640` means use status code `400` and corresponding custom text

> Don't let your global custom status be in 
`[120..122]` `[220..227]` `[320..327]` `[520..530]` `[620]` `[640..691]` 
if you want to control single response status text at the same time, 
because global custom status will override single custom status

##### Example
```go
func main() {
	g := gin.Default()
	http.EnableSingleCustomRule(true)
	http.AddCustomStatus(200, "nice")
	g.GET("/normal", func(context *gin.Context) {
		context.String(200, "")
	})
	g.GET("/custom", func(context *gin.Context) {
		context.String(220, "")
	})
	g.Run(":12398")
}
```

## License

MIT License.