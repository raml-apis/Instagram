---
site: https://anypoint.mulesoft.com/apiplatform/popular/admin/#/dashboard/apis/7954/versions/8117/portal/pages/6739/preview
apiNotebookVersion: 1.1.66
title: Instagram RAML API Notebook
---

```javascript

load('https://github.com/chaijs/chai/releases/download/1.9.0/chai.js')

```



See http://chaijs.com/guide/styles/ for assertion styles



```javascript

assert = chai.assert;

```

```javascript

CLIENT_ID = prompt("Please, enter client ID of your Instagram application")

CLIENT_SECRET = prompt("Please, enter client Secret of your Instagram application")

```

```javascript

API.createClient('client', '/apiplatform/repository/public/organizations/30/apis/7954/versions/8117/definition');

```

```javascript

API.authenticate(client, "oauth_2_0",{

  clientId: CLIENT_ID, clientSecret: CLIENT_SECRET, scope: [ "basic", "comments", "relationships", "likes" ] })

```



Lets get get current user.



```javascript

currentUserResponse = client("/users/self").get()

```

```javascript

currentUserId = currentUserResponse.body.data.id

```



Searching for 'Johnson'.



```javascript

usersSearchResponse = client.users.search.get({q:"Johnson"})

```

```javascript

assert.equal(usersSearchResponse.status,200,"Error")

someUserId = usersSearchResponse.body.data[0].id

```



What current user likes?



```javascript

currentUserLiked = client.users.self.media.liked.get()

```



Lets get feed object now.



```javascript

feedResponse = client.users.self.feed.get()

```



Lets get users requests.



```javascript

requestedResponse = client("/users/self/requested-by").get()

```



Let's get recent media



```javascript

recentMediaResponse = client.users.userId(currentUserId).media.recent.get()

```

```javascript

mediaId = recentMediaResponse.body.data[0].id

```



Get information about a media object. The returned type key will allow you

to differentiate between image and video media.

Note: if you authenticate with an OAuth Token, you will receive the

user_has_liked key which quickly tells you whether the current user has liked

this media item.



```javascript

mediaResponse = client.media.mediaId(mediaId).get()

```

```javascript

assert.equal( mediaResponse.status, 200, "Error" )

```



Get a popular media



```javascript

mediaResponsePopular = client.media.popular.get()

```



Get a full list of comments on a media



```javascript

commentsResponse = client.media.mediaId(mediaId).comments.get()

```

```javascript

assert.equal( commentsResponse.status, 200, "Error" )

```



Create a comment on a media. Please email apidevelopers[at]instagram.com for access



```javascript

//Commented because of access rights

//commentsResponse = client.media.mediaId(mediaId).comments.post({ text: "This+is+my+comment" })

```

```javascript

//Recheck and uncomment after https://github.com/mulesoft/api-notebook/issues/327 will be finished

//commentsPostResponse = client( "/media/{mediaId}/comments", { mediaId: mediaId } ).post({ text: "This+is+my+comment" })

```

```javascript

//commentsId = commentsPostResponse.body.data[0].id

```



Remove a comment either on the authenticated user's media or authored by the authenticated user.



```javascript

//Recheck and uncomment after https://github.com/mulesoft/api-notebook/issues/327 will be finished

//commentsPostResponse = client( "/media/{mediaId}/comments/commentsId", { mediaId: mediaId, commentsId: commentsId } ).delete()

```

```javascript

mediaId = mediaResponsePopular.body.data[0].id

```



Get a list of users who have liked this media.

Required scope: likes.



```javascript

likesResponse = client.media.mediaId(mediaId).likes.get()

```

```javascript

assert.equal( likesResponse.status, 200, "Error" )

```



Set a like on this media by the currently authenticated user



```javascript

likesPostResponse = client( "/media/{mediaId}/likes", { mediaId: mediaId } ).post()

```

```javascript

assert.equal( likesResponse.status, 200, "Error" )

```



Remove a like on this media by the currently authenticated user



```javascript

likesDeleteResponse = client( "/media/{mediaId}/likes", { mediaId: mediaId } ).delete()

```

```javascript

assert.equal( likesResponse.status, 200, "Error" )

```



Get information about a relationship to another user.



```javascript

relationshipResponse = client("/users/{userId}/relationship",{ userId: someUserId}).get()

```

```javascript

assert.equal( relationshipResponse.status, 200, "Error" )

```



Modify the relationship between the current user and the target user.



```javascript

relationshipResponse = client.users.userId(someUserId).relationship.post({action: "follow"})

```

```javascript

assert.equal( relationshipResponse.status, 200, "Error" )

```



Search for a user by name.



```javascript

searchResponse = client.users.search.get({ q: "Tim" })

```

```javascript

assert.equal( searchResponse.status, 200, "Error" )

```



Get the list of users this user follows.



```javascript

followsUsers = client("/users/{userId}/follows",{ userId: currentUserId}).get()

```

```javascript

assert.equal( followsUsers.status, 200, "Error" )

```



Get the list of users this user is followed by.



```javascript

followedByUsers = client("/users/{userId}/followed-by",{ userId: currentUserId}).get()

```

```javascript

assert.equal( followedByUsers.status, 200, "Error" )

```



Get information about a tag object.



```javascript

aboutTag = client.tags.tagName("surreal").get()

```

```javascript

assert.equal( aboutTag.status, 200, "Error" )

```



Search for tags by name. Results are ordered first as an exact match, then by popularity.

Short tags will be treated as exact matches.



```javascript

searchTags = client.tags.search.get({ q: "surreal" })

```

```javascript

assert.equal( searchTags.status, 200, "Error" )

```



Search for a location by geographic coordinate.



```javascript

searchLocations = client.locations.search.get({ lat: 41.89021, lng: 12.492231 })

```

```javascript

assert.equal( searchLocations.status, 200, "Error" )

```



Get information about a location.



```javascript

locId = searchLocations.body.data[6].id

```

```javascript

aboutLocation = client.locations.locId(locId).get()

```

```javascript

assert.equal( aboutLocation.status, 200, "Error" )

```



Get a list of recent media objects from a given location. May return a

mix of both image and video types.



```javascript

getMediaFromlocation = client.locations.locId(locId).media.recent.get()

```

```javascript

assert.equal( getMediaFromlocation.status, 200, "Error" )

```



Given a short link, issues a redirect to that media's JPG file.



```javascript

//Recheck and uncomment after https://github.com/mulesoft/api-notebook/issues/327 will be finished

//getMediaByShortcode = client.p.shortcode( "/p/{shortcode}/media" ,{ shortcode: "TWA8HeIdTG" }).get()

```

```javascript

//assert.equal( getMediaByShortcode.status, 200, "Error" )

```



Given a short link, returns information about the media associated with

that link.



```javascript

urlString = feedResponse.body.data[0].link

```



Accessing to Oembed



```javascript

oembedResponse = client.oembed.get({ url: "http://instagr.am/p/BUG/" })

```

```javascript

assert.equal( oembedResponse.status, 200, "Error" )

```

```javascript

subscriptionsResponse = client.subscriptions.get({

  client_id: CLIENT_ID,

  client_secret: CLIENT_SECRET

})

```

```javascript

assert.equal( subscriptionsResponse.status, 200, "Error" )

```



Lets subscribe to location



```javascript

subscriptionCreateResponse = client.subscriptions.post({

  client_id: CLIENT_ID,

  client_secret: CLIENT_SECRET,

  object: "geography",  

  aspect: "media",

  lat: 35.657872,

  lng: 139.70232,

  radius: 1000,

  callback_url: "http://liquid-anchor-544.appspot.com/instagram"

})

```

```javascript

assert.equal( subscriptionCreateResponse.status, 200, "Error" )

subscriptionId = subscriptionCreateResponse.body.data.id

geographyId = subscriptionCreateResponse.body.data.object_id

```



Get recent media from a geography subscription that you created.

Note: You can only access Geographies that were explicitly created by your

OAuth client. Check the Geography Subscriptions section of the real-time

updates page. When you create a subscription to some geography that you

define, you will be returned a unique geo-id that can be used in this

query. To backfill photos from the location covered by this geography,

use the media search endpoint.



```javascript

geoMmediaResponse = client.geographies.geoId(geographyId).media.recent.get()

```

```javascript

assert.equal( geoMmediaResponse.status, 200, "Error" )

```



Now let's remove our subscription



```javascript

subscriptionDeleteResponse = client.subscriptions.delete({},{

  query:{

    client_id: CLIENT_ID,

    client_secret: CLIENT_SECRET,

    id: subscriptionId

  }

})

```

```javascript

assert.equal( subscriptionDeleteResponse.status, 200, "Error" )

```