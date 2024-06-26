# Meet DocC documentation in Xcode

Discover how you can use DocC to build and share documentation for Swift packages and frameworks. We’ll show you how to begin generating documentation from your own code — or from third-party code you depend upon — and write and format it using Markdown. And we’ll also take you through the export process, helping you generate DocC archives to share with the public.

@Metadata {
   @TitleHeading("WWDC21")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc21/10166", purpose: link, label: "Watch Video (22 min)")

   @Contributors {
      @GitHubUser(Jeehut)
      @GitHubUser(zntfdr)
      @GitHubUser(multitudes)
   }
}



## Chapters
- Overview
- Building and browsing
- Authoring
- Sharing

# Overview

Xcode 13 has new features to build, write, and browse documentation for Swift frameworks and packages.

![DOCc][DOCc]

[DOCc]: ../../../images/notes/wwdc21/10166/DOCc.jpg

This documentation lives right alongside the platform libraries in the Developer Documentation window right in Xcode. 

Now Xcode comes with a compiler for the documentation as well as code, and we can build and view documentation for Swift frameworks and packages all inside Xcode.
It is called DocC (pronounced doxxi), and it is integrated throughout Xcode to enhance reading and writing documentation.

- Allows to write code reference docs
- Articles allow you to walk users through the big picture behind your framework
- Tutorials are a powerful step-by-step walk-through of writing a project that uses your framework

![DOCc][DOCc1]

[DOCc1]: WWDC21-10166-DOCc1

More sessions about DOCc at WWDC:
 
[Build interactive tutorials using DocC - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10235)  
[Elevate your DocC documentation in Xcode - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10167)
 
- Will be released as open source later this year

# Building and browsing

## How documentation gets built

- Xcode builds the frameworks or packages and asks the compiler to save information about its public APIs alongside the compiled artifacts  
- That public API information is then handed to DocC, which then combines it with the documentation catalog containing articles and tutorials written outside the app's source code to create the final archive containing the compiled documentation

![DOCc][DOCc2]

[DOCc2]: WWDC21-10166-DOCc2

 To learn more about documentation catalogs and organizing docs, check out the [Elevate your DocC documentation in Xcode - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10167) session.

And thanks to DocC's integration with Xcode's build system, this process repeats for every Swift framework and package that targets depends on. 

##  Three ways for building documentation for Swift frameworks and packages in Xcode 13

- to build documentation on demand, there's a new `Build Documentation` menu item to compile and load up docs.

![DOCc][DOCc3]

[DOCc3]: WWDC21-10166-DOCc3

- if working on a Swift framework and wanting to preview documentation, there's also a new `Build Documentation during ‘Build’` build setting to build docs every time we compile

![DOCc][DOCc4]

[DOCc4]: WWDC21-10166-DOCc4

- via `$ xcodebuild docbuild`

See : [Host and automate your DocC documentation - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10236)  

Introducing the SlothCreator package, which is all about cataloging and customizing cute little sloths. 

![DOCc][DOCc4a]

[DOCc4a]: WWDC21-10166-DOCc4a

Setting up the app called Slothy. Using SlothCreator to customize sloths and preview them. SlothCreator is set as a dependency of the app already, so let's open up the Product menu, and select Build Documentation.

![DOCc][DOCc5]

[DOCc5]: WWDC21-10166-DOCc5

the Developer Documentation window opens, and over in the Navigator, we can expand the Slothy project and the SlothCreator package to see an overview in the Navigator.

![DOCc][DOCc6]

[DOCc6]: WWDC21-10166-DOCc6

Loading the SlothCreator overview in the main view, and scrolling down, there's a list of types and protocols available.

![DOCc][DOCc7]

[DOCc7]: WWDC21-10166-DOCc7

## Authoring

How to write even better documentation with DocC.
DocC is designed around the benefits of in-source documentation.
Write your documentation right alongside the code, making it convenient and easy to integrate with existing development workflow.

In Swift and many other languages, a comment starts with two forward slashes.
```swift
// A model representing a sloth.
public struct Sloth {
	//...
}
```

By writing a comment that begins with three forward slashes, we are telling the Swift documentation compiler to associate the comment with the declaration immediately below it.

The comment will be included in the symbol's compiled documentation page and accessible to anyone who imports your framework. Block-style comments can be created in that samwe style by just including an extra asterisk in the opening delimiter.

![DOCc][DOCc8]

[DOCc8]: WWDC21-10166-DOCc8

DocC only generates documentation pages for the public and open symbols in our framework. Here there's just a couple of symbols in SlothCreator that still need documenting.

Ex this Food struct. Adding three forward slashes above the type's declaration a documentation comment is created. 

The first line of the documentation comment will turn into the symbol's summary, and some extra information can be added with a discussion section adding a line break after the summary and then adding some additional detail about the kinds of foods sloths love. And because DocC has full support for Markdown, we can add a code example using Markdown's fenced code block syntax.

![DOCc][DOCc9]

[DOCc9]: WWDC21-10166-DOCc9

Now rebuilding documentation. Do that by moving the mouse up to the Product menu and then selecting Build Documentation. Xcode will now rebuild SlothCreator's documentation alongside the framework itself.

Xcode has a feature called Quick Help which offers a short summarized version of a symbol's documentation right in the source editor. Holding down the Option key and then clicking on Food's declaration. Or open the same view by option-clicking on any reference to the Food struct in code.

So the comment is right here in the Summary and Discussion sections. Also, new in Xcode 13 is the Open in Developer Documentation link.

![DOCc][DOCc10]

[DOCc10]: WWDC21-10166-DOCc10

The Eat method still needs a documentation comment. We do that with a triple-slash comment above the method's declaration and adding text that will make up its documentation summary.
```swift
/// A model representing a sloth.
public struct Sloth {
	/// Sleep in the specified habitat.
	mutating public func sleep(in habitat: Habitat) {
		energyLevel += habitat. comfortLevel
	}
}
```

Documenting specifically what we're passing as a parameter.

There is a Parameters section. We add a Parameters section to the documentation by writing a Markdown list item starting with the word Parameter followed by the name of the method's parameter, a colon, and then its documentation. Add a Returns section in a very similar way, this time by writing a Returns delimiter followed by a colon and a description of what the method returns.
```swift
/// A model representing a sloth.
public struct Sloth {
	/// Sleep in the specified habitat.
	///
	/// - Parameter habitat: The location for the sloth to sleep.
	/// - Returns: The sloth's energy level after sleeping.
mutating public func sleep(in habitat: Habitat) ->
	mutating public func sleep(in habitat: Habitat) {
		energyLevel += habitat. comfortLevel
	}
}
```

But what if the method has multiple parameters? In this case, the best thing to do is to go from a singular Parameter delimiter to a plural Parameters one. This works much the same as the others except the parameter names are written as children of the parent Parameters delimiter.
```swift
/// A model representing a sloth.
public struct Sloth {
	/// Sleep in the specified habitat.
	///
	/// - Parameters:
	/// 	- habitat: The location for the sloth to sleep.
	/// 	- numberOfHours: The number of hours for the sloth to sleep.
	/// - Returns: The sloth's energy level after sleeping.
	mutating public func sleep(in habitat: Habitat, for numberfHours: Int = 12) -> Int {
		energyLevel += habitat.comfortLevel * number0fHours 
		return energyLevel
	}
}
```

![DOCc][DOCc11]

[DOCc11]: ../../../images/notes/wwdc21/10166/DOCc11.jpg

## documenting the sloth Eat method.

Documenting a more complicated symbol, we are going to take advantage of Xcode's great 'Add Documentation' feature which will insert a template best-suited for documenting the current declaration.

Holding down the Command key and then clicking on the method's declaration open the Action menu. Next, click on Add Documentation in the Action menu.

![DOCc][DOCc12]

[DOCc12]: WWDC21-10166-DOCc12

We get a template:

![DOCc][DOCc13]

[DOCc13]: WWDC21-10166-DOCc13

Fill out the method's summary, and then describe the food and quantity parameters as well as what this method returns. And then finish off the documentation by adding a Discussion section along with a code example.
```swift
/// Eat the provided specialty sloth food.
/// 
/// Sloths love to eat while they move very slowly through their rainforest habitats. They 
/// are especially happy to consumereaves and twigs, which they digest over long periods 
/// of time, mostly while they sleep.
///
/// ```swift
///	let flower = Sloth. Food(name: "Flower Bud", energy: 10)
/// superSloth.eat(flower)
/// ```
///
/// -Parameters:
///    	- food: The food for the sloth to eat.
/// 	- quantity: The quantity of the food for the sloth to eat.
/// - Returns: The sloth's energy level after eating.
mutating public func eat(_ food: Food, quantity: Int = 1) -> Int {
		energyLevel += food.energy * quantity
		//...
}
```

Rebuild documentation by moving the mouse to the Product menu and selecting the Build Documentation item.
And once again, Xcode is building the documentation alongside the framework itself.
If we navigate to the Sloth struct via the window's Navigator, we will find Sloth in the Sloths topic group. Scrolling down on the Sloth page, we'll find the method in the Instance Methods section.

![DOCc][DOCc14]

[DOCc14]: WWDC21-10166-DOCc14

## Connecting documentation

- Make connections
- Encourage discovery
- Code completion

ex. There are some other symbols that the reader should consider learning in the framework

New to Xcode 13 is the ability to link to symbols in the documentation. This is a great way to connect different parts of the framework and guide the reader to relevant pieces of information.

We write these links via a new double backtick syntax.
```swift
/// You can increase the sloth's energy level by asking them to eat ``(_:quantity:)``
/// or ``sleep(in: for:)``.
```
Example:
In the Sloth struct there's a value representing the current energy level of the sloth, and eventually it's important here for the reader to understand that one of the side effects of calling sleep is a change to the sloth's energy level.

So it is possible to add to the methods Discussion section a reference to the energyLevel property. Before Xcode 13, the natural way to do this would be to just write the property's name in a monospace code font by surrounding it in backticks.
But now we use double backtick syntax and create a link. Linking to energyLevel was pretty simple because that property is a sibling of the method we are documenting, and there is no need to further qualify the link with the parent type. 

But if we reference a child of a different type, we need to be a little more specific. So here we write Habitat/comfortLevel to link to a child of the Habitat struct.
```swift
/// A model representing a sloth.
public struct Sloth {
	/// The energy level of the sloth.
	public var energyLevel: EnergyLevel
	
	/// Sleep in the specified habitat.
	///
	/// Each time the sloth sleeps, their `energyLevel` increases every hour by the
	/// habitat's ``Habitat/comfortLevel``
	///
	/// - Parameters:
	/// 	- habitat: The location for the sloth to sleep.
	/// 	- numberOfHours: The number of hours for the sloth to sleep.
	/// - Returns: The sloth's energy level after sleeping.
	mutating public func sleep(in habitat: Habitat, for numberfHours: Int = 12) -> Int {
		energyLevel += habitat.comfortLevel * number0fHours 
		return energyLevel
	}
}
```

Xcode's code completion will help to make sure we are getting the correct link.

![DOCc][DOCc15]

[DOCc15]: WWDC21-10166-DOCc15

And links are even accessible in Quick Help.

So holding down the Option key and clicking on the method's declaration we open the Quick Help popover.
Here's the Discussion section we added and, of course, in the discussion, the two links.

![DOCc][DOCc16]

[DOCc16]: WWDC21-10166-DOCc16


# Sharing
- Documentation archives
- Open directly in the Xcode documentation window
- Publish to the web

Sharing. We can do this via the documentation archive that Xcode outputs as part of every documentation build. Contained in the documentation archive is a single-page web app that we can use to share the documentation on the web.
 
But Xcode also supports exporting and importing documentation directly from the documentation window.

![DOCc][DOCc17]

[DOCc17]: WWDC21-10166-DOCc17

Export SlothCreator from the documentation window by first moving the mouse over to the window's Navigator. When hovering over the SlothCreator framework item, a contextual menu icon will appear. Click on it, and Export.

Now it is ready to send it off to a colleague, and if they just double-click on the archive, it will open in Xcode's documentation window.

Adding a command line interface to SlothCreator, for instance ArgumentParser. Reading the docs...

![DOCc][DOCc18]

[DOCc18]: WWDC21-10166-DOCc18

# Wrap up

- Xcode supports opening the DocC Archive in a nice GUI with navigation & search (called Documentation Viewer)
- Documentation is written in-source, via `///` doc comments
- DocC only generates documentation for code marked as `public` or `open`
- Code examples can be added via Markdown code syntax via 3 backticks "\`\`\`swift"
- New “Open in Developer Documentation” link in any QuickHelp menu (Option + Click)
- Prefer `Parameters` over `Parameter` for multiple parameters for method docs
- Option+Click and choose “Add Documentation” to auto-generate documentation template
- New double-backtick syntax to inter-link between different documented elements
- For fields of different types, use `/` as in `Habitat/child`, includes auto-completion
- Xcode exports an archive which is a "single page web app", can be shared to Web easily
- Export via (…) menu, file extension is `.doccarchive`

### Link symbols
- Write links via a double backtick syntax 
- Linking to sibling definitions only requires the name of the definition

```swift
/// This is a sibling property link ``propertyName``.
```

- when referencing external definitions, we need to further qualify the link with the parent type. 

```swift
/// This is a property of another type link ``TypeName/propertyName``.
```

## Resources
[Formatting your documentation](https://developer.apple.com/documentation/Xcode/formatting-your-documentation)  
[Have a question? Ask with tag wwdc21-10166](https://developer.apple.com/forums/create/question?&tag1=266&tag2=76030&tag3=267030)  
[Search the forums for tag wwdc21-10166](https://developer.apple.com/forums/tags/wwdc21-10166)  
[SlothCreator: Building DocC Documentation in Xcode](https://developer.apple.com/documentation/xcode/slothcreator_building_docc_documentation_in_xcode)  
[Writing symbol documentation in your source files](https://developer.apple.com/documentation/Xcode/writing-symbol-documentation-in-your-source-files)  


## Related Videos
[Create rich documentation with Swift-DocC - WWDC23](https://developer.apple.com/videos/play/wwdc2023/10244)  
[Build interactive tutorials using DocC - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10235)  
[Elevate your DocC documentation in Xcode - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10167)  
[Host and automate your DocC documentation - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10236)  
[What‘s new in Swift - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10192)  
