# Shared Source Code #

In this repository is code that we share between many projects that we work on.  It is licensed under the BSD license and is free to use as you wish. 

## Prefix.pch ##

You will probably find macros in this code that does not compile.  The reason is that we have several macros that we add to our Prefix.pch file upon project creation.  Those macros are as follows:

    #ifdef DEBUG
      #define MCRelease(x) [x release]
      #define DLog(...) NSLog(@"%s(%p) %@", __PRETTY_FUNCTION__, self, [NSString stringWithFormat:__VA_ARGS__])
      #define DCLog(...) NSLog(@"%@", [NSString stringWithFormat:__VA_ARGS__])
      #define ALog(...) {NSLog(@"%s(%p) %@", __PRETTY_FUNCTION__, self, [NSString stringWithFormat:__VA_ARGS__]);[[NSAssertionHandler currentHandler] handleFailureInFunction:[NSString stringWithCString:__PRETTY_FUNCTION__ encoding:NSUTF8StringEncoding] file:[NSString stringWithCString:__FILE__ encoding:NSUTF8StringEncoding] lineNumber:__LINE__ description:__VA_ARGS__];}
    #else
      #define MCRelease(x) [x release], x = nil
      #define DLog(...) do { } while (0)
      #define DCLog(...) do { } while (0)
      #ifndef NS_BLOCK_ASSERTIONS
        #define NS_BLOCK_ASSERTIONS
      #endif
      #define ALog(...) NSLog(@"%s(%p) %@", __PRETTY_FUNCTION__, self, [NSString stringWithFormat:__VA_ARGS__])
    #endif

    #define ZAssert(condition, ...) do { if (!(condition)) { ALog(__VA_ARGS__); }} while(0)

    #define ISRETINADISPLAY (([[UIScreen mainScreen] respondsToSelector:@selector(scale)]) ? [[UIScreen mainScreen] scale] > 1.0 : NO)

    #define degreesToRadians(x) (M_PI * x / 180.0)

Adding these macros to your Prefix.pch will resolve any compile issues.