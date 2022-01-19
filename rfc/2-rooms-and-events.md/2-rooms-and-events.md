# Events and Rooms

## Summary

This RFC proposes a model of sharing data in the ForgeFlux protocol
through a concept of rooms and events. A room can be subscribed to,
unsubscribed from and can contain nested rooms, which themselves are
fully capable as parent rooms. Events are used to describe various types
of room activity.

The mechanism by which an interface decides to present the events of
rooms on it's managed forge is left to the discretion of the interface
implementers.

## Requirements

This protocol must support the following features which are required for
to enable comfortable development workflows:

-   Facilitate sending and receiving patches or pull requests
-   Review contributions via pull requests or patches or any other method
    supported by Git.
-   Take part in discussions
-   Observe contributions and discussions without active participation

Interface implementers should ensure these requirements are met and
exceptions should only be made when their forge isn't capable of meeting
the above requirement. For instance,
[cgit](https://git.zx2c4.com/cgit/about/)-SSH forge isn't capable of bug
tracking and therefore the implementer can choose to not meet said
requirement.

## Events

Data stored in rooms are expressed as events. Events are atomic and are
synchronised against `main forge`. Race conditions are possible when
events are first committed on non-main forges and are then committed on
the `main forge`, eventual consistency is aimed for in such cases.

### Naming conventions

Event values must be uniquely globally namespaced following Java's
[package naming
conventions](https://en.wikipedia.org/wiki/Java_package#Package_naming_conventions),
e.g. `com.example.com.myapp.event`. The special top-level namespace `f.`
is resolved for events resolved in the ForgeFlux specification - for
instance `f.event.patch`.

## Rooms

Rooms are the basic channels of communication in the ForgeFlux protocol.
All forge-activity should be modeled based on rooms. Rooms are
identified based on the `main forge` but all `main interfaces` should be
capable of accommodating subscribing non-native interfaces.

### Multilocation

Multilocation is a [multiformat](https://multiformats.io/multihash/)
identifier that self-describes the room and is capable of nesting. Each
room is identified by its unique multilocation. The format is as
follows:

```
<forge public URL>/<repository name>/<room-type>/<public URL of room>
```

When a room is nested, it can have the following format:

```
<Parent Multilocation>/<Child Multilocation>
```

Public URL of the room is made part of this format because it might be
helpful for non-native users when events are poorly rendered on their
forge. With a public URL, they will be able to see the events in their
native environment.

### Types

Room type is used to differentiate rooms with different capabilities.
Types values must be uniquely globally namespaced following Java's
[package naming
conventions](https://en.wikipedia.org/wiki/Java_package#Package_naming_conventions),
e.g. `com.example.com.myapp.room_type`. The special top-level namespace `f.`
is resolved for types resolved in the ForgeFlux specification - for
instance `f.room.contributions`.

## References

-   Rooms and events are heavily inspired by the [Matrix
    protocol](https://matrix.org/docs/spec/#architecture)
-   Multilocation is inspired by the
    [Multiformat](https://multiformats.io/) project
