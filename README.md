# Outbound API
Outbound sends automated email, SMS, phone calls and push notifications based on the actions users take or do not take in your app. The Outbound API has two components: 

1. Identify each of your users and their attributes using an identify API call.
2. Track the actions that each user takes in your app using a track API call.

Because every message is associated with a user (identify call) and a specific trigger action that a user took or should take (track call), Outbound is able to keep track of how each message affects user actions in your app. These calls also allow you to target campaigns and customize each message based on user data.

Example: When a user in *san francisco*(user attribute) does *signup*(event) but does not *upload a picture*(event) within *2 weeks*, send them an email about how they'll benefit from uploading a picture.

## Authentication
For all server-to-server communication, *Outbound API* uses HTTP headers for authorization. For each API request made, the `X-Outbound-Key` header should be set and contain your environment's private API key.

## Content Type
*Outbound API* uses `application/json` as the content type in all responses and expects it for the content type of the request as well.

## Error States
One of two possible error states could be returned by *Outbound API*. The first is a HTTP 500 error indicating that the request contained improperly formatted JSON. The second possible error is a HTTP 400 error indicating you have missing or invalid required field. When either of these errors occur, no processing is done on the request and the request is terminated.

## Request Parameters
### Required
Both the */identify* and */track* API calls require a `user_id`. It can be either a string or a number. This should be the same value you use to uniquely identify users in your system. We've found that customers like to use an email address or a user id from their database as the `user_id`.

The */track* API call also requires an `event` parameter. It should be a string and be the name of the event in a format you recognize. It does not need to be proper English or even words. Numbers (as strings), words, or random characters are all acceptable. In the example above, the *"signup"*, or *"upload a picture"* make good values for `event`.

Every other parameter of each call is completely optional.

# Users
You identify a user with Outbound each time you create a new user or update an existing user in your system. It is recommended that you send as much information about the user as possible. Any attribute you send can be used in the messages from Outbound. So, if you send a `first_name`, your email can say, `Hi {{first_name}}!`

The `attributes` parameter of the */identify* call is an optional hash/map/dictionary/object of free-form properties you want to track for the user. It may contain nested fields and fields can be of any type.

## Identify a User
### Request

        POST https://api.outbound.io/v2/identify
        Content-type: application/json
        X-Outbound-Key: YOUR_API_KEY
        {
            user_id: "The unique identifier used to identify this user", //Required
            first_name: "The user's first name",  // Optional
            last_name: "The user's last name",  // Optional
            email: "The user's email address", // Optional - required to send emails
            apns: ["An array of the user's iOS device tokens"],  // Optional - required to send iOS notifications
            gcm: ["An array of the user's Android device tokens"],  // Optional - required to send android notifications
            phone_number: "The user's phone number",  // Optional - required to send sms or make phone calls
            attributes: { } // Other optional information about the user you want to use in messages.
        }

### Response
200/OK

# Events
You can track unlimited events using the *Outbound API*. Any event you send can be used a trigger event for a message or the goal event of a desired user flow which triggers an event when not completed within a set period of time. 

Example: Following the example from the previous sections, the *"signup"* event acts like a campaign trigger, and if the user does not do the desired goal event *"upload a picture"* in 2 weeks, we send them a message encouraging them to share a smile.

The `properties` parameter of the */track* call is an optional hash/map/dictionary/object of free-form properties you want to track for the event. It may contain nested fields and fields can be of any type.

Example: `properties` is metadata like the `timestamp` of the *"signup"*" event, the `resolution` of the picture uploaded in the *"upload a picture"* event and so forth. 

## Track an Event
### Request

        POST https://api.outbound.io/v2/track
        Content-type: application/json
        X-Outbound-Key: YOUR_API_KEY
        {
            user_id: "The unique identifier used to identify this user", // Required
            event: "The name of the event that is being triggered", // Required
            properties: { } // Optional - Properties about the event.
        }

### Response
200/OK

Please feel free to contact [support@outbound.io](mailto:support@outbound.io?subject=Reg API documentation) for any further information.
