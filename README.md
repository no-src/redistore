# redistore

[![Build](https://img.shields.io/github/actions/workflow/status/no-src/redistore/go.yml?branch=main)](https://github.com/no-src/redistore/actions)
[![License](https://img.shields.io/github/license/no-src/redistore)](https://github.com/no-src/redistore/blob/main/LICENSE)
[![Go Reference](https://pkg.go.dev/badge/github.com/no-src/redistore.svg)](https://pkg.go.dev/github.com/no-src/redistore)
[![Go Report Card](https://goreportcard.com/badge/github.com/no-src/redistore)](https://goreportcard.com/report/github.com/no-src/redistore)
[![codecov](https://codecov.io/gh/no-src/redistore/branch/main/graph/badge.svg?token=pDtX3R6teh)](https://codecov.io/gh/no-src/redistore)
[![Release](https://img.shields.io/github/v/release/no-src/redistore)](https://github.com/no-src/redistore/releases)

A session store backend
for [gorilla/sessions](http://www.gorillatoolkit.org/pkg/sessions) - [src](https://github.com/gorilla/sessions).

The redistore project is a fork of [redistore](https://github.com/boj/redistore).
The purpose of this fork is to replace the [redigo](https://github.com/gomodule/redigo)
with [go-redis](https://github.com/redis/go-redis) as the driver of redis store.

## Requirements

Depends on the [go-redis](https://github.com/redis/go-redis) Redis library.

## Installation

```bash
go get -u github.com/no-src/redistore
```

## Documentation

Available on [pkg.go.dev](https://pkg.go.dev/github.com/no-src/redistore).

See https://github.com/gorilla/sessions for full documentation on underlying interface.

### Example

``` go
// Fetch new store.
store, err := NewRediStore(10, "tcp", ":6379", "", []byte("secret-key"))
if err != nil {
	panic(err)
}
defer store.Close()

// Get a session.
session, err = store.Get(req, "session-key")
if err != nil {
	log.Error(err.Error())
}

// Add a value.
session.Values["foo"] = "bar"

// Save.
if err = sessions.Save(req, rsp); err != nil {
	t.Fatalf("Error saving session: %v", err)
}

// Delete session.
session.Options.MaxAge = -1
if err = sessions.Save(req, rsp); err != nil {
	t.Fatalf("Error saving session: %v", err)
}

// Change session storage configuration for MaxAge = 10 days.
store.SetMaxAge(10 * 24 * 3600)
```
