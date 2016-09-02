# Bombast

Bombast is a template drive communications app. It can send via email and SMS channels. 
Templates are set up, using [Handlebars](http://handlebarsjs.com/) syntax, and messages can be sent against these templates, with receipient specific data rendered into the template. 

Bombast provides features such as resend, receipient (entity) and message reference tracking. 

## API

### Authentication

Bombast uses [Basic Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication) on every request, combined with SSL. 

So, for example : 

```
Username - test
password - test12345 
header - Authorization: Basic dGVzdDp0ZXN0MTIzNDU= 
```

This authentication header must be provided on every request to the api. If you do not you will get a 401 Unauthorised response with "Un" in the body of the response.

### Sending using a template

Using a predefined template, which can be setup on the front end or via api, messages can be sent via a POST to the path : 

`$BASE_URL/{bombastName}/messageTemplates/{templateName}/messages`

The BODY of the post should contain the `data` to template in, as well as the receipient info in the `message` object. 

```
{
  "entity_id" : "YOUR_CUSTOMER_NUMBER_123", 
  "reference" : "ORDER_2343748034", 
  "message": {
    "email": {
      "toAddress": "bradley@encentivize.co.za"
    },
    "sms": {
      "toNumber": "0748136247"
    }
  },
  "data": {
    "name": "Bradley",
    "surname": "Van Aardt",
    "voucherValue": "200",
    "voucherCode": "23042390483"
  },
  "priority": "Normal"
}

```

The fields in the `data` object is determined by the fields in the template - ie. there is no standard field set, it is dynamically created. 

`entity_id` is optional, but can be used to store a unique identifier for the receipient. This is helpful to find all messages sent to a customer, for example on dashboards or client service. 

`reference` is used to tie the message back to an event or similar on the originating system. 

If the request is successfull, an http `202` response will be returned, signifying that the message is queued for sending. 

A document will also be sent back, containing the merged message, and the unique `_id` to use for reference. 

The field `mergedMessage` is the final email and sms data that is sent to the receipient. 

## Searching messages

To find messages, you can perform a GET operation with a number of query string parameters. The most likely are `reference` or `entity_id`. 

For example

`$BASE_URL/{bombastName}/messages?entity_id=YOUR_CUSTOMER_NUMBER_123&limit=100&skip=0`

`skip` and `limit` can be used for paging - by default the first 50 results are returned. 

Sorting and ordering is also supported. for example, to return the last updated messages for a customer ordered by date sent descending, we can use : 

`$BASE_URL/{bombastName}/messages?entity_id=YOUR_CUSTOMER_NUMBER_123&sort=field-currentStatusDate`

### Resending messages

To resend a message, you can simply POST to  the `/resend` route of a message. 

For example, 

`/{bombastName}/messages/{message_id}/resend`

This will resend with the same data as the original send.










