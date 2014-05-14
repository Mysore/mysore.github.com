---
title: The Horror: GCC and glibc
layout: post
---
> ron minnich <rminnich@gmail.com>	 Wed, May 14, 2014 at 9:46 AM
> Reply-To: akaros@lists.eecs.berkeley.edu
> To: akaros <akaros@lists.eecs.berkeley.edu>
> I just love the GNU toolchain. Macro abuse, bad programming practice,
> all on a grand scale.
> 
> Now I have this:
> 
> ../../binutils-2.21.1/bfd/opncls.c: In function ‘bfd_fopen’:
> ./bfd.h:524:65: error: right-hand operand of comma expression has no
> effect [-Werror=unused-value]
>  #define bfd_set_cacheable(abfd,bool) (((abfd)->cacheable = bool), TRUE)
>                                                                  ^
> ../../binutils-2.21.1/bfd/opncls.c:249:5: note: in expansion of macro
> ‘bfd_set_cacheable’
>      bfd_set_cacheable (nbfd, TRUE);
>      ^
> cc1: all warnings being treated as errors
> 
> 
> Awful, .... anybody seen this? What do you do?
> 
> I'm getting lots of errors today and it seems I need to remake the
> world, but ...
> 
> 
> signed,
> Low in Livermore

> barret rhoden <brho@cs.berkeley.edu>	 Wed, May 14, 2014 at 9:55 AM
> Reply-To: akaros@lists.eecs.berkeley.edu
> To: akaros@lists.eecs.berkeley.edu
> Dear Low,
> 
> I take it you are rebuilding binutils and the toolchain?  Are you doing
> this on a new machine?  If so, what version of GCC and autoconf are you
> using?
> 
> The bit about warnings being treated as errors is interesting,
> considering how many warnings I usually see while building the
> toolchain, though perhaps not when building binutils.
> 
> Barret

> ron minnich <rminnich@gmail.com>	 Wed, May 14, 2014 at 9:59 AM
> Reply-To: akaros@lists.eecs.berkeley.edu
> To: akaros <akaros@lists.eecs.berkeley.edu>
> All I did was cd into a directory in which I had *already built* a
> working toolchain, and type make x86_64.
> 
> Which means, now that I think of it, that the problem is a recent
> upgrade to my native toolchain now breaks builds of our toolchain.
> 
> I just love this stuff. For the record, the plan 9 toolchain will work
> across the last 25 years of building Plan 9.
> 
> I'm glad I have Go. A future that involved only the gcc/glibc world is
> too awful to contemplate. I'd rather sell Tupperware.
> 
> Low

> Kevin Klues <klueska@eecs.berkeley.edu>	 Wed, May 14, 2014 at 10:04 AM
> Reply-To: akaros@lists.eecs.berkeley.edu
> To: akaros@lists.eecs.berkeley.edu
> Cc: ron minnich <rminnich@gmail.com>
> It sucks, but try doing a make clean in there AND wiping away your installation directory.

> ron minnich <rminnich@gmail.com>	 Wed, May 14, 2014 at 10:05 AM
> Reply-To: akaros@lists.eecs.berkeley.edu
> To: Kevin Klues <klueska@eecs.berkeley.edu>
> Cc: akaros <akaros@lists.eecs.berkeley.edu>
> Yep, make clean will cure what ails you. That's what I'm doing.
> 
> yikes.
> 
> ron

> ron minnich <rminnich@gmail.com>	 Wed, May 14, 2014 at 10:49 AM
> Reply-To: akaros@lists.eecs.berkeley.edu
> To: Kevin Klues <klueska@eecs.berkeley.edu>
> Cc: akaros <akaros@lists.eecs.berkeley.edu>
> Ah, brilliant, make clean helpeth not. Probably because this is an
> older glibc and a newer gcc, and the heavens forbid actually being
> able to compile old glibc cpp abuse with new code.
> 
> OK, I'll try to see where I can get.
> 
> ron


> ron minnich <rminnich@gmail.com>	 Wed, May 14, 2014 at 10:52 AM
> Reply-To: akaros@lists.eecs.berkeley.edu
> To: Kevin Klues <klueska@eecs.berkeley.edu>
> Cc: akaros <akaros@lists.eecs.berkeley.edu>
> What I really like is the way the bfd.h guys took a simple function
> that might be called once and turned it into a cpp macro to do ..
> what, precisely?
> 
> And in so doing created a horrible error situation just waiting to
> bite you. I've fixed it, but I can't look in a mirror.
> 
> ron
