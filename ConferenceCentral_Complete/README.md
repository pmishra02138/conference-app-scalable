## Conference Central: A Conference Organization App.

## Overview
Conference central is cloud-based API server to support a provided conference
organization application that exists on the web. The API supports the following
functionality found within the app: user authentication, user profiles,
conference information and various manners in which to query the data.

## Tasks

#### Add Sessions to a Conference
To create session, ```Session``` class and ```SessionForm``` are defined. Session class is used to define a Session Object. Session Object is for creating session entities in datastore. A Session object is created as a chikd of the conference
object. The Session object is as follows:

```python
class Session(ndb.Model):
    """Session -- Session object"""
    name            = ndb.StringProperty(required=True)
    highlights      = ndb.StringProperty()
    speaker         = ndb.StringProperty()
    typeOfSession   = ndb.StringProperty(required=True)
    duration        = ndb.IntegerProperty()
    location        = ndb.StringProperty()
    date            = ndb.DateProperty(required=True)
    startTime       = ndb.TimeProperty(required=True)
```

 Input for an API request is received via SessionForm defined as follows:

 ```python
 class SessionForm(messages.Message):
     """SessionForm --- Session outbound form message"""
     name            = messages.StringField(1)
     highlights      = messages.StringField(2)
     speaker         = messages.StringField(3)
     typeOfSession   = messages.StringField(4)
     duration        = messages.IntegerField(5, variant=messages.Variant.INT32)
     location        = messages.StringField(6)
     conferenceKey   = messages.StringField(7)
     date            = messages.StringField(8)
     startTime       = messages.StringField(9)
 ```
 Four session APIs for creating, retrieving,  querying (by speaker and type) are defined. These for APIs with inputs and outputs paramters are defined as follows:

 ```python
 @endpoints.method(SESS_GET_REQUEST, SessionForms,
         path='conference/{conferenceKey}/sessions',
         http_method='GET', name='getConferenceSessions')

 @endpoints.method(SESS_BY_SPEAKER_REQUEST, SessionForms,
         path='conference/sessions/speakerName/{speaker}',
         http_method='GET', name='getSessionsBySpeaker')

  @endpoints.method(SESS_BY_TYPE_REQUEST, SessionForms,
         path='conference/{conferenceKey}/sessions/type/{typeOfSession}',
         http_method='GET', name='getConferenceSessionsByType')

   @endpoints.method(SessionForm, SessionForm,
           path='conference/{conferenceKey}/session',
           http_method='POST', name='createSession')

 ```
#### Add Sessions to User Wishlist
 Users can mark sessions that they are interested in and retrieve their own current
 wishlist. This functionality is defined as a HAS-A relationship with conference.
 To add user wishlist following endpoints are defined:

```python
@endpoints.method(SESS_WISHLIST_REQUEST, BooleanMessage,
        path='profile/wishlist/{sessionKey}',
        http_method='POST', name='addSessionToWishlist')

@endpoints.method(SESS_WISHLIST_REQUEST, BooleanMessage,
        path='profile/wishlist/{sessionKey}',
        http_method='DELETE', name='deleteSessionInWishlist')

@endpoints.method(message_types.VoidMessage, SessionForms,
        path='profile/wishlist',
        http_method='GET', name='getSessionsInWishlist')

```
#### Indexes and queries

Two query endpoints are implemented. The first query ```getSessionsByDate```, lists all thee sessions that are scheduled for given date. The second query ```getSessionsByStartTime```, lists all session that are starting at the specified time.

** Query sessions that are not worksops and are held before 7 pm? **

This query can solved in two steps. In first step, all sessions object that are
not workshop sesstionTypes need to be queried. In the second step, the list of 'non-workshop' sessions objects need to be filtered for a startime beore 7 pm.
The second query needs to carried on startTime property.



## Language
- [Python][2]


## APIs
- Conference central app can be by logging into accessed at [conf-app-udacity][3]
- API endpoints can be accessed at [Conference API v1][4]

## Setup Instructions
1. Download app engine from [here][4].
1. Download the source code from git [repo][5].
1. Update the value of `application` in `app.yaml` to the app ID you
   have registered in the App Engine admin console and would like to use to host
   your instance of this sample.
1. Update the values at the top of `settings.py` to
   reflect the respective client IDs you have registered in the
   [Developer Console][6].
1. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
1. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's
running by visiting your local server's address (at a port assigned by Google App Engine).
1. (Optional) Generate your client library(ies) with [the endpoints tool][7].
1. Deploy your application.


[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://conf-app-udacity.appspot.com/
[4]: https://apis-explorer.appspot.com/apis-explorer/?base=https://conf-app-udacity.appspot.com/_ah/api
[5]: https://github.com/pmishra02138/conference-app-scalable/tree/master/ConferenceCentral_Complete
[6]: https://console.developers.google.com/
[7]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
