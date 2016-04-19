## [Unify UI Font](http://blog.smilexiaofeng.com/blog/2015/11/26/ios-modify-all-font-type-in-app/)

    #define kAppFontTypeName @"HelveticaNeue-light"

In Code

    #import "UIFont+AppFont.h"
    
    @implementation UIFont (AppFont)
    
    #pragma clang diagnostic push
    #pragma clang diagnostic ignored "-Wobjc-protocol-method-implementation"
    
    + (UIFont *)systemFontOfSize:(CGFloat)fontSize {
        return [UIFont fontWithName:kAppFontTypeName  size:fontSize];
    }
    
    #pragma clang diagnostic pop
    
    @end
    

In Nib/Storyboard

    - (void)awakeFromNib

    #import "UILabel+OverrideBaseFont.h"
    #import "Aspects.h"

    @implementation UILabel (OverrideBaseFont)

    + (void)load {
        [[self class]aspect_hookSelector:@selector(awakeFromNib) withOptions:AspectPositionAfter usingBlock:^(id<AspectInfo> aspectInfo) {
            UILabel* instance = [aspectInfo instance];
            UIFont* font = [UIFont fontWithName:kAppFontTypeName size:instance.font.pointSize];
            instance.font = font;
        }error:nil];
    }
    
    @end