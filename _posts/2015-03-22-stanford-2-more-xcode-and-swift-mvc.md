---
layout: post
title: "stanford 2 more xcode and swift mvc"
description: ""
category: [stanford]
tags: [func, closure expression, lambda, anonymous function]
---
{% include JB/setup %}

### calculator code

    class ViewController: UIViewController {

        @IBOutlet weak var display: UILabel!
        
        var isTyping = false

        @IBAction func appendDigit(sender: UIButton) {
            let digit = sender.currentTitle!
            if isTyping{
                display.text = display.text! + digit
            } else {
                display.text = digit
                isTyping = true
            }
        }
        
        @IBAction func operate(sender: UIButton) {
            let operation = sender.currentTitle!
            if isTyping{
                enter()
            }
            switch operation {
            case "×": performOperation { $0 * $1 }
            case "÷": performOperation { $1 / $0 }
            case "+": performOperation { $0 + $1 }
            case "−": performOperation { $1 - $0 }
            case "√": performOperation { sqrt($0) }
            default: break
            }
        }
        
        func performOperation(operation:(Double, Double) -> Double) {
            if 2 <= operandStack.count{
                displayValue = operation(operandStack.removeLast(), operandStack.removeLast())
                enter()
            }
        }
        
        func performOperation(operation:(Double) -> Double) {
            if 1 <= operandStack.count{
                displayValue = operation(operandStack.removeLast())
                enter()
            }
        }
        
        var operandStack = [Double]()
        
        @IBAction func enter() {
            isTyping = false
            operandStack.append(displayValue)
            println("operandStack = \(operandStack)")
        }
        
        var displayValue: Double {
            get {
                return NSNumberFormatter().numberFromString(display.text!)!.doubleValue
            }
            
            set {
                display.text = "\(newValue)"
                isTyping = false
            }
        }
    }

### calculator code analysis

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