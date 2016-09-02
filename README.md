# Bombast

Bombast is a template drive communication app. It can send email and SMS communication. 
Templates are set up, using (Handlebars)[http://handlebarsjs.com/] syntax, and messages can be sent against these templates, with receipient specific data rendered into the template. 

Bombast provides features such as resend, receipient (entity) and message reference tracking. 


# API

## Authentication

Bombast uses Basic Authentication on every request, combined with SSL. 

## Sending using a template

Using a predefined template, which can be setup on the front end or via api, a POST to the path : 

```/{tenantName}/messageTemplates/{templateName}/messages```

The BODY of the post should contain the data to template in, as well as the receipient info. 

```{
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
}```

The object in the `data` field is determined by the fields in the template - ie. there is no standard field set, it is dynamically created. 

`entity_id` is optional, but can be used to store a unique identifier for the receipient. This is helpful to find all messages sent to a customer, for example on dashboards or client service. 

`reference` is used to tie the message back to an event or similar on the originating system. 






