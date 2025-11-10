---
layout: post
title: "Apple Mail: lost mails, broken backups"
tags: writing
---

This is a story about how mails suddenly disappeared from Apple Mail (hosted on iCloud), how I got them back and how I found out that the export (backup) is completely broken in Apple Mail on macOS.

As for my setup, I host my e-mail on iCloud using the iCloud+ feature where I can use my own domain name (so I have the flexibility to move along when things stops working). I've had this domain for years and have about 20 years worth of e-mails. I use the official Apple Mail-clients, both on my various Macs and on my iPhone (and Apple Watch I guess).

## Things are missing

In September of 2025 I noticed that I was missing an important mail. I couldn't figure out what happened but I had a suspicion it might be Apple's fault (maybe because I read [Apple Photos App Corrupts Images](https://tenderlovemaking.com/2025/09/17/apple-photos-app-corrupts-images/) the night before with a lot of comments on Hacker News about sync being broken in weird ways) - I have not had this issue myself though. To my advantage, I had a MacBook Pro that was offline and I hoped the mail would still be there - also luckily, the computer was completely out of battery so no "app nap" syncing mail while being suspended.

I blocked the internet access for that Mac, booted it up and found the mail I was looking for! But I also found a lot of other mails compared to my iPhone and my other Mac that was synced. When looking at the count, I saw that a whopping 5,396 mails were missing. About 1/4th of all my emails were gone!

Luckily, I do have backups. Unfortunately, it was a while since I did my last backup. I proceeded to take a fresh backup (export) on my synced machine, one on the the offline machine, downloaded my latest backup and started comparing them.

My method of comparison was fairly simple, essentially I did `grep "^From " mbox`, normalized to lowercase, sorted the lines and did a diff. The result showed that everything was a big mess. All backups had partial information but there were hundreds of mails only present in either one of the three backup. No single backup could show a complete picture.

Maybe things had gotten a little corrupted over time and had recently gone completely haywire. I don't know why it happened now but my suspicion was that upgrading to iOS 26 may have triggered this since that release seems very buggy. I've also upgraded my work machine to MacOS 26 (Tahoe) but I don't use my private mail on that machine, although it is connected to iCloud and some general mail settings like Smart Mailboxes seem to sync in the client. My other Macs are running older versions (Sonoma and Sequioa) and can't be upgraded to Tahoe.

## Apple support

My initial knee-jerk reaction would be to write an angry blog, ditch Apple Mail / iCloud and move somewhere else. But I thought, I'm gonna give Apple the benefit of doubt. I've never contacted Apple support before - but I've had very bad experiences with other big tech companies' support.

I started with online chat, it started as it usually do: "Hi! My name is Shahbaz. I hope you are doing good.". No, not really, Shahbaz. But, he was helpful and the interaction was quick. He created a case id and escalated to senior advisors - but told me I'd have to call them, they can't help over chat. Ugh.

I called the Swedish number for Apple support, a young woman answered and remotely looked at my phone, mainly looking at settings, iCloud space usage and that I wasn't using any filters etc. I was not confident this would lead anywhere and she couldn't see any issues but after a while she said she would rebuild my mailbox - but I had to disable "advanced data protection" first which I did not like at all, but understandable I guess? I'm not really sure why it's needed though because [according to documentation](https://support.apple.com/guide/security/advanced-data-protection-for-icloud-sec973254c5f/web):

> Because of the need to interoperate with the global email, contacts, and calendar systems, iCloud Mail, Contacts, and Calendar aren’t end-to-end encrypted.

Pretty much as soon as she started the process I could see the mails starting to re-appear and thousands of them started downloading. I found the important mail I was missing!

## Broken export / backups

When the rebuilding / syncing was done I started a new export - now hoping to have a fresh, complete and uncorrupted backup (I followed the [Export mailboxes](https://support.apple.com/guide/mail/import-or-export-mailboxes-mlhlp1030/mac) as described by Apple's official documentation). When you do it you'll get a folder called something like `MailboxName.partial.mbox` Then you wait for an eternity until the `.partial` is removed, you can watch the mbox file inside it grow in size but there is absolutely no feedback in Apple Mail that it's doing any work, no progress bar, nothing. No indication if anything stops or hangs. 

After the export was done, I started diffing again but everything looked completely broken again and made no sense.

I found out that the `mbox` format has a limit of ~2.15 GB due to use of 32-bit integers. So I needed a way to split it into smaller pieces. What I did to solve this was to use "Smart Mailboxes" and split my mails into time ranges like "received before 2010", "between 2010-2020" etc (here I could go on a tangent about what timezone the received seems to use because that is also wild, but I digress...). I then hit export again on the smart mailbox. Nothing seemed to happen. Turns out, when you do this on a smart mailbox it looks like nothing happens. No `.partial`-folder is created. Absolutely zero feedback.

After "an eternity" a folder appeared called ` 2.mbox`. Yes, really, first a "blankspace", then the number 2 (or sometimes 3 if you do another, but never 1 because god knows why...), then `.mbox`. This seems to be the way it works. I could finally get a total size larger than 2.15 so I joined all my partial exports, normalized and diffed. The count of mails in the backup at this point was larger(!?) compared to what it said in Apple Mail, so surely I should have a complete backup now. Maybe Apple Mail was just grouping things or something?

But it still didn't match up. Everything still looked corrupted. I could track down specific mails that existed in older backups, but not in my new one - and the mail did exist in the Apple Mail client. It just wasn't exported for some reason.

So I tried to do an export on my offline Mac (that was not synced) using the same "Smart mailbox" method. It worked fine (seemingly), no errors, but the files created were tiny in comparison to my online Mac. No way it could contain all my emails. It made no sense. At this point I just surrendered to the fact that the export functionality in Apple Mail on macOS is completely broken and there's no way I could ever make it work reliably or trust it in any way. Things fail in different ways and you get no feedback whatsoever.

You should absolutely not use "export mailbox" in Apple Mail.

## Thunderbird

So I thought, let's try Thunderbird if it works better. So I downloaded it, entered my email account for "auto configuration" and Thunderbird crashed. I restarted it and it opened a prompt do donate money to them. I tried again and Thunderbird crashed again.

I moved on to manual setup, entered all the server settings, ports, ssl - then I created an app-specific password and as I added the password to Thunderbird, it cleared all server setting fields that I had painstakingly filled in. So I had to fill them in again, sigh...

But then it worked. However, I couldn't see many of my mailboxes. Found out that you have to "subscribe" to them in Thunderbird. Did that and mails started downloading. Problem now was that the mail count was much larger compared to Apple mail, several thousands more. Looking around, I could see that there were a lot of duplicates in Thunderbird. So It thought "that's great, Thunderbird is duplicating emails now", searched around and it seemed like a common issue and at this point I started losing my mind. It was late and I was tired.

After pulling myself together and Thunderbird was done downloading, I tried to do an export but seems like Thunderbird has the same size limitation, but at least they are up front about it!

![Thunderbird export](/images/apple_mail/thunderbird_export.png)

What can be done here is that you can actually copy the mailbox files manually from the Thunderbird folder by clicking "Open profile folder" as seen in the screenshot above. I did not dig much into it but it's probably something mbox-like, but I can see files larger than 2.15 GB so hopefully complete - but you know by this point, you need to make sure yourself.

However, I didn't like the duplication issue.

## offlineimap
Moving on, I started looking at cli solutions. I picked [offlineimap](https://www.offlineimap.org), set it up and started running it. At this point I noticed that offlineimap and Thunderbird actually agreed on the mail count, so maybe there was actually duplicates but Apple Mail was hiding them. 

I enabled the iCloud web ui, logged in to my mail via the browser and it actually also agreed with Thunderbird and offlineimap about the mail count! Turns out Apple Mail on macOS is actually hiding duplicates and excluding them from the total mail count (but they are included in the broken exports).

![Agree or disagree](/images/apple_mail/agree_disagree.png)
_Apple Mail (Web), Thunderbird, Thunderbird backup files, offlineimap all agrees on the count. Apple Mail on Mac does not._

Finally, now I know the correct mail count, and I can use offlineimap or Thunderbird to download all of them, and I can verify the expected count in the backup and make a diff that actually works.

## Unread count
While the total count was under control, the unread count was off - and I did the mistake of trying to understand it. (Look at the screenshot above and it wil say 42 unread in Apple Mail on the web and 29 unread in the Apple Mail on macOS.)

With Apple Mail on Mac the unread count is inconsistent when it comes to duplicates. It will say you have 29 unread mails but you will only see 27 unread when you filter or count them manually. Turns out that if you read one of the duplicates (on another client, so one duplicate is read, one is unread) Apple Mail on Mac will mark it as read but count it as unread.

![29 unread](/images/apple_mail/29_unread.png)
![27 unread](/images/apple_mail/27_unread.png)

With Apple Mail on the web, it will count every duplicate as individual mails so the count is "correct"(?), but you have to expand the mails to see the duplicate if you do count them manually. (Expanding duplicates 
is not possible on Apple Mail on macOS, everything regarding duplicates is hidden.)

![Inconsistent mails](/images/apple_mail/inconsistent.png)
_Two duplicates merged into one mail, one duplicate is read, the other is not. The combined result seem to be "not read" - but Apple Mail on different platforms do not agree on this._

On Apple Mail on iOS it does the same even though you can't see the duplicates which is super confusing. So it will say that you have "45 unread" (total duplicate count) but when you count them in the list you will only find 30 mails because the duplicates are hidden. At one point the Apple Mail on iOS said I had 60 unread (2x the number of de-duplicated mails) and the morning after it was back to 45 (the number of duplicated unread mails).

I will not try to make more sense of it. Just know that mail count and unread count is highly inconsistent between Apple Mail on Mac, iOS and the web.

## Conclusion
Do not, under any circumstances, use the "Export Mailbox" functionality in Apple Mail on a Mac. It is 100% broken.

If you're missing e-mails on iCloud, Apple support is actually good and can help rebuild the inbox.

`mbox`-files has a limit of ~2.15 gb due to 32-bit integers. (Not sure why they are used anymore as every company on earth wants to subscribe me to their mailing list and flood my inbox with GBs of newsletters.)

CLI-tools are great, a common pattern seems to be that they work much better compared to trillion dollar companies' software. I haven't explored the different imap backup tools much but [offlineimap](https://www.offlineimap.org) seems to work fine and I'll add it to my backup server with some rsync for redundancy. Thunderbird seems fine as well if you do a manual copy and not an export.

Take backups regularly. Verify that they actually work.

### Update: 2025-10-10
I don't know how the duplicates are created but it seems to be an increasing problem. Since the original writing of this post (less than a month ago), I've received about 100 new e-mails but I've also gotten 5,500 duplicates of old existing emails. No loss of e-mails though so I guess that's something...