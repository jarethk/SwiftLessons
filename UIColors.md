# UI Colors

[1] Apple standard colors
Initializing colors standard interface uses CGFloat range 0-1, so if you are used to using 0-255 you have to divide by 255 to get a CGFloat value.  
These extensions below simplify this, helping to make your code readable in a 255 color range.

```swift
extension UIColor {
    
    convenience init(red: Int, green: Int, blue: Int) {
        let newRed = CGFloat(red)/255
        let newGreen = CGFloat(green)/255
        let newBlue = CGFloat(blue)/255
               
        self.init(red: newRed, green: newGreen, blue: newBlue, alpha: 1.0)
    }

    convenience init(red: Int, green: Int, blue: Int, alpha: CGFloat) {
        let newRed = CGFloat(red)/255
        let newGreen = CGFloat(green)/255
        let newBlue = CGFloat(blue)/255
               
        self.init(red: newRed, green: newGreen, blue: newBlue, alpha: alpha)
    }
}
```

## App-Defined Colors
You can define app-specific colors and give them names.  Add as an extension on UIColor to get the convenience of referring to it by dot-reference.

```swift
extension UIColor {
    static var appGold = UIColor(red: 255, green: 215, blue: 0)
}

myLabel.backgroundColor = .appGold
```

 - [1](https://developer.apple.com/documentation/uikit/uicolor/standard_colors)
 - [2](https://www.codingexplorer.com/create-uicolor-swift/)
