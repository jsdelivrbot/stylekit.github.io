parsing all the macOS Sierra GUI styles takes between 5-10secs on a 4-core 2013 MacBook Pro Retina. To make it faster <!--more--> one could re-write the current CSS parsing engine that uses RegExp. Or even using a speedier open-source css parsing engine from someone else. But then adding new features or quickly fixing bugs would be hard to accomplish.  

Another option would be to cache the styles after they were rendered. And then if no .css file changed in the subsequent runs, the pre rendered cached styles would be used. In this scenario loading a window packed with GUI elements would load bellow 1 sec. Which is imperceptible. Opening up additional GUI elements would be instant.  

## To accomplish this a couple of things was needed: 

1. Get data from the rendered styles (Reflection)  
2. Rebuild the styles from a storage (UnWrapping)  
3. Check if any .css files had been modified since last run  

A Reflection library was built in swift. And an UnWrapping library. There were a few open-source project that could Reflect and UnWrap. But some would only support structs while others wouldn't work with CGColor etc. So Building new libraries was justified. Also Reflection and UnWrapping is pretty complicated with swift as Swift is a "statically-typed-language" and so there needs to be a level of custom code to most custom classes. Unlike Dynamically-typed-languages were you can automate all reflection and unwrapping.   

The Reflection and UnWrapping library was written as an universal library that can work on any class. Some classes needs to have custom code to work and this can be accomplished through extensions: 

## Reflection and UnWrapping Example code:

```swift
/**
 * NOTE: we use 32 bit RGBA values when storing color data (This also stores the alpha value)
 * NOTE: XML is used as the storage syntax. JSON could be used but there was no apparent benefit so XML it is
 */
let temp = Temp(NSColor.redColor())
let xml = Reflection.toXML(temp)/*Reflection*/
print(xml.XMLString)//Output: <Temp><color type="NSColor">FFFF0000</color></Temp>
let newInstance = Temp.unWrap(xml)!/*UnWrapping*/
print(newInstance.color.hexString)//Output: FF0000

class Temp{
    var color:NSColor
    init(_ color:NSColor){
        self.color = color
    }
}
extension Temp:UnWrappable{
    static func unWrap<T>(xml:XML) -> T? {
        let color:NSColor? = unWrap(xml,"color")
        return Temp(color!) as? T
    }
}

```

The Reflection and UnWrapping library can be found here: https://github.com/eonist/swift-utils