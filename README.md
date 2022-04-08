# AdeptSharedPtr
An implementation of Shared Pointers in Adept



### How to Compile

Notice: This example is only designed for Adept 2.6, as it explicitly relies on the `2.6` version of management semantics and `Optional.adept`. It may need to updated for future releases.

For best results, use `adept2-6`

To compile and run tests
```
adept2-6 -e
```



### How to use in your own project

Copy `Shared.adept` into your project and then import it, that's it.

```
pragma ignore_unused

import basics
import "Shared.adept"

func main {
    name <ubyte> SharedPtr = SharedPtr(strdup('Hello World'))
    also_name <ubyte> SharedPtr = name
    weak_pointer <ubyte> WeakPtr = also_name.weak()
    greet(name)
    
    printf("%s\n", name.get())
    printf("%s\n", also_name.get())
    printf("%s\n", weak_pointer.get())
    
    maybe_from_weak <<ubyte> SharedPtr> Optional = weak_pointer.shared()
    
    if maybe_from_weak.has {
        from_weak <ubyte> SharedPtr = maybe_from_weak.get()
    }
    
    expired <int> WeakPtr
    
    if true {
        one <int> SharedPtr = getSharedOne()
        two <int> SharedPtr = getSharedTwo()
        expired = two.weak()
    }
    
    printf('expired.get() = %p\n', expired.get())
}

func greet(name <ubyte> SharedPtr) {
    printf("Welcome %s\n", name.get())
}

func getSharedOne() <int> SharedPtr {
    return SharedPtr(new int)
}

func getSharedTwo() <int> SharedPtr {
    pointer <int> SharedPtr = SharedPtr(new int)
    return pointer.clone()
}
```

