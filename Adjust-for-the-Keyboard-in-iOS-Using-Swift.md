Let's say you have an app with a text view which should just take all the space possible without being beneath the keyboard.

Fist add constraints which pin the text view to all four edges of the super view.

![Screen Shot](http://dasdev.de/wp-content/uploads/2014/11/Screen-Shot-2014-11-29-at-18.02.14.png)

Ctr-drag from the bottom constraint to the view controller to make an outlet for the constraint. This will allow you to change the constant of the bottom constraint.

![Monosnap](http://dasdev.de/wp-content/uploads/2014/11/Monosnap-2014-11-29-18-10-29.png)

Add the view controller as an observer for the notification UIKeyboardWillShowNotification in viewDidLoad() and set the constant of the bottom constraint to 0.0 to make the text view take all the available space at the beginning:

```swift
override func viewDidLoad() {
  super.viewDidLoad()
  
  NSNotificationCenter.defaultCenter().addObserver(self, selector: "keyboardWillShow:", name: UIKeyboardWillShowNotification, object: nil)

  textViewBottomConstraint.constant = 0.0
}
```
    
Add the method which gets called when the notification is triggered by the system:

```swift
func keyboardWillShow(sender: NSNotification) {
  if let userInfo = sender.userInfo {
    if let keyboardHeight = userInfo[UIKeyboardFrameEndUserInfoKey]?.CGRectValue().size.height {
        textViewBottomConstraint.constant = keyboardHeight
        UIView.animateWithDuration(0.25, animations: { () -> Void in
          self.view.layoutIfNeeded()
        })
    }
  }
}
```
Done!
