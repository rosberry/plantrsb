# plantrsb
Provides an extended plantuml preprocessing functionality to adopt diagrams for different platforms.

## Get Started

This framework overrides standart plantuml syntax. Instead of using arrow syntax to send message from one instance to other, it is required to use accessor to according responsibility levevel. There are the following responcipility levels:

- $App()
- $User()
- $Screen()
- $Logic()

### $App()

Defines the interaction of an application with the system. Currently it can handle the following messages:

- $App(launch) - the main entry point into application.
- $App(show <Screen>) - show specified screen as the root screen of application.
- $App(finish <Screen>) - finish specified screen.

Missing functionality:
- Handle background and foreground app states
- Handle remote notifications

### $User()

Defines an interaction with an application. Currently it can handle the following messages:

- $User(finish App)

### $Screen()

Defines the functionality of `App` or `User` interaction with the screen.  Currently it can handle the following messages:

- $Screen(<name>,  tap on <action> <element>) - defines <action> handler when `User` taps an <element> on the screen with <name>
- $Screen(<name>, show <child>) - show <child> screen from screen with <name>
- $Screen(<name>, end tap) - end last tap action handling on the screen with <name>
- $Screen(<name>, finish <child>) - finish <child> screen and return to screen with <name>

### $Logic

Defines the functionality of `App` or `Screen` interaction with business logic of the application. Currently it can handle the following messages:

- $Logic(<instance>,<action>) - initiates interaction of <instance> with business logic to perform <action>. Instance can be `App` or `Screen`.
- $Logic(<instance>, fail <action>) - returns negative result of <action> to <instance>.
- $Logic(<instance>, success <action>) - returns positive result of <action> to <instance>.
- $Logic(<instance>, end <action>) - end performing <action>.

### Customization

Add `app.uml` file and scpecify `$CONFIG` variable:

```
@startuml

!$CONFIG = {
  "app": {
    "color": "#<color>"
  },
  "screen": {
    "color": "#<color>"
  },
  "screens": [
    ...
    {"name": "<name>", "color": "#<color>"},
    ...
  ]
}

@enduml
```

Here "app" customize appearance of `App` responcibility level, "screen" - default appearance of any screen, "screens" - appearence and order of concrete screen with <name>.

### Installation

Make sure you have installed [plantuml](https://plantuml.com/ru/starting).
Then just put `rsb` folder of this repo in the same folder with your diagram and include `main` file:
```
!include rsb/main.uml   
```

### Example

```
@startuml

!include rsb/main.uml

== Launch ==
$App(launch)

$App(show Library)

== Subscription ==
group Subscription
$Screen(Library, tap on Subscription Button)
$Screen(Library, show Subscription)
$Screen(Library, end tap)

$Screen(Subscription, tap on Purchase Button)

$Logic(Subscription, purchase subscription)
$Screen(Subscription, end tap)
$Logic(Subscription, fail purchase subscription)
$Screen(Subscription, show Error)
$Screen(Subscription, finish Error)
$Logic(Subscription, success purchase subscription)
$Logic(Subscription, end purchase subscription)
$Screen(Library, finish Subscription)

== Finish ==
$App(finish Library)
$User(finish App)

@enduml
```

## Authors

* Nikolay Tyunin, nikolay.tyunin@rosberry.com

## About

<img src="https://github.com/rosberry/Foundation/blob/master/Assets/full_logo.png?raw=true" height="100" />

This project is owned and maintained by [Rosberry](http://rosberry.com). We build mobile apps for users worldwide 🌏.

Check out our [open source projects](https://github.com/rosberry), read [our blog](https://medium.com/@Rosberry) or give us a high-five on 🐦 [@rosberryapps](http://twitter.com/RosberryApps).

## License

The project is available under the MIT license. See the LICENSE file for more info.
