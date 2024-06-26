---
  layout: post
  title: "Instagram API Scala"
  categories: projects
  tags: code
---

This is an Scala implementation of the Instagram API.

This project uses scalaj-http to send HTTP-requests and lift-json to parse the JSON response to case classes.

You need to register an application for Instagram and get a client id (and later an access token for full access) before you can use this API. First register on: [instagram.com/developer](http://instagram.com/developer) to get the required client id and secret - then use this API to receive an access token.

A Response object is returned from all methods and it contains:

 * Data - the stuff you actually want. This is the JSON response parsed to a corresponding object.
 * Pagination - if a large result set is retrieved, this object contains what you need to fetch the next "batch" of information. You can pass the relevant pieces of this information to the same method as called before as optional parameters.
 * Meta - contains the status from Instagram (like 200 OK, 400 error). Internal errors such as parsing or socket errors will be stored in this meta object as well.

## Installation & Demo
You can use SBT to download the required dependencies and run the Demo-file:

```text
sbt run
```

The demo-file contains all implemented methods but you need to uncomment some of them to try them (to avoid spamming the Instagram servers). You need to get a client id (and optionally an access token) before running the examples.

## Adding to your project
Simply run:

```text
sbt package
```

To create a Jar-file. However, you need to add the dependencies below to your project manually!

### Dependencies

```scala
"net.liftweb" %% "lift-json" % "2.5"
"org.scalaj" %% "scalaj-http" % "0.3.15"
```

## Implementation status

### Implemented
 * Images and Videos.
 * Get basic information about a user.
 * Search for a user by name.
 * Get information about a media object.
 * See the authenticated user's feed.
 * Get the most recent media published by a user.
 * See the authenticated user's list of liked media.
 * Get the list of users this user follows.
 * Get the list of users this user is followed by.
 * Get a list of currently popular media.
 * Get a full list of comments on a media.
 * Get a list of users who have liked this media.
 * Get information about a tag object.
 * Get a list of recently tagged media.
 * Search for tags by name.
 * Get information about a location.
 * Search for a location by geographic coordinate.
 * Get a list of media objects from a given location.
 * Token support.
 * Client ID support.
 * Pagination.
 * OAuth 2.0 authentication.
 * Get information about a relationship to another user.
 * Modify the relationship with target user.
 * List the users who have requested to follow.
 * Set a like on this media by the current user.
 * Remove a like on this media by the current user.
 * Add a comment (requires special permission from Instagram).
 * Remove a comment.
 * Search for media in a given area. The default time span is set to 5 days. The time span must not exceed 7 days. Defaults time stamps cover the last 5 days.
 * Create a comment on a media. Please email apidevelopers[at]instagram.com for access.

### Not implemented
 * Subscriptions.

## Example usage

```scala
// You need to request these variables from Instagram: www.instagram.com/developer
val clientId = "client-id"
val clientSecret = "client-secret"
val redirectURI = "redirect-URI"

// Client-Side (Implicit) Authentication
// Get a URL to call. This URL will return the TOKEN in the URI after the #-symbol (and you're done)..
println(Authenticator.tokenURL(clientId, redirectURI))
// You can append scopes by doing println(Authenticator.tokenURL(clientId, redirectURI, comments = true, relationships = true, likes = true))

// Server-side (Explicit) Flow
// Step 1: Get a URL to call. This URL will return the CODE to use in step 2 in the URI as a parameter code.
println(Authenticator.codeURL(clientId, redirectURI))
// You can append scopes by doing println(Authenticator.codeURL(clientId, redirectURI, comments = true, relationships = true, likes = true))
// Step 2: Request a token for the code requested in step 1 (the code is valid one time only).
// This will return Either a Authentication object with access token and user information or a Meta object on failure.
println(Authenticator.requestToken(clientId, clientSecret, redirectURI, code = "the-code-from-step-1"))

// Use either Left("The access-token you requested") or Right("your client ID").
// Note that not all features are available without an access token.
val instagram = new Instagram(accessTokenOrClientId = Left("Put-access-token-here"))

// println("Here is some stuff you can do (when you have a valid Access Token or Client Id):")

// println(s"Get basic information about a name: ${instagram.userInfo("12895238")}")

// println(s"Search for a name by name: ${instagram.userSearch("AntonFagerberg")}")

// println(s"Get the list of users this user follows: ${instagram.follows("12895238")}")

// println(s"Get the list of users this user is followed by: ${instagram.followedBy("12895238")}")

// println(s"See the authenticated user's feed: ${instagram.feed()}") // Token

// println(s"Get the most recent media published by a user: ${instagram.mediaRecent("12895238")}")

// println(s"See the authenticated user's list of liked media: ${instagram.liked()}") // Token

// println(s"Get information about a media object: ${instagram.media("452194471682227494_12895238")}")

// println(s"Get a list of currently popular media: ${instagram.popular}")

// println(s"Get a full list of comments on a media: ${instagram.comments("452194471682227494_12895238")}")

// println(s"Get a list of users who have liked this media: ${instagram.likes("452194471682227494_12895238")}")

// println(s"Get information about a tag object: ${instagram.tagInformation("hipster")}")

// println(s"Get information about a tag object: ${instagram.tagSearch("snowy")}")

// println(s"Get a list of recently tagged media: ${instagram.tagRecent("beer")}")

// println(s"Get information about a location: ${instagram.location("1")}")

// println(s"Get a list of media objects from a given location: ${instagram.locationMedia("1")}")

// println(s"Search for a location by geographic coordinate: ${instagram.locationSearch(Some("48.858844" -> "2.294351"))}")

// println(s"Like an image: ${instagram.like("457326401520123505_12895238")}")

// println(s"Unlike an image: ${instagram.unlike("457326401520123505_12895238")}")

// println(s"Relationship to a user: ${instagram.relationship("12895238")}")

// println(s"Follow a user: ${instagram.relationshipFollow("12895238")}")

// println(s"Unfollow a user: ${instagram.relationshipUnfollow("12895238")}")

// println(s"Block a user: ${instagram.relationshipBlock("12895238")}")

// println(s"Unblock a user: ${instagram.relationshipUnblock("12895238")}")

// println(s"Approve a user: ${instagram.relationshipApprove("12895238")}")

// println(s"Ignore a user: ${instagram.relationshipIgnore("12895238")}")

// println(s"Relationship requests: ${instagram.relationshipRequests}")

// Whitelisting of client is needed for comment access.
// Please visit http://bit.ly/instacomments for commenting access
// println(instagram.comment("457326401520123505_12895238", "test! :D"))
// println(instagram.commentDelete("media-id", "comment-id"))

// println(s"Search for images near a coordinate: ${instagram.mediaSearch("48.858844" -> "2.294351")}")
```

## Download
[github.com/AntonFagerberg/InstagramScalaAPI](https://github.com/AntonFagerberg/InstagramScalaAPI)

## License
Copyright 2013 Anton Fagerberg (http://www.antonfagerberg.com).

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

For more information and examples, visit the project on GitHub.
