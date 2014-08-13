Title: Command-t for vim fails on Mavericks (and how to fix it)
Date: 2014-01-07 00:53
Author: alex
Category: Technology
Slug: command-t-for-vim-fails-on-mavericks-and-how-to-fix-it

I recently had to rebuild my dotfiles and haven't done so for several
months, before I updated my Mac OSX to Mavericks. I got a nice little
error when I tried to use vim after building command-t:

Vim: Caught deadly signal SEGV  
Vim: Finished.  
Segmentation fault

Apparently Mavericks uses ruby 2.0 instead of 1.8. Vim was compiled with
Ruby1.8 so building with Ruby 2.0 fails. I fixed the issue by going to:

`cd /System/Library/Frameworks/Ruby.framework/Versions`

In that directory were both versions of Ruby and a "Current" symlink
pointing to 2.0. I deleted the symlink and created a new one to point to
1.8 and rebuilt command-t. Aha! It didn't work. It was missing the ruby
header file for 1.8. I fixed that by installing Xcode command line tools
using the terminal.

`xcode-select --install`

After that, I went back to build command-t and it was working again. I
also deleted the "Current" symlink and recreated a new one again to
point back to 2.0.

