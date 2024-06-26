# Mix Swift and C++

Learn how you can use Swift in your C++ and Objective-C++ projects to make your code safer, faster, and easier to develop. We’ll show you how to use C++ and Swift APIs to incrementally incorporate Swift into your app. 

@Metadata {
   @TitleHeading("WWDC23")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc23/10172", purpose: link, label: "Watch Video (17 min)")

   @Contributors {
      @GitHubUser(RamitSharma991)
   }
}



# Basics of interoperability

## Incremental adoption

  * Builds on Objective-C interoperability
  * Supports incremental Swift adoption

### Using Swift in more places

   * Adopt Swift in large C++ codebases.
   * Use C++ libraries in Swift.
   * Remove Objective-C bridging. 
   * While using C++ frameworks, Xcode imports C++ APIs automatically and don’t need a bridging header.
   * Enable C++ interoperability in the build settings.

```swift
  // Calling a C++ method from Swift
  func loadImage(_ image: UIImage) {
  // Load an image into the shared C++ class.
  CxxImageEngine.shared.pointee.loadImage(image)
}

  // importing a C++ framework 
  import CxxImageKit
```

## Bidirectional interoperability

 * Incremental Swift adoption.
 * Completely automatic integration by the Swift compiler without writting a Bridging layer.
 * Direct calls and no overheads when calling APIs in Swift and vice-versa.
 * C++ APIs availablity in Swift:
    * can import most C++ collections, both from the standard library and elsewhere
        * **Collections**: `std::vector`, `std::map`, User-defined
    * handlew function templates and class template specializations
    * supports managing memory using shared pointer and similar user-defined types. 
        * **Smart pointers**: `std::shared_ptr`, user-defined
    * **Members**: methods, properties and intializers
    * **Generics**: structs and enums
    * **Standard library**: `swift::Array`, `swift::String`, and `swift::Optional`
    * **Library evolution**:  resilient structs and classes

* Xcode Compatiblity
    * Code completion
    * Jump-to-definition
    * Global rename
    * Debugger support

# Natural Swift APIs

* Swift compiler is able to automatically import most C++ APIs and represent them as safe Swift APIs.
    * Getters and Setters -> Methods
    * Value types -> Structs 
    * Reference Types -> Pointers 
    * Operators -> Operators 
    * Containers -> Swift Collections
    * Constructors -> initializers 
* Swift compiler allows us to fine-tune how APIs are imported and expose APIs that feel even more natural by providing the compiler with more information about your APIs through the use of annotations.
* structs represent value types and classes represent reference types.
* C++ types will be imported as value types in Swift.


## Imported value types

   * Short lifetimes and deep copies.
   * Copy constructor and destructor manage lifetime.

## Collections

   * Imports any type with `begin` and `end` methods as Swift collection.
   * Swift Collections can be used in `for-loops`, `map`, `filter` 

## Swift Collections Safety

   * use these Swift Collection APIs rather than C++ iterator APIs which do not fit into Swift's safety model.
   * The Swift compiler will help guide you towards these safer APIs by marking unsafe C++ APIs as unavailable and suggesting a safer alternative.
   * C++ doesn't have a strong distinction between value types and reference types.
   * Swift also gives you the option to import some things as reference or class types by adding an annotation to your C++ code.


## Foreign reference types

   * map `CxxImageEngine` to a Swift class using the `SWIFT_SHARED_REFERENCE` attribute.
   * Enforces reference sematics.
   * Removes UnsafePointer indirection.
   * Manages lifetime with custom retain and release operations.


```swift
  //importing swift/bridging
  #import <swift/bridging>

  //Applying SWIFT_SHARED_REFERENCE attribute to CxxImageEngine
  struct SWIFT_SHARED_REFERENCE(IKRetain, IKRelease) CxxImageEngine {
      // ...
  };
```
     
### Computed properties

  `SWIFT_COMPUTED_PROPERTY` attribute maps getter and setter to property.

```swift
   /// \returns all images that have been loaded into the engine. Includes any modifications that were
   /// applied to the images.
   SWIFT_COMPUTED_PROPERTY
   inline std::vector<Image *_Nonnull> getImages() const;
```

## Future evolution

  * Evolve based on your feedback.
  * Opt-in to changes with interoperability versions.
  * You can join the workgroup and get involved on the forums. Just head to [Mixing Swift and C++](https://www.swift.org/documentation/cxx-interop/) 
