# Yeelight Bedside Lamp RE Notes
*NOTE: Code snippets are in Swift since I'm reversing the iOS App*

### Endpoints

**Service UUID:** `8E2F0CBD-1A66-4B53-ACE6-B494E25F87BD`

**Characteristics UUID:** `AA7D3F34-2D4F-41E0-807F-52FBF8CF7443`


### Command Formatting

So far it seems like the hex command length is always 18 Bytes. We need to pad out the string with 0.
```swift
func formatCommandString(string: NSString, length: Int = 36) -> NSString {
    return string.stringByReplacingOccurrencesOfString(" ", withString: "")
            .stringByPaddingToLength(length, withString: "0", startingAtIndex: 0)
}
```
### Notify
Register for Notify on `8F65073D-9F57-4AAA-AFEA-397D19D5BBEB`

#### Notify types:
When receiving a notification, the first bytes define what is being received

- `4363..` Device Status
    -  `01` Unauthorized/Not paired
    -  `02` Authorized/Paired
    -  `04` Authorized device (UDID)
    -  `07` Lamp disconnect imminent

### Pairing with the Device
To Pair with the device,send the `4367` command with a client uuid. Example: `43677207d94ecb9e4ec5be6eafa46ed1c07c`

The Lamp will notify with `43 63 01` to signal that it is in pairing mode.

Press the Scene Button on your Lamp to pair.

The Lamp will notify with `43 63 02` to confirm successful pairing

If the Lamp was previously paired with the device, it will notify `43 63 04`

### Turn On/Off
The On/Off state can be switched with the `4340..` command. `434001` will turn it on, `434002` will turn it off

### Change Color with RGB & Brightness
```swift
func changeColorString(red: Int, green: Int, blue: Int, brightness: Int = 0) -> NSString {
    //not sure what the 4th param does yet, also setting brightness to 0 will have no effect
    return NSString(format: "4341 %02X %02X %02X %02X %02X", red, green, blue, 0, brightness)
}
```
Formatted Example: `4341FF00D700000000000000000000000000`

### Change Temperature and Brightness
Temperature can range from `1700` to `6500`
```swift
func changeTempBrightnessString(temperature: Int, brightness: Int = 0) -> NSString {
    return NSString(format: "4343 %04X %02lX", temperature, brightness)
}
```
Formatted Example: `434313886400000000000000000000000000`


### More Commands
- Disconnect: `4368`
- Read color flow: `434c %02lx`
- Delete color flow: `4373 %02lx`
- Read lamp name: `4352`
- Get statistics Data: `438c`
- Set delay to off: `437f01%02x`
*Will add more soon...*
