# MyPreprocessor
A better header file / preprocessor directive utility.

As you may know, calling a UDF is slow. Calling a method is even slower.  The fastest way is to hard code, but that means tons of copied code, carved in stone. What is possible is to use #DEFINE to create an expression that is injected at compile time. This has limits. It cannot inject more than a single line of code at a time. It cannot accept parameters, like a UDF. I did something called a SnippetFactory back in the day, and while that was dynamic coding, and it did allow parameters, I hated having to use & or execscript to execute the code at run time.

What if we had a third option? GoFish has the power to provide that third option, but maybe, just maybe RegExp can do it.

Here's the simple example: USE IN IIF(USED('SOMEALIAS'),SELECT('SOMEALIAS'),0). I am, once again, perturbed that we cannot upgrade from THAT to the more efficient USE IN (SELECT(SOME'ALIAS')) throughout an application with ease.

There is a way, but it does not accept parameters, so it's not as useful as it could be.

kxCloseAlias.h
#IFNDEF kxCloseAlias
#define kxCloseAlias USE IN (SELECT(m.lcAlias))
#ENDIF

Then in your code somewhere....

#include "kxCloseAlias.h"
lcAlias = "SOMEALIAS"
kxCloseAlias

That is more typing than it should be. You must then also create lcAlias as a variable before executing the kxCloseAlias preprocessor directive. That's worse because now it's 2 lines of code per call.

Ideally, we would have a single piece of code, like a udf, yet it would recurse to expand other of this 'directives' and we'd be well off if we could trace the expanded code.

So, assuming we wrote @@CloseAlias('SomeAlias'), and at compile time, this was search and replaced to "USE IN (SELECT('ALIAS'))", the net effect would be a single file to support, CloseAlias.MY - :) - which would contain...
