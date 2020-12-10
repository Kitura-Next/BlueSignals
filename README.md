<p align="center">
<a href="http://kituranext.org/">
<img src="https://raw.githubusercontent.com/Kitura-Next/Kitura/master/Sources/Kitura/resources/kitura-bird.svg?sanitize=true" height="100" alt="Kitura">
</a>
</p>

<p align="center">
    <a href="https://www.kituranext.org/learn/">
    <img src="https://img.shields.io/badge/docs-kitura-1FBCE4.svg" alt="APIDoc"></a>
    <a href="https://github.com/Kitura-Next/LoggerAPI/actions?query=workflow%3ASwift+MacOS">
    <img src="https://github.com/Kitura-Next/LoggerAPI/workflows/Swift%20MacOS/badge.svg"></a>
    <a href="https://github.com/Kitura-Next/LoggerAPI/actions?query=workflow%3ASwift+Ubuntu">
    <img src="https://github.com/Kitura-Next/LoggerAPI/workflows/Swift%20Ubuntu/badge.svg"></a>
    <img src="https://img.shields.io/badge/license-Apache2-blue.svg?style=flat" alt="Apache 2">
    <a href="http://swift-at-ibm-slack.mybluemix.net/">
    <img src="http://swift-at-ibm-slack.mybluemix.net/badge.svg" alt="Slack Status"></a>
</p>

# Signals

Generic Cross Platform Signal Handler.

## Prerequisites

### Swift version
Requires **Swift 5.1** or newer. You can download the Swift binaries by following this [link](https://swift.org/download/).
Compatibility with other Swift versions is not guaranteed.

### macOS

* macOS 10.14 or higher.
* Xcode Version 11.3 or higher using the included toolchain.

### Linux

* Ubuntu tested on 18.04.
* One of the Swift Open Source toolchain listed above.

## Build

To build Signals from the command line:

```
% cd <path-to-clone>
% swift build
```

## Using Signals

### Including in your project

#### Swift Package Manager

To include BlueSignals into a Swift Package Manager package, add it to the `dependencies` attribute defined in your `Package.swift` file. You can select the version using the `majorVersion` and `minor` parameters. For example:
```
	dependencies: [
		.Package(url: "https://github.com/Kitura-Next/BlueSignals.git", majorVersion: <majorVersion>, minor: <minor>)
	]
```

#### Carthage
To include BlueSignals in a project using Carthage, add a line to your `Cartfile` with the GitHub organization and project names and version. For example:
```
	github "Kitura-Next/BlueSignals" ~> <majorVersion>.<minor>
```

#### CocoaPods
To include BlueSignals in a project using CocoaPods, you just add `BlueSignals` to your `Podfile`, for example:
```
    platform :ios, '10.0'

    target 'MyApp' do
        use_frameworks!
        pod 'BlueSignals'
    end
```

### Before starting

The first thing you need to do is import the Signals framework.  This is done by the following:
```
import Signals
```

### Provided APIs

Signals provides four (4) class level APIs.  Three (3) are used for trapping and handling operating system signals.  The other function allows for the raising of a signal.

#### Trapping a signal
- `trap(signal signal: Signal, action: SigActionHandler)` - This basic API allows you to set and specific handler for a specific signal.

The example below shows how to add a trap handler to a server in order to perform and orderly shutdown in the event that user press `^C` which sends the process a `SIGINT`.
```swift
import Signals

...

let server: SomeServer = ...

Signals.trap(signal: .int) { signal in

	server.shutdownServer()
}

server.run()
```
Additionally, convenience API's that build on the basic API specified above are provided that will allow for trapping multiple signals, each to a separate handler or to a single handler.
- `trap(signals signals: [(signal: Signal, action: SigActionHandler)])` - This lets you trap multiple signals to separate handlers in a single function call.
- `trap(signals signals: [Signal], action: SigActionHandler)` - This API lets you trap multiple signals to a common handler.

#### Raising a signal
- `raise(signal signal: Signal)` - This API is used to send an operating system signal to your application.

This example illustrates how to use Signals to raise a signal with the OS, in this case `SIGABRT`.
```swift
import Signals

...

Signals.raise(signal: .abrt)
```

#### Ignoring a signal
- `func ignore(signal: Signal)` - This API is used to ignore an operating system signal.

This example illustrates how to use Signals to ignore a signal with the OS, in this case `SIGPIPE`.
```swift
import Signals

...

Signals.ignore(signal: .pipe)
```

#### Restoring a signals default handler
- `func restore(signal: Signal)` - This API is used to restore an operating system signals default handler.

This example illustrates how to use Signals to restore a signals default handler, in this case `SIGPIPE`.
```swift
import Signals

...

Signals.restore(signal: .pipe)
```

#### Adding a USER-DEFINED signal

This example shows how to add a user defined signal, add a trap handler for it and then raise the signal.
```swift
import Signals

let mySignal = Signals.Signal.user(20)

Signals.trap(signal: mySignal) { signal in

	print("Received signal \(signal)")
}

Signals.raise(signal: mySignal)

```
The output of the above snippet is:
```
Received signal 20
```

## Community

We love to talk server-side Swift and Kitura. Join our [Slack](http://swift-at-ibm-slack.mybluemix.net/) to meet the team!

## License

This library is licensed under Apache 2.0. Full license text is available in [LICENSE](https://github.com/Kitura-Next/BlueSignals/blob/master/LICENSE).
