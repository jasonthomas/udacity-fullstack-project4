Udacity Full Stack Project 4 - Conference Application
## Products
- [App Engine][1]

## Language
- [Python][2]

## APIs
- [Google Cloud Endpoints][3]

## Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][4].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. (Optional) Mark the configuration files as unchanged as follows:
   `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
1. (Optional) Generate your client library(ies) with [the endpoints tool][6].
1. Deploy your application.


## Task 1
Session and SessionFrom was added to models.py. Endpoints related to Session returned a SessionForm object.

The only required fields for Session is name and wsck (Websafe key). Although without the other values you might not able perform complex queries. They Websafe key can be found in the Conference object either by looking at the Conference page url or looking at the Conference object in the Datastore. A Session is a ancestor of Conference, such that a relationship is formed between Conference and Session. The speaker is represented by a String instead of it's own Entity.

getConferenceSessionsByType - requires two input values, the wsck and typeOfSession.
getConferenceSessions - requires one input value, the wsck.
getSessionsBySpeaker - requires one input value, the speaker.

## Task 2
Wishlist and WishlistForm was added to models.py. Wishlist object are repeatable and are represented as a list in Datastore.

A wishlist for a Profile is created only when the user adds a session to the Wishlist.

addSessionToWishlist - Creates and adds sessions to Wishlist. Returns current sessions in Wishlist.
deleteSessionInWishlist - Deletes session from Wishlist. Returns current sesions in Wishlist.


## Task 3
Indexes were creating automatically locally and populated in index.yaml. Deploying to app engine populated the indexs in Datestore.

### Query 1
getSessionsByTypeToday() - Returns sessions by type and date greater than Today. This is useful for Users that want to find sessions that haven't completed yet.
```
today = datetime.now()
sessions = Session.query(ndb.AND(Session.typeOfSession == request.typeOfSession,
                                         Session.date > today.date()))
```

### Query 2
getSessionsBySpeakerType() - Returns sessions by speaker and type. You might like a specific speaker and want to attend specific type of sessions.
```
Session.query(ndb.AND(Session.speaker == request.speaker,
                                 Session.typeOfSession == request.typeOfSession))
```
### Query Problem
Implemented in getCustomQueryTask3()

Since Datastore supports only one inequality filter per query you can filter by the first inequality and then do the rest of the filtering in python (implemented in getCustomQueryTask3). You can also run two seperate inequality queries and then get the intersection of both.


## Task 4
getFeaturedSpeaker() is called as a task when a new session is added. A post request is made to the task endpoint, which passes useful request information to cacheFeaturedSpeaker()


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
