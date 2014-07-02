# Outbound API
Put a description about Outbound and/or the API here.

## Authentication
For all server-to-server communication, *Outbound API* uses HTTP headers for authorization. For each API request made, the `X-Outbound-Key` header should be set and contain your environment's private API key.

## Content Type
*Outbound API* uses `application/json` as the content type in all responses and expects it for the content type of the request as well.

## Error States
One of two possible error states could be returned by *Outbound API*. The first is a 500 error indicating that the request contained improperly formatted JSON. The second possible error is a 400 error indicating you have missing or invalid required field. When either of these errors occur, no processing is done on the request and the request is terminated.

## Request Parameters
### Required
The only parameters that are ever required are `user_id` and `event`. Both the */identify* and */track* API calls require a `user_id`. It can be either a string or a number. This should be the same value you use to uniquely identify users in your system. The */track* API call also requires an `event` property. It should be a string and be the name of the event in a format you recognize. It does not need to be proper English or even words. Numbers (as strings), words, or random characters are all acceptable.

Every other parameter of each call is completely optional.

# Users
You identify a user with Outbound each time you create a new user or update an existing user in your system. It is recommended that you send as much information about the user as possible. Any attribute you send could potentially be used to filter users and/or in the rendering of a message to the user.

The `attributes` parameter of the */identify* call is an optional hash/map/dictionary/object of free-form properties you want to track for the user. It may contain nested fields and fields can be of any type.

## Identify a User
### Request

        POST https://api.outbound.io/v2/identify
        Content-type: application/json
        X-Outbound-Key: YOUR_API_KEY
        {
            user_id: "The unique identifier used to identify this user",
            first_name: "The user's first name",
            last_name: "The user's last name",
            email: "The user's email address",
            apns: ["An array of the user's iOS device tokens"],
            gmc: ["An array of the user's Android device tokens"],
            phone_number: "The user's phone number",
            attributes: { }
        }

### Response
200/OK

# Events
You can track any number of events using the *Outbound API*. Any event you send can be used a trigger event for a message or the goal event of a desired user flow which triggers an event when not completed within a set period of time.

The `properties` parameter of the */track* call is an optional hash/map/dictionary/object of free-form properties you want to track for the event. It may contain nested fields and fields can be of any type.

## Track an Event
### Request

        POST https://api.outbound.io/v2/track
        Content-type: application/json
        X-Outbound-Key: YOUR_API_KEY
        {
            user_id: "The unique identifier used to identify this user",
            event: "The name of the event that is being triggered",
            properties: { }
        }

### Response
200/OK
