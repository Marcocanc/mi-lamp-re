#Yeelight Bedside Lamp RE Notes
*NOTE: Code snippets are in Swift since I'm reversing the iOS App*

##Changing Light Attributes

###Endpoints

**Service UUID:** `8E2F0CBD-1A66-4B53-ACE6-B494E25F87BD`

**Characteristics UUID:** `AA7D3F34-2D4F-41E0-807F-52FBF8CF7443`


###Command Formatting

So far it seems like the hex command length is always 18 Bytes. We need to pad out the string with 0.
```swift
func formatCommandString(string: NSString, length: Int = 36) -> NSString {
    let nString = string.stringByReplacingOccurrencesOfString(" ", withString: "")
    return nString.stringByPaddingToLength(length, withString: "0", startingAtIndex: 0)
}
```
