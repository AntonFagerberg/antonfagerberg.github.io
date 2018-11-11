---
  layout: post
  title: "Lesson from debugging Oozie's errors"
  categories: blog
---

Here's a story of the "joys" of debugging, hopefully it can help some of you out there since I did search a lot a solution but only found people with similar problems and no answers.

I was developing my own Oozie client since I needed a non-blocking async client (which is not available in the standard Java Oozie client). The [API](https://oozie.apache.org/docs/4.0.1/WebServicesAPI.html#Job_Submission) looks straight forward to implement but much to my despair I couldn't get it to work.

At first I got a HTTP Status 401 with error message:

```
This request requires HTTP authentication.
```

This was strange since our Oozie service did not have any authentication turned on. Digging around in the Oozie Client source code I found a `doAs` query string, I didn't see it actually being used but I gave it a shot guessing that you may have to impersonate a Hadoop user — and it "worked". I put worked in quotes since it did remove the authentication problem but I got another error instead.

```
HTTP Status 400: The request sent by the client was syntactically incorrect.
```

I checked everything, the request headers, the XML format and to my knowledge I did everything exactly the same way as the official Oozie client did it. I searched for a solution but nothing worked. Eventually, and out of desperation, I though that I would give it a try via [cURL](https://curl.haxx.se) just to do something different and by sheer luck I used it with `-i` (without thinking) which very fortunately prints the response headers.

Turns out the error message in the body is not correct at all and the actual error is only visible in the response headers.

With the `doAs` query string, the response headers stated:

```
oozie-error-code: E1603
oozie-error-message: proxyUser cannot be null, If you're attempting to use user-impersonation via a proxy user, please make sure that oozie.service.ProxyUserService.proxyuser.#USER#.hosts and oozie.service.ProxyUserService.proxyuser.#USER#.groups are configured correctly
```

And after removing the `doAs` query string the real error revealed itself:

```
oozie-error-code: E0504
oozie-error-message: E0504: App directory [hdfs://[...].xml] does not exist
```

**Lesson: when debugging, verbose is your friend!**

I went to file this as a bug, turns out this was [reported](https://issues.apache.org/jira/browse/OOZIE-1782) many years ago but nothing is merged.

At least now we know not to trust the body and instead look in the response headers for clues when it comes to Oozie.