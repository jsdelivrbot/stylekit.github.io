Migration insights <!--more--> 


## Range:
Range in swift 3 has been totally re-designed. 🙈
We now have 2 Main Range types: Range and CountableRange.
Apples motivation for seperating these types was:
1. Make a light-weight range type that only holds 2 Ints (start and end)
2. Make a heavy-weight range type that holds a copy of the original Collection type (aka Array)

**Important:** If you want to create extensions for Range you now have to make extensions for: Both types as they don't inherit a common protocol
**Important:** Previously the generic item type for ranges was Element in swift 3 this is called Bound

## For-loop:

The one c-style for-loop to rule them all is gone, now we have 7 different to take its place: 

- ``for i in 0..4{}`` 👈 regular forward looping
- ``for (i,obj) in arr.enumerate(){print(i);print(obj)}`` 👈 access to i and obj
- ``for obj in arr{}`` 👈 iterate over objects
- ``for (i in 0..<4).reversed{}`` 👈 backward looping
- ``var i = 0;while(i<4){print(i);i+=1}`` 👈 If you want to manipulate i while looping
- ``for i in stride(from:0,to:10,skip:2){}`` 👈 Skips every other
- ``arr.forEach{$0}`` 👈 Easiest for-loop but only if you don't need to exit early

## NSView:

``drawLayer(layer:CALayer, inContext ctx: CGContext)`` 👈 This has vanished with out a trace to work around build it your self. 
``actionForLayer(layer:CALayer, forKey event: String) -> CAAction?`` 👈 Also gone, Solution: build it your self.

## Modulo:

Bring back the simple modulo syntax in swift 3:

This syntax was actually suggested on Apples official swift mailing list here but for some reason they opted for a less elegant syntax.

```swift
infix operator %%/*<--infix operator is required for custom infix char combos*/
/**
 * Brings back simple modulo syntax (was removed in swift 3)
 * Calculates the remainder of expression1 divided by expression2
 * The sign of the modulo result matches the sign of the dividend (the first number). For example, -4 % 3 and -4 % -3 both evaluate to -1
 * EXAMPLE: 
 * print(12 %% 5)    // 2
 * print(4.3 %% 2.1) // 0.0999999999999996
 * print(4 %% 4)     // 0
 * NOTE: The first print returns 2, rather than 12/5 or 2.4, because the modulo (%) operator returns only the remainder. The second trace returns 0.0999999999999996 instead of the expected 0.1 because of the limitations of floating-point accuracy in binary computing.
 */
public func %% (left:CGFloat, right:CGFloat) -> CGFloat {
    return left.truncatingRemainder(dividingBy: right)
}
```

