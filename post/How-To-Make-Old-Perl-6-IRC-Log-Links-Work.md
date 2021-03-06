%% title: How To Make Old #perl6 IRC Log Links Work
%% date: 2018-06-06
%% desc: Find out how where PerlGeek.De IRC log links link to

As you may know, recent European legislation
[resulted](https://twitter.com/nogoodnickleft/status/1003717165256728578)
in our primary #perl6 channel logger going offline.

Our [secondary logger](http://colabti.org/irclogger/irclogger_log/perl6) is
still available with a significant portion of the primary logger's data.
However, the biggest problem is there are still a ton of links to the primary
logger in things like [commit messages](https://github.com/search?q=irclog.perlgeek.de&type=Commits) and
[Issues](https://github.com/search?q=irclog.perlgeek.de&type=Issues).

How to figure out what messages those links refer to now? I'm gonna tell you.

## Log Mapper

The owner of the primary logger was kind enough to provide me a dump SHA1
hashes of #perl6 channel messages, together with their database IDs. The
IDs are used as hash part of the URLs that link to specific lines in the old
log. The URLs also contain the date and the channel.

Given all of that info, the log mapper looks up the SHA hash in the database,
then goes to the new loger and looks which messages for that date in that
channel hashes to the same SHA hash. From that, the mapper knows which
line on the *new logger* to link to, and it redirects there.

So, say, we want to know what discussion [message of commit `792e1fa`](https://github.com/perl6/doc/commit/792e1facdc2785ca1bac6180aef03a5d543513e4)
links to. We take the original URL:

    https://irclog.perlgeek.de/perl6/2018-05-23#i_16195729

And change the domain to `irc.perl6.party`:

    https://irc.perl6.party/perl6/2018-05-23#i_16195729

The mapper does its thing, and redirects us to:

    http://colabti.org/irclogger/irclogger_log/perl6?date=2018-05-23#l138

Which is the message the commit links to.



<strike>In fact, if you add this to your hosts file (`/etc/hosts` on *nix;
`C:\Windows\System32\drivers\etc\hosts` on Windows):</strike>

    69.164.222.157 irclog.perlgeek.de

<strike>Then you can simply visit the original URL in your browser (confirm SSL
cert exception, because I couldn't figure out how to make it work with
proper SSL), and that's it. You can now just click the original URLs to logs
and get proper content.</strike>

**UPDATE: the above redirect is no longer needed. The old log site now
redirects to the mapper automatically.**


## What's Available

The available data is the intersection of sets of when the secondary logger
became active, which for `#perl6` channel is 2005-02-26, and when the primary
logger was disabled, which is around 2018-06-04.

<strike>Only the `#perl6` channel data is available. Despite the primary logger being
in operation in `#perl6-dev` and `#moarvm`, the secondary logger was enabled
in them only a few days ago, so sadly a lot of the more recentish dev commits
won't have working URLs.</strike>

**UPDATE: we're working on the final piece required to bring back the logs from
the rest of Perl&nbsp;6/MoarVM channels.**

## Conclusion

Some of the logs are now available. If more alternative loggers to map to are
available, I'm sure the owner of the now-disabled will dump the hashes for
other channels, so we can restore those logs too.

If you have those loggers, [ping me on Twitter](https://twitter.com/zoffix).
