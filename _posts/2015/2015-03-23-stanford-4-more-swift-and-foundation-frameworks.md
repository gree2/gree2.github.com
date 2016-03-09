---
layout: post
title: "stanford 4 more swift and foundation frameworks"
description: ""
category: [stanford]
tags: [swift]
---
{% include JB/setup %}

### optional

1. optional is just an enum

        enum Optional<T>{
            case None
            case Some(T)
        }

1. usage of nil

        let x: String? = nil
        ... is ...
        let x = Optional<String>.None

1. usage of not nil

        let x: String? = "hello"
        ... is ...
        let x = Optional<String>.Some("hello")

1. unwrap an optional value

        var y = x!
        ... is ...
        switch x {
            case Some(let value): y = value
            case None: // raise an exception
        }

### array

1. define

        var a = Array<String>()
        var a = [String]()

1. immutable array

        let animals = ["Giraffe", "Cow", "Doggie", "Bird"]
        // won't compile
        animals.append("Ostrich")

1. index out of bounds

        // crash
        let animal = animals[5]

1. enumerating an Array

        for animal in animals{
            println("\(animal)")
        }

### dictionary

1. define

        var rankings = Dictionary<String, Int>()
        var rankings = [String: Int]()

1. usage

        rankings = ["stanford":1, "Cal":10]
        // ranking is an Int? would be nil
        let ranking = rankings["Ohio State"]

1. enumerating a Dictionary

        for (key, value) in rankings{
            println("\(key) = \(value)")
        }

### range

1. is just two end points of a sensible type

        Range is generic
        pseudo-representation of Range
        struct Range<T>{
            var startIndex: T
            var endIndex: T
        }

1. an Array's range would be a `Range<Int>`

        since Arrays are indexed by Int

1. a String subrange is not Range<Int>

        it is Range<String.Index>

1. inclusive and open-ended range

        ... inclusive
        ..< open-ended

        let array = ["a", "b", "c", "d"]
        let sub1 = array[2...3] // ["c", "d"]
        let sub2 = array[2..<3] // ["c"]
        for i in [27...104]{ // some code }

### other classes

1. NSObject

        base class for all objective-c class
        some advanced features will require you to subclass from it

1. NSNumber

        generic number-holding class
        let n = NSNumber(35.5)
        let intVersion = n.intValue // also doubleValue, boolValue, etc.

1. NSDate

        find out date and time right now, past or future
        NSCalendar, NSDateFormatter, NSDateComponents

1. NSData

        a "bag o' bits"
        used to save/restore/transmit raw data throughout the iOS SDK

### data structures in swift

1. classes, structures and enumerations

        these are the 3 fundamental building blocks of data structures in swift

1. similarities

        declaration syntax
        properties and functions
        initializers (not enum)

1. differences

        inheritance (class only)
        introspection and casting (class only)
        value type (struct, enum) vs. reference type (class)

### value vs. reference

1. value (struct and enum)

        copied when passed as an argument to a function
        copied when assigned to a different variable
        immutable if assigned to a variable with let
        func parameters are by default constants
        put var on a parameter it will be mutable, but still a copy
        func can mutate a struct/enum with the keyword mutating

1. reference (class)

        stored in the heap and reference counted (automatically)
        constant pointers to a class (let) still can mutate by calling methods and changing properties
        when passed as an argument, does not make a copy (just passing a pointer to same instance)

1. choosing which to used

        usually will choose class over struct
        struct tends to be more for fundamental types
        use of eunm is situational (any time you have a type of data with discrete values)

### methods

1. override

        precede your func or var with keyword override
        method marked final can not be overrided
        classes can also be marked final

1. both type and instances can have methods/properties

        var d: Double = ...
        if d.isSignMinus {
            d = Double.abs(d)
        }

        isSignMinus is an instance property of a Double
        abs is a typle method of Double

        declare a type method or property with a static prefix
        static func abs(d: Double) -> Double

### methods parameters names

1. all parameters to all functions have an internal name and an external name

        func foo(exteranl internal: Int){
            let local = internal
        }

        func bar() {
            let result = foo(external: 123)
        }

1. the internal name is the name of the local variable you use inside the method

        func foo(exteranl internal: Int){
                          ^^^^^^^^
            let local = internal
                        ^^^^^^^^
        }

        func bar() {
            let result = foo(external: 123)
        }

1. the external name is what the callers will use to call the method

        func foo(exteranl internal: Int){
                 ^^^^^^^^
            let local = internal
        }

        func bar() {
            let result = foo(external: 123)
                             ^^^^^^^^
        }

1. put _ if you don't want callers to use an external name for a given parameter

        func foo(_ internal: Int){
                 ^
            let local = internal
        }

        func bar() {
            let result = foo(123)
                             ^^^
        }

1. an _ is the default for the first parameter (only) in a method (but not for init methods)

1. force the first parameter's external name to be the internal name with #

        func foo(#internal: Int){
                 ^
            let local = internal
        }

        func bar() {
            let result = foo(internal: 123)
                             ^^^^^^^^
        }

1. for other (not the first) parameters the internal name is, by default the external name

        func foo(first: Int, second: Double){
                             ^^^^^^
            let local = internal
        }

        func bar() {
            let result = foo(123, second: 5.5)
                                  ^^^^^^
        }

1. any parameter's external name can be changed

        func foo(first: Int, externalSecond second: Double){
                             ^^^^^^^^^^^^^^ ^^^^^^
            let local = internal
        }

        func bar() {
            let result = foo(123, externalSecond: 5.5)
                                  ^^^^^^^^^^^^^^
        }

1. even omitted (though this would be sort of "anti-swift")

        func foo(first: Int, _ second: Double){
                             ^ ^^^^^^
            let local = internal
        }

        func bar() {
            let result = foo(123, 5.5)
                                  ^^^
        }

### properties

1. property observers

        you can observer changes to any property with willSet and didSet

        var storedProperty: Int = 42{
            willSet { newValue is the new value }
            didSet { oldValue is the old value }
        }

        override var inheritedProperty{
            willSet { newValue is the new value }
            didSet { oldValue is the old value }
        }

        one common thing to do an observer in a controller is to update the user-interface

1. `lazy` initialization

        a lazy property does not get intialized until someone accesses it
        you can allocate an object, execute a closure, or call a method if you want

        lazy var brain = CalculatorBrain() // nice if CalculatorBrain used lots of resources

        lazy var someProperty: Type{
            // construct the value of someProperty here
            return <the constructed value>
        }()

        lazy var myProperty = self.initializeMyProperty()

        this still satisfies the "you must initialize all of your properties" rule
        things initialized this way can't be constants (i.e. var ok, let not okay)
        this can be used to get around some initialization dependency conundrums

### initialization

1. when is an `init` method needed

        init methods are not so common because properties can have their defaults set using =
        properties might be optional, in which case ther start out nil
        you can also initialize a property by executing a closure
        or use lazy instantiation

        so you can only need init when a value can't be set in any of there ways

1. you also get some "free" `init` method

        if all properties in a base class (no superclass) have defaults, you get init() for free
        if a struct has no initializers, it will get a default one with all properties as arguments

            struct MyStruct{
                var x: Int = 42
                var y: String = "moltuae"

                init(x: Int, y: String) // comes for free
            }

1. what can you do inside an `init`

        you can set any property's value, even those with default values
        constant properties can be set
        you can call other init methods in your own class using self.init(<args>)
        in a class you can of course also call super.init(<args>)
        but there are some rules for calling inits from inits in a class ...

1. what are you required to do inside init

        by the time any init is done, all properties must have values (optionals can have the value nil)
        there are two types of inits in a class, convenience and designated (i.e. not convenience)
        a designated init must (and can only) call a designated init that is in its immediate superclass
        you must initialize all properties introduced by your class before calling a superclass's init
        you must call a superclass's init before you assign a value to an inherited property
        a convenience init must (and can only) call a designated init in its own class
        a convenience init may call a designated init indirectly (through another convenience init)
        a convenience init must call a designated init before it can set any property values
        the calling of other inits must be complete before you can access properties or invoke methods
        