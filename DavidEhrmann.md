= What I'm doing here = I've been a little frustrated with the both
sparse and unorganized documentation here, for a while, so I'm going to
try to slowly clean it up. = My devices = \*
\[:OpenWrtDocs/Hardware/Linksys/WRT54GL:Linksys WRT54GL\] \*
\[:OpenWrtDocs/Hardware/Netgear/DG834G: Netgear DG834\] \*
\[:OpenWrtDocs/Hardware/Netgear/WGT634UNetgear: WGT634U\] \* Some
PCEngines board. I can't remember which one.

= Wiki! = I'm not sure what it should ultimately look like, but there
are a few problems I'm trying to address, like somewhat duplicated
howtos, missing category tags, and painfully out of date pages. Probably
one of the biggest observations is that pages listing add-on packages
and modifications are best left to categories. Pages listing just a few
modifications quickly become a dumping ground for links to others--even
basic configuration pages collect links useful to only a handful of
users.

I'd like a structure somewhat like this:

-   

    OpenWrtDocs/

    :   -   Kamikaze/
            \* Pages specific to Kamikaze
        -   WhiteRussian/
            \* Pages specific to White Russian
        -   Hardware/
            -   &lt;manufacturer&gt;/
                \* Model-specific pages

            \* Customizations/
        -   Packages/
            \* &lt;package name&gt;: package documentation
        -   Installation
        -   Upgrading
        -   Removing

But it's still changing...

= A few external links =

:   -   \[<http://s3mp.org> s3mp\]: My Amazon S3-based mp3 player. Extra
        pre-beta.
    -   \[<http://www.stuffengineerslike.com> Stuff Engineers Like\]

----CategoryHomepage
