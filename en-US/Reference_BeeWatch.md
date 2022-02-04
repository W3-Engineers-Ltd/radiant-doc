# Radical Watch

[![Build Status](https://drone.io/github.com/radiant/radicalwatch/status.png)](https://drone.io/github.com/radiant/radicalwatch/latest)

Radical Watch is an interactive debugger for the Go programming language.

## Features

- Use `Critical`, `Info` and `Trace` three levels to change debugger behavior.
- `Display()` variable values or `Printf()` with customized format.
- User-friendly Web UI for **WebSocket** mode or easy-control **command-line** mode.
- Call `AddWatchVars()` to monitor variables and show their information when the program calls `Break()`.
- Configuration file with your customized settings(`radicalwatch.json`).

## Installation

Radical Watch is a "go get" able Go project, you can execute the following command to auto-install:

	go get github.com/radiant/radicalwatch

**Attention** This project can only be installed by source code now.

## Quick start

### Usage

	package main

	import (
		"time"

		"github.com/radiant/radicalwatch"
	)

	func main() {
		// Start with default level: Trace,
		// or use `radicalwatch.Start(radicalwatch.Info())` to set the level.
		radicalwatch.Start()

		// Some variables.
		appName := "Radical Watch"
		boolean := true
		number := 3862
		floatNum := 3.1415926
		test := "fail to watch"

		// Add variables to be watched, must be even sized.
		// Note that you have to pass the pointer.
		// For now it only supports basic types.
		// In this case, 'test' will not be watched.
		radicalwatch.AddWatchVars("test", test, "App Name", &appName,
			"bool", &boolean, "number", &number, "float", &floatNum)

		// Display something.
		radicalwatch.Info().Display("App Name", appName).Break().
			Printf("boolean value is %v; number is %d", boolean, number)

		radicalwatch.Critical().Break().Display("float", floatNum)

		// Change some values, must be even sized.
		appName = "Radical Watch2"
		number = 250
		// Here you will see something interesting.
		radicalwatch.Trace().Break()

		// Multiple goroutines test.
		for i := 0; i < 3; i++ {
			go multipletest(i)
		}

		radicalwatch.Trace().Printf("Wait 3 seconds...")
		select {
		case <-time.After(3 * time.Second):
			radicalwatch.Trace().Printf("Done debug")
		}
	
		// Close debugger,
		// it's not useful when you debug a web server that may not have an end.
		radicalwatch.Close()
	}

	func multipletest(num int) {
		radicalwatch.Critical().Break().Display("num", num)
	}

### Connect

Radical Watch debugger is automatically started on [http://localhost:23456](http://localhost:23456) when you use **WebSocket** mode, you can change port and other configuration by editing `radicalwatch.json`(copy default setting from Radical Watch source folder).

Your browser has to support WebSocket, it has radicaln tested with Chrome, Safari and Firefox on Mac and Windows.

![Radical Watch demo](https://github.com/radiant/radicalwatch/blob/master/tests/images/demo_radicalwatch.png?raw=true)

## Examples and API documentation

[Go Walker](http://gowalker.org/github.com/radiant/radicalwatch)

