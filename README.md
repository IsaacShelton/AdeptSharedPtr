# AdeptSharedPtr
An implementation of Shared Pointers in Adept

*Notice: This example is designed for Adept 2.6. Due to potential future improvements in management semantics, this library will most likely evolve over time and may not work in its current form in future Adept versions. However, it does work with Adept 2.7 indev as of April 8th 2022.*


### How to Compile

For best results, use `adept2-6`

To compile and run tests
```
adept2-6 -e
```



### How to use in your own project

Copy `Shared.adept` into your project and then import it, that's it.

```
pragma compiler_version '2.6'
pragma ignore_unused

import basics
import "Shared.adept"

func main {
    // Creating Shared Pointers
    name <ubyte> SharedPtr = SharedPtr(strdup('Hello World'))
    
    // Cloning Shared Pointer is automatically done on assignment
    also_name <ubyte> SharedPtr = name
    
    // Creating weak pointers with `.weak()`
    weak_pointer <ubyte> WeakPtr = also_name.weak()
    
    // Shared Pointers are automatically cloned when passed
    // as arguments (that aren't marked as POD)
    greet(name)
    
    // `.get()` can be used to get at the underlying pointer
    // For Weak Pointers, `null` will be returned if the data no longer exists
    printf("First shared pointer is '%s'\n", name.get())
    printf("Should be the same as second shared pointer '%s'\n", also_name.get())
    printf("Should be the same as weak pointer '%s'\n", weak_pointer.get())
    
    // Attempting to create a shared pointer from a weak pointer
    // This is not always necessary, since `.get()` can be written to
    maybe_from_weak <<ubyte> SharedPtr> Optional = weak_pointer.shared()
    
    // Getting the constructed shared pointer out from inside the Optional
    if maybe_from_weak.has {
        from_weak <ubyte> SharedPtr = maybe_from_weak.get()
    }
    
    // Expired shared pointers will be null if the underlying data was destroyed
    expired <int> WeakPtr
    
    if true {
        // Getting shared pointers from functions
        one <int> SharedPtr = getSharedOne()
        two <int> SharedPtr = getSharedTwo()
        
        // Creating a weak pointer (that will soon reference destroyed data)
        expired = two.weak()
    }
    
    // Expired weak pointer will be null, since the data it references
    // doesn't exist anymore
    printf('expired.get() = %p, which should be null\n', expired.get())
}

func greet(message <ubyte> SharedPtr) {
    printf("Welcome, %s!\n", message.get())
}

func getSharedOne() <int> SharedPtr {
    // Returning a constructed shared pointer directly
    return SharedPtr(new int)
}

func getSharedTwo() <int> SharedPtr {
    pointer <int> SharedPtr = SharedPtr(new int)
    
    // When returning an existing shared pointer, it must be cloned
    return pointer.clone()
}
```

