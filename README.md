# TTGTagCollectionView

[![CI Status](http://img.shields.io/travis/zekunyan/TTGTagCollectionView.svg?style=flat)](https://travis-ci.org/zekunyan/TTGTagCollectionView)
[![Version](https://img.shields.io/cocoapods/v/TTGTagCollectionView.svg?style=flat)](http://cocoapods.org/pods/TTGTagCollectionView)
[![License](https://img.shields.io/cocoapods/l/TTGTagCollectionView.svg?style=flat)](http://cocoapods.org/pods/TTGTagCollectionView)
[![Platform](https://img.shields.io/cocoapods/p/TTGTagCollectionView.svg?style=flat)](http://cocoapods.org/pods/TTGTagCollectionView)
[![Apps Using](https://img.shields.io/badge/Apps%20Using-%3E%201,096-blue.svg)](https://github.com/zekunyan/TTGTagCollectionView)
[![Total Download](https://img.shields.io/badge/Total%20Download-%3E%2068,461-blue.svg)](https://github.com/zekunyan/TTGTagCollectionView)

![Screenshot](https://github.com/zekunyan/TTGTagCollectionView/raw/master/Resources/screen_shot.png)

![Alignment Type](https://github.com/zekunyan/TTGTagCollectionView/raw/master/Resources/alignment_type.png)

## What 

TTGTagCollectionView is useful for showing different size tag views in a vertical or horizontal scrollable view. And if you only want to show text tags, you can use TTGTextTagCollectionView instead, which has more simple api. At the same time, It is highly customizable that many features of the text tag can be configured, like the tag font size and the background color.

## Features
* Both text tag and custom view tag supported.
* Highly customizable, each text tag can be configured.
* Vertical and horizontal scrollable.
* Support different kinds of alignment types.
* Support specifying number of lines.
* Support Autolayout `intrinsicContentSize` to auto determine height based on content size.
* Support pull to refresh, like `SVPullToRefresh`.
* Use `preferredMaxLayoutWidth` to set available width like UIlabel.

## Demo
You can find demos in the `Example->TTGTagCollectionView.xcworkspace` project.

![Example project](https://github.com/zekunyan/TTGTagCollectionView/raw/master/Resources/demo_example.png)

## Requirements
iOS 7 and later.

## Installation

### CocoaPods
TTGTagCollectionView is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "TTGTagCollectionView"
```

### Carthage
You can use [Carthage](https://github.com/Carthage/Carthage) to install `TTGTagCollectionView` by adding it to your `Cartfile`:
```
github "zekunyan/TTGTagCollectionView"
```

## Usage
### TTGTextTagCollectionView
Use TTGTextTagCollectionView to show text tags.

#### Basic usage
```Objective-C
TTGTextTagCollectionView *tagCollectionView = [[TTGTextTagCollectionView alloc] initWithFrame:CGRectMake(20, 20, 200, 200)];
[self.view addSubview:tagCollectionView];
[tagCollectionView addTags:@[@"TTG", @"Tag", @"collection", @"view"]];

```

#### Delegate
Conform the `TTGTextTagCollectionViewDelegate` protocol to get callback when you select the tag or content height changes.

```Objective-C
@protocol TTGTextTagCollectionViewDelegate <NSObject>
@optional
- (BOOL)textTagCollectionView:(TTGTextTagCollectionView *)textTagCollectionView
                    canTapTag:(NSString *)tagText
                      atIndex:(NSUInteger)index
              currentSelected:(BOOL)currentSelected
                    tagConfig:(TTGTextTagConfig *)config;

- (void)textTagCollectionView:(TTGTextTagCollectionView *)textTagCollectionView
                    didTapTag:(NSString *)tagText
                      atIndex:(NSUInteger)index
                     selected:(BOOL)selected
                    tagConfig:(TTGTextTagConfig *)config;

- (void)textTagCollectionView:(TTGTextTagCollectionView *)textTagCollectionView
            updateContentSize:(CGSize)contentSize;
@end
```

#### Customization
Each tag can be configured.

```Objective-C
@interface TTGTextTagConfig : NSObject

// Text font
@property (strong, nonatomic) UIFont *textFont;

// Text color
@property (strong, nonatomic) UIColor *textColor;
@property (strong, nonatomic) UIColor *selectedTextColor;

// Background color
@property (strong, nonatomic) UIColor *backgroundColor;
@property (strong, nonatomic) UIColor *selectedBackgroundColor;

// Gradient background color
@property (assign, nonatomic) BOOL enableGradientBackground;
@property (strong, nonatomic) UIColor *gradientBackgroundStartColor;
@property (strong, nonatomic) UIColor *gradientBackgroundEndColor;
@property (strong, nonatomic) UIColor *selectedGradientBackgroundStartColor;
@property (strong, nonatomic) UIColor *selectedGradientBackgroundEndColor;
@property (assign, nonatomic) CGPoint gradientBackgroundStartPoint;
@property (assign, nonatomic) CGPoint gradientBackgroundEndPoint;

// Corner radius
@property (assign, nonatomic) CGFloat cornerRadius;
@property (assign, nonatomic) CGFloat selectedCornerRadius;
@property (assign, nonatomic) Boolean cornerTopRight;
@property (assign, nonatomic) Boolean cornerTopLeft;
@property (assign, nonatomic) Boolean cornerBottomRight;
@property (assign, nonatomic) Boolean cornerBottomLeft;

// Border
@property (assign, nonatomic) CGFloat borderWidth;
@property (assign, nonatomic) CGFloat selectedBorderWidth;
@property (strong, nonatomic) UIColor *borderColor;
@property (strong, nonatomic) UIColor *selectedBorderColor;

// Shadow.
@property (nonatomic, copy) UIColor *shadowColor;    // Default is [UIColor black]
@property (nonatomic, assign) CGSize shadowOffset;   // Default is (2, 2)
@property (nonatomic, assign) CGFloat shadowRadius;  // Default is 2f
@property (nonatomic, assign) CGFloat shadowOpacity; // Default is 0.3f

// Extra space in width and height, will expand each tag's size
@property (assign, nonatomic) CGSize extraSpace;

// Max width for a text tag. 0 and below means no max width.
@property (assign, nonatomic) CGFloat maxWidth;
// Min width for a text tag. 0 and below means no min width.
@property (assign, nonatomic) CGFloat minWidth;

// Exact width. 0 and below means no work
@property (nonatomic, assign) CGFloat exactWidth;
// Exact height. 0 and below means no work
@property (nonatomic, assign) CGFloat exactHeight;

// Extra data. You can use this to bind any object you want to each tag.
@property (nonatomic, strong) NSObject *extraData;

@end
```

You can also configure scroll direction, alignment, lines limit, spacing and inset.

```Objective-C
// Define if the tag can be selected.
@property (assign, nonatomic) BOOL enableTagSelection;

// Tags scroll direction, default is vertical.
@property (nonatomic, assign) TTGTagCollectionScrollDirection scrollDirection;

// Tags layout alignment, default is left.
@property (nonatomic, assign) TTGTagCollectionAlignment alignment;

// Number of lines. 0 means no limit, default is 0 for vertical and 1 for horizontal.
@property (nonatomic, assign) NSUInteger numberOfLines;

// Tag selection limit, default is 0, means no limit
@property (nonatomic, assign) NSUInteger selectionLimit;

// Horizontal and vertical space between tags, default is 4.
@property (assign, nonatomic) CGFloat horizontalSpacing;
@property (assign, nonatomic) CGFloat verticalSpacing;

// Content inset, default is UIEdgeInsetsMake(2, 2, 2, 2).
@property (nonatomic, assign) UIEdgeInsets contentInset;

// The true tags content size, readonly
@property (nonatomic, assign, readonly) CGSize contentSize;

// Manual content height
// Default = NO, set will update content
@property (nonatomic, assign) BOOL manualCalculateHeight;
// Default = 0, set will update content
@property (nonatomic, assign) CGFloat preferredMaxLayoutWidth;

// Scroll indicator
@property (nonatomic, assign) BOOL showsHorizontalScrollIndicator;
@property (nonatomic, assign) BOOL showsVerticalScrollIndicator;
```

Alignment types:

```Objective-C
typedef NS_ENUM(NSInteger, TTGTagCollectionAlignment) {
    TTGTagCollectionAlignmentLeft = 0,                           // Default
    TTGTagCollectionAlignmentCenter,                             // Center
    TTGTagCollectionAlignmentRight,                              // Right
    TTGTagCollectionAlignmentFillByExpandingSpace,               // Expand horizontal spacing and fill
    TTGTagCollectionAlignmentFillByExpandingWidth,               // Expand width and fill
    TTGTagCollectionAlignmentFillByExpandingWidthExceptLastLine, // Expand width and fill, except last line
};
```

#### Config tags

Add tag.

```Objective-C
// Add tag with detalt config
- (void)addTag:(NSString *)tag;
- (void)addTags:(NSArray <NSString *> *)tags;

// Add tag with custom config
- (void)addTag:(NSString *)tag withConfig:(TTGTextTagConfig *)config;
- (void)addTags:(NSArray <NSString *> *)tags withConfig:(TTGTextTagConfig *)config;
```

Insert tag.

```Objective-C
// Insert tag with default config
- (void)insertTag:(NSString *)tag atIndex:(NSUInteger)index;
- (void)insertTags:(NSArray <NSString *> *)tags atIndex:(NSUInteger)index;

// Insert tag with custom config
- (void)insertTag:(NSString *)tag atIndex:(NSUInteger)index withConfig:(TTGTextTagConfig *)config;
- (void)insertTags:(NSArray <NSString *> *)tags atIndex:(NSUInteger)index withConfig:(TTGTextTagConfig *)config;
```

Remove tag.

```Objective-C
// Remove tag
- (void)removeTag:(NSString *)tag;
- (void)removeTagAtIndex:(NSUInteger)index;
- (void)removeAllTags;
```

#### Config tag selection

```Objective-C
- (void)setTagAtIndex:(NSUInteger)index selected:(BOOL)selected;
```

#### Update tag style config

```Objective-C
// Update tag config
- (void)setTagAtIndex:(NSUInteger)index withConfig:(TTGTextTagConfig *)config;
- (void)setTagsInRange:(NSRange)range withConfig:(TTGTextTagConfig *)config;
```

#### Get tag information

```Objective-C
// Get tag
- (NSString *)getTagAtIndex:(NSUInteger)index;
- (NSArray <NSString *> *)getTagsInRange:(NSRange)range;

// Get tag config
- (TTGTextTagConfig *)getConfigAtIndex:(NSUInteger)index;
- (NSArray <TTGTextTagConfig *> *)getConfigsInRange:(NSRange)range;

// Get all
- (NSArray <NSString *> *)allTags;
- (NSArray <NSString *> *)allSelectedTags;
- (NSArray <NSString *> *)allNotSelectedTags;
```

#### Reload
You can reload tags programmatically.

```Objective-C
- (void)reload;
```

#### Index at point
Returns the index of the tag located at the specified point.

```Objective-C
- (NSInteger)indexOfTagAt:(CGPoint)point;
```

### TTGTagCollectionView
Use `TTGTagCollectionView` to show custom tag views.

#### DataSource and Delegate
Just like the UITableView, you must conform and implement the required methods of `TTGTagCollectionViewDelegate` and `TTGTagCollectionViewDataSource` to get `TTGTagCollectionView` work.

**DataSource**

```Objective-C
@protocol TTGTagCollectionViewDataSource <NSObject>
@required
- (NSUInteger)numberOfTagsInTagCollectionView:(TTGTagCollectionView *)tagCollectionView;

- (UIView *)tagCollectionView:(TTGTagCollectionView *)tagCollectionView tagViewForIndex:(NSUInteger)index;
@end
```

**Delegate**

```Objective-C
@protocol TTGTagCollectionViewDelegate <NSObject>
@required
- (CGSize)tagCollectionView:(TTGTagCollectionView *)tagCollectionView sizeForTagAtIndex:(NSUInteger)index;

@optional
- (BOOL)tagCollectionView:(TTGTagCollectionView *)tagCollectionView shouldSelectTag:(UIView *)tagView atIndex:(NSUInteger)index;

- (void)tagCollectionView:(TTGTagCollectionView *)tagCollectionView didSelectTag:(UIView *)tagView atIndex:(NSUInteger)index;

- (void)tagCollectionView:(TTGTagCollectionView *)tagCollectionView updateContentSize:(CGSize)contentSize;
@end
```

#### Customization

```Objective-C
// Tags scroll direction, default is vertical.
@property (nonatomic, assign) TTGTagCollectionScrollDirection scrollDirection;

// Tags layout alignment, default is left.
@property (nonatomic, assign) TTGTagCollectionAlignment alignment;

// Number of lines. 0 means no limit, default is 0 for vertical and 1 for horizontal.
@property (nonatomic, assign) NSUInteger numberOfLines;

// Horizontal and vertical space between tags, default is 4.
@property (nonatomic, assign) CGFloat horizontalSpacing;
@property (nonatomic, assign) CGFloat verticalSpacing;

// Content inset, default is UIEdgeInsetsMake(2, 2, 2, 2).
@property (nonatomic, assign) UIEdgeInsets contentInset;

// The true tags content size, readonly
@property (nonatomic, assign, readonly) CGSize contentSize;

// Manual content height
// Default = NO, set will update content
@property (nonatomic, assign) BOOL manualCalculateHeight;
// Default = 0, set will update content
@property (nonatomic, assign) CGFloat preferredMaxLayoutWidth;

// Scroll indicator
@property (nonatomic, assign) BOOL showsHorizontalScrollIndicator;
@property (nonatomic, assign) BOOL showsVerticalScrollIndicator;
```

#### Reload
You can reload tags programmatically.

```Objective-C
- (void)reload;
```

#### Index at point
Returns the index of the tag located at the specified point.

```Objective-C
- (NSInteger)indexOfTagAt:(CGPoint)point;
```

## Fix

`UITableViewAutomaticDimension` may not work when using tagView in tableViewCell. You should reload your tableView in the `viewDidAppear`.

## Author

zekunyan, zekunyan@163.com

## License

TTGTagCollectionView is available under the MIT license. See the LICENSE file for more info.


