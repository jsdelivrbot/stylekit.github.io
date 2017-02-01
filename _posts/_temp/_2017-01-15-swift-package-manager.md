Notes on Swift package manager<!--more--> Swift package manager seems like the easier to use than CocoaPod and Carthage. Here is the basic workflow:

## Pretext:  
We are all DevOps now. There is no getting around this, if you want to code efficiently you have to master the art of DevOps. Or descend into "dependency hell". Package Dependency managers Are not easy to use, but one cannot live with out them. developing for apple devices requires you to use either Cocoapod Carthage or SPM. Which can be divided into 3 camps: The past, the present and the future. SPM being the future. 

## First impression using SPM:

SPM seems like a command-line tool for downloading projects that contain .swift files from github. It's able to derive files based on "Sem-Ver" tagging of committed code. 

## Export: 
1. Add an empty Package.Swift to your xcode project
```swift
import PackageDescription
let package = Package(
  name: "Element"
)
```
2. keep all other .swift files you want to include in the package in a folder named Sources
3. Upload the Sources folder and the Package.swift file to github


## Import: 

1. Add a Package.swift to your xcode project with example: 
```swift
import PackageDescription
let package = Package(
    name: "Element",
    dependencies: [
        .Package(url: "https://github.com/eonist/Element.git", majorVersion: 1),
    ]
)
```

Pick versions with this syntax: ``majorVersion: 0, minor: 4`` For more specific version picking use: ``Version(0, 0, 0, prereleaseIdentifiers: ["alpha", "3"])`` which would download ``0.0.0-alpha.3`` for ranges you can use syntax such as : ``Version(2, 0, 1) ..< Version(2, 1, 0)``



2. navigate to the folder in terminal ``cd ~/Documents/dev/SomeProject``
3. in terminal execute ``swift build``

## Excluding non-source files:  
Use this property to exclude files and directories from the package sources.  
```swift
let package = Package(
    name: "Foo",
    exclude: ["Sources/Fixtures", "Sources/readme.md", "Tests/FooTests/images"]
)
```

## Create .xcodeproj file via terminal:
cd to the respected folder
```bash
swift package generate-xcodeproj
```

## SPM dependency resolvent: 

If two packages depend on different versions of a third package, the package manager tries to find a version of that package that both will find acceptable.

