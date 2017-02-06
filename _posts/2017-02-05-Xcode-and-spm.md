Here is how you use Swift package manager in your XCode app projects<!--more--> 

## The workflow:  

- Terminal: ``cd ~/dev/MyProject/`` 👈 navigate to your project  
- Terminal: ``swift package init`` 👈 creates the initial SPM files    
- Add the bellow to your newly created Package.swift file:   

```swift
import PackageDescription
let package = Package(
    name: "MyProject",
	dependencies: [
		.Package(url: "https://github.com/eonist/swift-utils.git", Version(0, 0, 0, prereleaseIdentifiers: ["alpha", "3"]))
    ]
)
```
👆 baszically adds Swift-utils as a third Party framework in your project    
- Terminal: ``swift build`` 👈 downloads the dependencies from github and builds binaries (aka .framework)    
- Terminal: ``swift package generate-xcodeproj`` 👈  Creates an XCode project that has .framework files  
- XCode: Open the .xcodeproj file file -> Target -> Cocoa app  
- XCode: Add: ``@testable import Utils`` to ``AppDelegate.swift`` and ``print(StringParser.sansSuffix("blue"))`` inside the ``applicationDidFinishLaunching`` method  
- ``cmd + r`` will now print ``blu``  

## Why is this awesome?

1. When you develop your app. You use binaries but you have full access to third-party code. 🔑  
2. When you need to download a new version of third party libs: Terminal: ``swift package update``    
3. Supports nested frameworks 👈 "holy grail of Dependency management"   	 
4. Supports CI untested but check  [this](https://www.linkedin.com/pulse/apple-swift-package-manager-deep-dive-shashikant-jagtap) 
5. If you are developing on your own third-party modules you can integrate this workflow with no extra steps. as the packages are pure .git projects. Just commit back to github repo directly from inside your project.    
6. If you add code to third-party dependencies then they are embedded in the binaries automatically.   

## Todo:  
- [ ] Investigate ``buildMetadataIdentifier`` 👈 Supposedly it enables you to target single commit ids rather than version tags
- [ ] Implement CI Travis 
