---
layout: post
title: "navigation between two view controllers"
description: ""
category: [ios]
tags: [navigation, swift]
---
{% include JB/setup %}


### navigation use button -> view 2

1. drag `View Controller` into `Main.storyboard`
1. `Editor` -> `Embed in` -> `Navigation Controller`
1. impl button press action

        @IBAction func toView2(sender : AnyObject){
            let view2 = storyboard.instantiateViewControllerWithIdenfifier("View2") as View2
            self.navigationController.pushViewController(view2, animated: true)
        }

### navigation use button -> view 1

1. create `View2 : UIViewController`
1. drag `View Controller` into `Main.storyboard`
1. select `View Controller` set `Custom Class`
1. impl button press action

        @IBAction func toView1(sender : AnyObject){
            self.navigationController.popToRootViewControllerAnimated(true)
        }

### navigation use segue view 1 <--> view 2

1. add a `Bar Button Item` to `View1` on `Main.storyboard`
1. `Ctrl+Drag` form `Bar Button Item` to View2
1. `Action Segue` choose `show`

### navigation use segue customize back button

1. open `Main.storyboard` and click on the link connect two controller
1. open `Utilities` and click on `Attributes inspector`
1. give the segue a meaningful `identifier`
1. open `parent view controller` and insert code

        // In a storyboard-based application, you will often want to do a little preparation before navigation
        override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
            // Get the new view controller using [segue destinationViewController].
            // Pass the selected object to the new view controller.
            if let identifier = segue.identifier{
                if identifier == "segue_setting"{
                    self.navigationItem.backBarButtonItem =
                        UIBarButtonItem(title: "返回", style: .Bordered, target: nil, action: "")
                }
            }
        }