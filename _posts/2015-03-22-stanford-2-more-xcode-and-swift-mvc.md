---
layout: post
title: "stanford 2 more xcode and swift mvc"
description: ""
category: [stanford]
tags: [func, closure expression, lambda, anonymous function]
---
{% include JB/setup %}

### demo calculator code

1. define a func

        func performOperation(operation:(Double, Double) -> Double) {
            if 2 <= operandStack.count{
                displayValue = operation(operandStack.removeLast(), operandStack.removeLast())
                enter()
            }
        }

1. you can call this func

        performOperation({ (op1: Double, op2: Double) -> Double in return op1 * op2 })

1. get rid of the type declaration

        performOperation({ (op1, op2) in return op1 * op2 })

1. get rid of the return

        performOperation({ (op1, op2) in op1 * op2 })

1. do not need to name arguments

        performOperation({ $0 * $1 })

1. this is the last argument you can move this outside the parentheses

        performOperation() { $0 * $1 }

1. there are no other arguments you can get rid of the parentheses

        performOperation { $0 * $1 }

