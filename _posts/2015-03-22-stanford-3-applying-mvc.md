---
layout: post
title: "stanford 3 applying mvc"
description: ""
category: [stanford]
tags: [mvc, protocol, enum, recursion]
---
{% include JB/setup %}

### calculator brain code

    class CalculatorBrain
    {
        private enum Op: Printable{
            case Operand(Double)
            case UnaryOperation(String, Double -> Double)
            case BinaryOperation(String, (Double, Double) -> Double)
            
            // impl Printable protocol
            var description: String{
                get {
                    switch self{
                    case .Operand(let operand):
                        return "\(operand)"
                    case .UnaryOperation(let symbol, _):
                        return symbol
                    case .BinaryOperation(let symbol, _):
                        return symbol
                    }
                }
            }
        }
        
        private var opStack = [Op]()
        
        private var knownOps = [String: Op]()
        
        init() {
            
            func learnOp(op: Op){
                knownOps[op.description] = op
            }
            
            learnOp(Op.BinaryOperation("×", *))
            learnOp(Op.BinaryOperation("÷") { $1 / $0 })
            learnOp(Op.BinaryOperation("+", +))
            learnOp(Op.BinaryOperation("−") { $1 - $0 })
            learnOp(Op.UnaryOperation("√", sqrt))
        }
        
        private func evaluate(ops: [Op]) -> (result: Double?, remainingOps: [Op]) {
            if !ops.isEmpty {
                var remainingOps = ops
                let op = remainingOps.removeLast()
                switch op{
                case .Operand(let operand):
                    return (operand, remainingOps)
                case .UnaryOperation(_, let operation):
                    let operandEvaluation = evaluate(remainingOps)
                    if let operand = operandEvaluation.result{
                        return (operation(operand), operandEvaluation.remainingOps)
                    }
                case .BinaryOperation(_, let operation):
                    let op1Evaluation = evaluate(remainingOps)
                    if let operand1 = op1Evaluation.result{
                        let op2Evaluation = evaluate(op1Evaluation.remainingOps)
                        if let operand2 = op2Evaluation.result{
                            return (operation(operand1, operand2), op2Evaluation.remainingOps)
                        }
                    }
                }
            }
            return (nil, ops)
        }
        
        func evaluate() -> Double? {
            let (result, remainder) = evaluate(opStack)
            println("\(opStack) = \(result) with \(remainder)")
            return result
        }
        
        func pushOperand(operand: Double) -> Double? {
            opStack.append(Op.Operand(operand))
            return evaluate()
        }
        
        func performOperation(symbol: String) -> Double? {
            if let operation = knownOps[symbol]{
                opStack.append(operation)
            }
            return evaluate()
        }
    }

### calculator brain code analysis

1. protocol `Printable`

        impl property description

1. func in another func

        func learnOp(op: Op)

1. `enum` with func

        case BinaryOperation(String, (Double, Double) -> Double)

1. `recursion` func

        func evaluate(ops: [Op]) -> (result: Double?, remainingOps: [Op])