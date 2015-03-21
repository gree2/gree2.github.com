---
layout: post
title: "stanford 7 multiple mvcs"
description: ""
category: [stanford]
tags: [mvc,TabBar, SplitView, Navigation]
---
{% include JB/setup %}

### accessing the sub-mvc

1. get sub-mvcs via `viewControllers` property

    * tab bar `-->` they are in order, left to right, in the array
    * split view `-->` [0] is master and [1] is detail
    * navigation controller `-->` [0] is root and the rest are in order on the stack

            even though this is settable usually setting via storyboard, segues, or other
            eg. nc's push and pop methods

1. get svc tbc or nc itself

    * var tabBarController: UITabBarController? { get }
    * var splitViewController: UISplitViewController? { get }
    * var navigationController: UINavigationController? { get }

            eg. get the detail of the split view controller
            if let dvc: UIViewController = splitViewController?.viewControllers[1] { ... }

1. wiring up mvcs

    * drag out a `Split View Controller`
    * ctrl-drag from `UISplitViewController` to `master` and `detail` mvcs
    * warp a `nc`  around an mvc with `Editor->Embed In->Navigation Controller`

            split view can only do its thing properly on iPad
            so put some nc in there so it will work on iPhone

### segues

1. segue

    * show segue `-->` will push in a nc
    * show detail `-->` segue will show in detail of a split view or will push in a nc
    * modal segue `-->` take over the entir screen while the mvc is up
    * popover segue `-->` make the mvc appear in a little popover window

1. segue identifier

    * invoke this segue from code using this UIViewController method

            func performSegueWithIdentifier(identifier: String, sender: AnyObject?)

    * ctrl-drag from the controller itself to another controller if you're segueing via code

1. preparing for a segue

    1. the identifier form the storyboard
    2. the controller of the mvc you are segueing to

            func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?){
                if let identifier = segue.identifier{
                    switch identifier{
                        case "segue_show_detail":
                            if let vc = segue.destinationViewController as? MyController{
                                vc.property = ...
                                vc.callMethodToSetup()
                            }
                    }
                }
            }

    `it is crucial to understand that this preparation is happening BEFORE outlets get set!!`

    `it is a very common bug to prepare an mvc thinking its outlets are set`

1. prevent a segue

    * just impl this in your UIViewController

            func shouldPerformSegueWithIdentifier(identifier: String?, sender: AnyObject?) -> Bool

### popover

1. `prepareForSegue`

        func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?){
            if let identifier = segue.identifier{
                switch identifier{
                    case "segue_do_sth":
                        if let vc = segue.destinationViewController as? MyController{
                            if let ppc = vc.popoverPresentationController{
                                ppc.permittedArrowDirections = UIPopoverArrowDirection.Any
                                ppc.delegate = self
                            }
                        }
                }
            }
        }

1. delegate

        func adaptivePresentationStyleForPresentationController(UIPresentationController)
            -> UIModalPresentationStyle
        {
            return UIModalPresentationStyle.None // don't adapt default is .FullScreen
        }

        func presentationController(UIPresentationController, style: UIModalPresentationStyle)
            -> UIViewController?
        {
            // return a UIViewController to use
            // eg. wrap a nc around your mvc
        }

1. popover size

    * a popover will be made pretty large unless someone tells it otherwise
    * mvc being presented knows best what it's `preferred` size inside a popover would be
    * it expresses that via this property in itself

            var preferredContentSize: CGFloat

    * mvc is not guaranteed to be that size but the system will try its best

1. demo

    * UIViewController

            class TextViewController: UIViewController, UIPopoverPresentationControllerDelegate {
                @IBOutlet weak var textView: UITextView!{
                    didSet{
                        textView.text = text
                    }
                }

                var text: String = ""{
                    didSet{
                        textView?.text = text
                    }
                }

                override var preferredContentSize: CGFloat{
                    get {
                        if textView != nil && presentingViewController != nil{
                            return textView.sizeThatFits(presentingViewController!.view.bounds.size)
                        } else {
                            return super.preferredContentSize
                        }
                    }
                    set { super.preferredContentSize = newValue }
                }
            }

    * DiagnosedHappinessViewController

            DiagnosedHappinessViewController: HappinessViewControl{

                override var happiness: Int{
                    didSet{
                        diagnosticHistory += [happiness]
                    }
                }
                private let defaults = NSUserDefaults.standardUserDefaults()

                var diagnosticHistory: [Int] {
                    get { return defaults.objectForKey(History.DefaultKey) as? [Int] ?? [] }
                    set { defaults.setObject(newValue, forKey: History.DefaultKey) }
                }

                private struct History {
                    static let SegueIdentifier = "SegueIdentifier"
                    static let DefaultKey = "ClassName.History"
                }

                override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?){
                    if let identifier = segue.identifier{
                        switch identifier{
                            case History.SegueIdentifier:
                                if let tvc = setup.destinationViewController as? TextViewController{
                                    if let ppc = tvc.popoverPresentationController{
                                        ppc.delegate = self
                                    }
                                    tvc.text = "\(diagnosticHistory)"
                                }
                            default: break
                        }
                    }
                }

                func adaptivePresentationStyleForPresentationController(controller: UIPresentationController)
                    -> UIModalPresentationStyle{
                    return UIModalPresentationStyle.None
                }
            }