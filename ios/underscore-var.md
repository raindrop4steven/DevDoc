This is an artifact of a previous version of the Objective-C runtime.

Originally, @synthesize was used to create accessors methods, but the runtime still required that instance variables had to be instantiated explicitly:

@interface Foo : Bar {
  Baz *_qux;
}

@property (retain) Baz *qux;
@end

@implementation Foo
@synthesize qux = _qux;

- (void)dealloc {
  [_qux release];
  [super dealloc];
}

@end
People would prefix their instance variables to differentiate them from their properties (even though Apple doesn't want you to use underscores, but that's a different matter). You synthesize the property to point at the instance variable. But the point is, _qux is an instance variable and self.qux (or [self qux]) is the message qux sent to object self.

We use the instance variable directly in -dealloc; using the accessor method instead would look like this (though I don't recommend it, for reasons I'll explain shortly):

- (void)dealloc {
  self.qux = nil; // [self setQux:nil];
  [super dealloc];
}
This has the effect of releasing qux, as well as zeroing out the reference. But this can have unfortunate side-effects:

You may end up firing some unexpected notifications. Other objects may be observing changes to qux, which are recorded when an accessor method is used to change it.
(Not everyone agrees on this point:) Zeroing out the pointer as the accessor does may hide logic errors in your program. If you are ever accessing an instance variable of an object after the object has been deallocated, you are doing something seriously wrong. Because of Objective-C's nil-messaging semantics, however, you'll never know, having used the accessor to set to nil. Had you released the instance variable directly and not zeroed-out the reference, accessing the deallocated object would have caused a loud EXC_BAD_ACCESS.
Later versions of the runtime added the ability to synthesize instance variables in addition to the accessor methods. With these versions of the runtime, the code above can be written omitting the instance variables:

@interface Foo : Bar
@property (retain) Baz *qux;
@end

@implementation Foo
@synthesize qux = _qux;

- (void)dealloc {
  [_qux release];
  [super dealloc];
}

@end
This actually synthesizes an instance variable on Foo called _qux, which is accessed by getter and setter messages -qux and -setQux:.

I recommend against this: it's a little messy, but there's one good reason to use the underscore; namely, to protect against accidentally direct ivar access. If you think you can trust yourself to remember whether you're using a raw instance variable or an accessor method, just do it like this instead:

@interface Foo : Bar
@property (retain) Baz *qux;
@end

@implementation Foo
@synthesize qux;

- (void)dealloc {
  [qux release];
  [super dealloc];
}

@end
Then, when you want to access the instance variable directly, just say qux (which translates to self->qux in C syntax for accessing a member from a pointer). When you want to use accessors methods (which will notify observers, and do other interesting things, and make things safer and easier with respect to memory management), use self.qux ([self qux]) and self.qux = blah; ([self setQux:blah]).

The sad thing here is that Apple's sample code and template code sucks. Never use it as a guide to proper Objective-C style, and certainly never use it as a guide to proper software architecture. :)

[why-rename-synthesized-properties-in-ios-with-leading-underscores](http://stackoverflow.com/questions/5466496/why-rename-synthesized-properties-in-ios-with-leading-underscores)