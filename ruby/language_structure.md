# Ruby Language Structure

## Object and Kernel
- `Object` is the base class that all Ruby objects inherit from.
- `Kernel` is a module that `Object` mixes in.
  - Therefore, all methods defined in `Kernel` are available to every Ruby object.

## Ruby Core vs Standard Library
- Core methods, classes and modules are available automatically.
- Standard Library methods, classes and modules need to be explicitly loaded with - `require` - before you can use them
