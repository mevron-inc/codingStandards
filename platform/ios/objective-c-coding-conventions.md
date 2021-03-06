# Objective-C Coding Conventions

1. [Identifiers](#identifiers)
1. [Naming classes, protocols, categories](#naming-classes-protocols-categories)
1. [Variables](#variables)
1. [Methods](#methods)
1. [Preprocessor Macros](#preprocessor-macros)
1. [Functions](#functions)
1. [Constants](#constants)
1. [Enumerations](#enumerations)
1. [Types Ans Structures](#types-and-structures)
1. [Notifications](#notifications)
1. [Class Fields](#class-fields)
1. [Class Properties](#class-properties)
1. [Indents](#indents)
1. [Long Lines](#long-lines)
1. [Singletons](#singletons)
1. [Braces](#braces)
1. [Pointers](#pointers)
1. [Blocks](#blocks)
1. [Project Structure](#project-structure)
1. [Autogenerated Code](#autogenerated-code)
1. [Grouping Methods](#grouping-methods)
1. [References](#references)


## Identifiers

Identifiers (variable names, class names, functions, constants, types, and other elements of the source code) must be clear and without abbreviations. 

Exceptions are described [here](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html) (also `database` may be shortened to `db`).

Use _camelNotation_ if the identifier is more than one word. No underscores, except as noted below.

**Bad:** `pushNotif`, `is_sel`

**Good:** `pushNotification`, `isSelected`


## Naming classes, protocols, categories

Class, protocol and category names must consist of words and/or generally accepted abbreviations (see above), each of which begins with a capital letter. If the class is not related to MVC/GUI, and implements some specific (business) logic or functionality, or we develop a library/framework, the prefix of the capital letters should be added.

**Bad**: `MainVC` (for controller), `ContactTableContent` (for table cell), `CustomSlider` (inherited from `UIView`) 

**Good**: `MainViewController`, `ContactTableViewCell/ContactCell`, `CustomSliderView`

_For framework_:

**Bad**: `ViewController`

**Good**: `MLSViewController`

_For Facebook_:

**Bad**: `LoginFacebook`, `FacebookWallMessage`

**Good**: `FBLoginHelper`, `FBWallMessageService` _(something like that ;))_

_For RESTful web services:_

**Bad**: `RestfulRequest`, `RestWebServiceResponse`

**Good**: `RESTService`, `RESTRequest`, `RESTResponse`


The names of the base classes, which, logically, are abstract (although the latter are not supported in Objective-C), must start with the word _Base_. For example, `BaseSearchEngine`, `BaseViewController`. 

Ideally, the abstract method bodies should not be empty. Those methods should throw exceptions instead.

```objc
@interface BaseSearchEngine 

- (void)search:(Query *)aQuery;

@end


@implementation BaseSearchEngine

- (void)search:(Query *)aQuery 
{
    @throw [NSException exceptionWithName:@"This method is abstract!" 
                                   reason:@"You should not invoke "\
                                          "this method." 
                                 userInfo:nil];
}

@end
```


### File Names

File names must match the names of the classes (interfaces, protocols). As for categories, it's the individual case described below.


### Protocols

Protocols for _delegate_, _dataSource_ and other interfaces should be named with the interface name and the word _Delegate_, _DataSource_, etc. at the end. If you can not figure out the last word, you should use the word _Protocol_.

Should you extract protocols into separate header files? Yes, if interface declarations are too large.

```objc
@interface MainViewController -> @protocol MainViewControllerDelegate
@interface ClientTableView -> @protocol ClientTableViewDataSource
@interface AsynchronousImageProcessor -> @protocol AsynchronousImageProcessorProtocol
```

### Categories

Category must have a clear title, if you extend the functionality of a particular class. For example, 

```objc
@interface NSImage(Base64)
```

If you can not describe all added functionality in one word, use the word `Extra`.


```objc
@interface NSImage(Base64)
@interface NSData(Extra)
```

File names for categories: `NSImage+Base64.h (.m), NSData+Extra.h (.m)`


## Variables

Use _camelNotation_ with the first word lowecased.


## Methods

Read [this](http://cocoadevcentral.com/articles/000082.php#6) and [this](http://cocoadevcentral.com/articles/000083.php#1).

Regarding the spaces in method declarations, follow Apple official documentation and Xcode-generated code:

```objc
- (NSArray *)arrayByAddingObjectsFromArray:(NSArray *)otherArray;
```

Put a space before return value type (after _+/-_) and also in a parentheses, if the parameter is a pointer. Don't put spaces after parentheses.

**Bad**:

```objc
+(UIColor*)colorWithPatternImage:(UIImage*)image;
+ (UIColor *) colorWithWhite:(CGFloat) white alpha:(CGFloat) alpha;
```

**Good**:

```objc
+ (UIColor *)colorWithPatternImage:(UIImage *)image;
+ (UIColor *)colorWithWhite:(CGFloat)white alpha:(CGFloat)alpha;
```


## Preprocessor Macros

Preprocessor directives are `#define`, `#ifdef` and things like that.

For macro names use the whole words without abbreviations (except for common acronyms, see above), and underscores.


**Bad**: `debug`, `distrConfig`

**Good**: `DEBUG`, `DISTRIBUTION_CONFIGURATION`

Example:

```objc
#ifndef DEBUG
#define DISTRIBUTION_CONFIGURATION
#endif
```


## Functions

Use the same rules as for [class names](#naming-classes-protocols-categories).


## Constants

Start with a lowercase letter _k_, continue all the words with a first capital letter.
Abbreviations should be uppercased.

**Bad**: `ST_BAR_TITLE`, `CELL_NAME`

**Good**: `kStatusBarTitle`, `kCellName`, `kRESTWebService`


## Enumerations

Use the same rules as for [class names](#naming-classes-protocols-categories).


## Types Ans Structures

Use the same rules as for [class names](#naming-classes-protocols-categories).


## Notifications

Same as for class names, but with the word _Notification_ at the end. 

**Bad**: 

```objc
const NSString* MLSNotifySalary = @"MLSNotifySalary";
```

**Good**: 
```objc
const NSString* MLSSalaryNotification = @"MLSSalaryNotification";
```


## Class Fields

Class fields are _camelCased_ [identifiers](#identifiers) with lowercased first letter.

```objc
@interface A 
{
    NSInteger fieldName;
}
@end
```


## Class Properties

Class fields are _camelCased_ [identifiers](#identifiers) with lowercased first letter.
**Even CoreData table field names!**

```objc
@interface User : NSManagedObject 

@property (nonatomic, retain) NSNumber* userId;

@end

```


## Indents

Don't use tabs.

Use 4 spaces. 

Line length should not exceed 100 characters.


## Long Lines

When you declare, define or call a method with a long list of parameters, align the names of the arguments by colon (one line - one parameter). 

**Bad**:

```objc
- (NSColor *)colorWithCalibratedHue:(CGFloat)aHue saturation: (CGFloat)aSaturation brightness:(CGFloat)aBrightness alpha:(CGFloat)anAlpha;
```

**Good**:

```objc
- (NSColor *)colorWithCalibratedHue:(CGFloat)aHue 
                         saturation:(CGFloat)aSaturation 
                         brightness:(CGFloat)aBrightness 
                              alpha:(CGFloat)anAlpha; 
```

Do not make nested calls methods in such cases, it is better to store the result of a method call in a local variable.

**Bad**:

```objc
[view setBackgroundColor:[NSColor colorWithCalibratedHue:0.10 saturation:0.82 brightness:0.89 alpha:1.0]];
```

**Good**: 

```objc
NSColor *color = [NSColor colorWithCalibratedHue:0.10 
                                      saturation:0.82 
                                      brightness:0.89 
                                           alpha:1.00];

[view setBackgroundColor: color];
```

It is allowed to leave multiple parameters on a single line in the case of standard methods usage. Here, the first parameter of each line must be aligned to the colon of the first parameter.

```objc
[NSTimer scheduledTimerWithTimeInterval:kTimeInterval target:self
                               selector:@selector(fireMethod)
                               userInfo:nil repeats:YES];
```


## Singletons

Call a singleton instance access method `sharedInstance`:

```objc
@interface MyCoolSingleton 

+ (id)sharedInstance;

@end

...

MyCoolSingleton *coolSingleton = 
        (MyCoolSingleton *)[MyCoolSingleton sharedInstance];
```


## Braces

An opening brace should _always_ be on a new line, no exceptions:

```objc
@interface Person : NSObject
{
    NSString *name;
}
```

```objc
+ (Person *)personWithName:(NSString *)personName 
{
    if (personName == nil)
    {
        return nil;
    }
    // ...
}
```

Always use braces in conditional statements, even if there is only one operator: 

```objc
if (person)
{
    [person release];
}
```


## Pointers

When using pointers, always put a space between class name and asterisk.

**Bad**: 

```objc
Person* person = ...;
```

**Good**:

```objc
Person *person = ...;
```

**Bad**:

```objc
+ (Person*)personWithName:(NSString*)personName;
```

**Good**:

```objc
+ (Person *)personWithName:(NSString *)personName; 
````


## Blocks

If you need to pass a block as a value of the parameter when calling the function (sending a message), it is recommended to define a variable:

```objc
void(^success)(NSURLRequest *,NSHTTPURLResponse *,id) =
    ^(NSURLRequest *request, NSHTTPURLResponse *response, id JSON) 
{
    // ...
};

[AFJSONRequestOperation JSONRequestOperationWithRequest:request
                                                success:success
                                                failure:nil];
```
                                                
However, small blocks may be passed without additional variable usage:

```objc
[UIView animateWithDuration:kAnimationDuration
                 animations:^{
                     [someView setAlpha:1.f];
                 }];
```


## Project Structure

The structure of the project should be simple and clear. Classes should be grouped by purpose or pattern the class implements.

Root group classes should be in the following order: 
* Models
* Controllers
* Views
* Managers
* Factories
* Builders
* Helpers
* Wrappers
* Categories (all `NS*` and `UI*` class categories, and also categories for third-party classes),
* AppDelegate
* Resources
* Supporting Files

**Bad**:

<img src="/platform/ios/bad-structure.png" alt="Bad Project Structure">

**Good**:

<img src="/platform/ios/good-structure.png" alt="Good Project Structure">

The next level of grouping is the _meaning_ of class/interface etc. For example, controllers must be grouped by application screens.

The folders in a file system (on hard drive) must have the same structure as the Xcode project.


## Autogenerated Code

When you create a new class in Xcode (controller, for example), remove all the autogenerated crap if you are not going to override methods right now.

**There should be no such things in a source code**:

```objc
- (void)didReceiveMemoryWarning {
    // Releases the view if it doesn't have a superview.
    [super didReceiveMemoryWarning];
    
    // Release any cached data, images, etc that aren't in use.
}

- (void)viewDidUnload {        
    // Release any retained subviews of the main view.
    // e.g. self.myOutlet = nil;
}
```


## Grouping Methods

To improve the readability of the code (and navigation through it), use the following macro:

```objc
#pragma mark - %group name%
```

For example, 

```objc
@implementation MarkViewController

#pragma mark - lifecycle

- (id)init;

- (void)dealloc;

#pragma mark - view lifecycle

- (void)viewDidLoad;

- (void)viewWillAppear:(BOOL)animated;

- (void)viewDidAppear:(BOOL)animated;

- (void)viewWillDisappear:(BOOL)animated;

- (void)viewDidDisappear:(BOOL)animated;

#pragma mark - appearance

- (NSUInteger)supportedInterfaceOrientations;

- (BOOL)prefersStatusBarHidden;

- (void)viewWillLayoutSubviews

- (void)viewDidLayoutSubviews

#pragma mark - public methods

- (NSString *)getStringFromData:(NSData *)data;

#pragma mark - actions

- (IBAction)buttonPressed;

#pragma mark - private methods

- (void)prepareTheKraken;

#pragma mark - delegate methods

- (void)gotResponseFromObject:(id)object withData:(NSDAta *)data;

@end
```


## References

* [Introduction to Coding Guidelines for Cocoa](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [Cocoa Style for Objective-C: Part I](http://cocoadevcentral.com/articles/000082.php)
* [Cocoa Style for Objective-C: Part II](http://cocoadevcentral.com/articles/000083.php)
* [Google Objective-C Style Guide](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
