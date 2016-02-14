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
###Notify
Register for Notify on `8F65073D-9F57-4AAA-AFEA-397D19D5BBEB`

####Notify types:
When receiving a notification, the first 2 bytes define what is being sent

- `4363...` Device Status 1 is Unauthorized, 2 is Authorized

###Pairing with the Device
To Pair with the device,send the following command `436700000000000000000000000000000000` to the lamp
The Lamp will notify with `43 63 01`
After that the device should go into pairing mode (dim on and off). Press the Scene Button on your Lamp to pair.
The Lamp will notify with `43 63 02` to confirm pairing

###Change Color with RGB & Brightness
```swift
func changeColorString(red: Int, green: Int, blue: Int, brightness: Int = 0) -> NSString {
    //not sure what the 4th param does yet, also setting brightness to 0 will have no effect
    return NSString(format: "4341 %02X %02X %02X %02X %02X", red, green, blue, 0, brightness)
}
```
Formatted Example: `4341FF00D700000000000000000000000000`

###Change Temperature and Brightness
Temperature can range from `1700` to `6500`
```swift
func changeTempBrightnessString(temperature: Int, brightness: Int = 0) -> NSString {
    return NSString(format: "4343 %04X %02lX", temperature, brightness)
}
```
Formatted Example: `434313886400000000000000000000000000`
