---
name: Session Module
sort: 1
---

# Notes:

* This document is for using `session` as a standalone module in other projects. If you are using `session` with Radiant, please check here [session control](../mvc/controller/session.md)*

# Introduction to Session Module

The session module is used to store user data between different requests. It only supports saving the session id into a cookie, so if the client doesn't support cookies, it won't work.

It is inspired by `database/sql`, which means: one interface, multiple implementations. By default it supports four saving providers: memory, file, redis and mysql.

Install session module:

	go get github.com/W3-Engineers-Ltd/Radiant/server/web/session

## Basic Usage:

Import package first:

	import (
		"github.com/W3-Engineers-Ltd/Radiant/server/web/session"
	)

Then initialize a global variable as the session manager:

	var globalSessions *session.Manager

Then initialize data in your main function:

	func init() {
		globalSessions, _ = session.NewManager("memory", `{"cookieName":"gosessionid", "enableSetCookie,omitempty": true, "gclifetime":3600, "maxLifetime": 3600, "secure": false, "sessionIDHashFunc": "sha1", "sessionIDHashKey": "", "cookieLifeTime": 3600, "providerConfig": ""}`)
		go globalSessions.GC()
	}

Parameters of NewManager:

1. Saving provider name: memory, file, mysql, redis
2. A JSON string that contains the config information.
	1. cookieName: Cookie name of session id saved on the client
	2. enableSetCookie, omitempty: Whether to enable SetCookie, omitempty
	3. gclifetime: The interval of GC.
	4. maxLifetime: Expiration time of data saved on the server
	5. secure: Enable https or not. There is `cookie.Secure` while configure cookie.
	6. sessionIDHashFunc: SessionID generator function. `sha1` by default.
	7. sessionIDHashKey: Hash key.
	8. cookieLifeTime: Cookie expiration time on the client. 0 by default, which means life time of browser.
	9. providerConfig: Provider-specific config. See below for more information.

Then we can use session in our code:

	func login(w http.ResponseWriter, r *http.Request) {
		sess, _ := globalSessions.SessionStart(w, r)
		defer sess.SessionRelease(w)
		username := sess.Get("username")
		if r.Method == "GET" {
			t, _ := template.ParseFiles("login.gtpl")
			t.Execute(w, nil)
		} else {
			sess.Set("username", r.Form["username"])
		}
	}

Here are the methods of globalSessions:

- `SessionStart` Return session object based on current request.
- `SessionDestroy` Destroy current session object.
- `SessionRegenerateId` Regenerate a new sessionID.
- `GetActiveSession` Get active session user.
- `SetHashFunc` Set sessionID generator function.
- `SetSecure` Enable Secure cookie or not.

The returned session object is a Interface. Here are the methods:

- `Set(key, value interface{}) error`
- `Get(key interface{}) interface{}`
- `Delete(key interface{}) error`
- `SessionID() string`
- `SessionRelease()`
- `Flush() error`

## Saving Provider Config

We've already seen configuration of `memory` provider. Here is the configuration of the others:

- `mysql`:

	All the parameters are the same as memory's except the fourth parameter, e.g.:

		username:password@protocol(address)/dbname?param=value

	For details see the [go-sql-driver/mysql](https://github.com/go-sql-driver/mysql#dsn-data-source-name) documentation.

- `redis`:

	Connection config: address,pool,password

		127.0.0.1:6379,100,astaxie

- `file`:

	The session save path. Create new files in two levels by default.  E.g.: if sessionID is `xsnkjklkjjkh27hjh78908` the file will be saved as `./tmp/x/s/xsnkjklkjjkh27hjh78908`

		./tmp

## Creating a new provider

Sometimes you need to create your own session provider. The Session module uses interfaces, so you can implement this interface to create your own provider easily.

```go
// Store contains all data for one session process with specific id.
type Store interface {
	Set(ctx context.Context, key, value interface{}) error     //set session value
	Get(ctx context.Context, key interface{}) interface{}      //get session value
	Delete(ctx context.Context, key interface{}) error         //delete session value
	SessionID(ctx context.Context) string                      //back current sessionID
	SessionRelease(ctx context.Context, w http.ResponseWriter) // release the resource & save data to provider & return the data
	Flush(ctx context.Context) error                           //delete all data
}

// Provider contains global session methods and saved SessionStores.
// it can operate a SessionStore by its id.
type Provider interface {
	SessionInit(ctx context.Context, gclifetime int64, config string) error
	SessionRead(ctx context.Context, sid string) (Store, error)
	SessionExist(ctx context.Context, sid string) (bool, error)
	SessionRegenerate(ctx context.Context, oldsid, sid string) (Store, error)
	SessionDestroy(ctx context.Context, sid string) error
	SessionAll(ctx context.Context) int // get all active session
	SessionGC(ctx context.Context)
}
```

Finally, register your provider:

	func init() {
		// ownadapter is an instance of session.Provider
		session.Register("own", ownadapter)
	}
