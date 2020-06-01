# Integration Case Study 1 - Klarna

## Table of Contents

* [Solutions](#solutions)
* [About](#about)
  * [Tools](#tools)
* [Order Placed](#placed)
* [Order Captured](#captured)
* [Order Cancelled](#cancelled)
* [Order Refunded](#refunded)
* [Notes](#notes)


## Solutions
* [Placed]    -  16bd0116-3c27-27e5-ae81-ecea193f9b0c
* [Captured]  -  01b2ac26-fb76-2cd2-94f8-e8a619e4c004
* [Cancelled] -  85b0e719-f372-232b-a568-2f5ca39e3be1
* [Refunded]  -  81b5a32e-bb9f-2ae0-bb22-b5a207adba60

## About
The objetive of this document is to demostrate the understanding of the provided solutions for the Integration Case Study 1. 

Altough some of the requested order cases could be contained in a same order - ie, the "cancelled order", is a "placed order" too -  there is a different order for each case in order to keep it simple for the reviewer. 

### Tools
As suggested in the case statement email, the following tools have been used.
* [Postman](https://postman.com)
* [Codepen](https://codepen.io)

## Placed
1. Create the server-side session by a POST REQUEST 
```JS
POST /payments/v1/sessions
Authorization: Basic pwhcueUff0MmwLShJiBE9JHA==
Content-Type: application/json

{
  "purchase_country": "US",
  "purchase_currency": "USD",
  "locale": "us-US",
  "order_amount": 4000,
  "order_tax_amount": 0,
  "order_lines": [{
    "type": "physical",
    "reference": "REF1",
    "name": "Battery Power Pack",
    "quantity": 1,
    "unit_price": 4000,
    "tax_rate": 0,
    "total_amount": 4000,
    "total_discount_amount": 0,
    "total_tax_amount": 0
  }]
}
```
2. Display the widget and authorize the pursache using the client-JS-SDK, using the client_token from result in 1. 
```JS
window.onload = function () {
  Klarna.Payments.init({
    client_token: "eyJhbGciOiJSUzI1NiIsImtpZCI6IjgyMzA1ZWJjLWI4MTEtMzYzNy1hYTRjLTY2ZWNhMTg3NGYzZCJ9.ewogICJzZXNzaW9uX2lkIiA6ICIxZmIyNzdjZi01MjUzLTIwNDItOTBiMi03MjU2MDE2Yjc3ZTQiLAogICJiYXNlX3VybCIgOiAiaHR0cHM6Ly9rbGFybmEtcGF5bWVudHMtbmEucGxheWdyb3VuZC5rbGFybmEuY29tL3BmbXZwIiwKICAiZGVzaWduIiA6ICJrbGFybmEiLAogICJsYW5ndWFnZSIgOiAidXMiLAogICJwdXJjaGFzZV9jb3VudHJ5IiA6ICJVUyIsCiAgInRyYWNlX2Zsb3ciIDogZmFsc2UsCiAgImVudmlyb25tZW50IiA6ICJwbGF5Z3JvdW5kIiwKICAibWVyY2hhbnRfbmFtZSIgOiAiUGxheWdyb3VuZCBEZW1vIE1lcmNoYW50IiwKICAic2Vzc2lvbl90eXBlIiA6ICJQQVlNRU5UUyIsCiAgImNsaWVudF9ldmVudF9iYXNlX3VybCIgOiAiaHR0cHM6Ly9uYS5wbGF5Z3JvdW5kLmtsYXJuYWV2dC5jb20iLAogICJzY2hlbWUiIDogdHJ1ZQp9.MdIgDcjUSG8n9EbrhnoWTzI0yMkOCzBdfRgQnnOJetOKdluWSmYO96zc7nssxY2sRkZFBn0NJBnSt0NhjTZ51BsVnVtClL5cKxDs2GVQzlq9AG4GWrXmO9byjUDMgqw9lsdw7-W47FJ-VEyo2b06CjwGg13iTMxFPN6PnOcLsqlh7ql543ap_mzm6XY6pVkUmVylx8v5Cu7xWeq0nVUGBcT455kycZupwPJaf2cn-lqLAQfvDVVE2aXRyxHdgv4OUHN7DJFSZ_tzGDkgpb4HgqVDmIos1pizAiWcBRzGjkAD-eeTI-91hMCBy0K7oKuI_eiedJjIW4UyBrZVr0XZKQ"
  });
  Klarna.Payments.load(
    {
      container: "#klarna-container",
      payment_method_category: "pay_over_time"
    },
    {
  "purchase_country": "US",
  "purchase_currency": "USD",
  "locale": "us-US",
  //All Amounts are ISO 4217 - Minor units compliant as docummented 
  "order_amount": 40000,
  "order_tax_amount": 0,
  "order_lines": [{
    "type": "physical",
    "reference": "19-402-USA",
    "name": "Battery Power Pack",
    "product_identifiers": {
                "brand": "Intel",
                "category_path": "Electronics Store > Computers & Tablets > Desktops",
                "global_trade_item_number": "735858293167",
                "manufacturer_part_number": "BOXNUC5CPYH"
    },
    "quantity": 1,
    "unit_price": 40000,
    "tax_rate": 0,
    "total_amount": 40000,
    "total_discount_amount": 0,
    "total_tax_amount": 0
  }]
},
    function (res) {
      console.debug(res);
    }
  );
};
//get a reference to the element
var placeOrder = document.getElementById("place_order");
placeOrder.addEventListener("click", function (event) {
  Klarna.Payments.authorize({
  payment_method_category: "pay_over_time"
}, {
  purchase_country: "US",
  purchase_currency: "USD",
  locale: "en-US",
  billing_address: {
    given_name: "John",
    family_name: "Doe",
    email: "franjoespejo@gmail.com",
    title: "Mr",
    street_address: "Lombard St 10",
    street_address2: "Apt 214",
    postal_code: "90210",
    city: "Beverly Hills",
    region: "CA",
    phone: "+34696909414",
    country: "US"
  },
  shipping_address: {
    given_name: "John",
    family_name: "Doe",
    email: "john@doe.com",
    title: "Mr",
    street_address: "Lombard St 10",
    street_address2: "Apt 214",
    postal_code: "90210",
    city: "Beverly Hills",
    region: "CA",
    phone: "3334445551",
    country: "US"
  },
  order_amount: 40000,
  order_tax_amount: 0,
  order_lines: [{
    type: "physical",
    reference: "19-402-USA",
    name: "Battery Power Pack",
    quantity: 1,
    unit_price: 40000,
    tax_rate: 0,
    total_amount: 40000,
    total_discount_amount: 0,
    total_tax_amount: 0,
    product_url: "https://www.estore.com/products/f2a8d7e34",
    image_url: "https://www.exampleobjects.com/logo.png"
  }],
  customer: {
    date_of_birth: "1970-01-01",
    gender: "male"
  },
}, function(res) {
  console.debug(res);
});
});
```
3. Going forward with all the client-side interacctions to proceed with the order. All is handled through client-side SDK
* Real-world phone with country code
* Klarma-test-env card as Credit Card, [Documentation](https://developers.klarna.com/documentation/testing-environment/#test-credit-card)
4. Place Order 
```JS
POST /payments/v1/authorizations/<authorization_token>/order
Authorization: Basic pwhcueUff0MmwLShJiBE9JHA==
Content-Type: application/json

{
  "purchase_country": "US",
  "purchase_currency": "USD",
  "locale": "en-US",
  "billing_address": {
    "given_name": "John",
    "family_name": "Doe",
    "email": "franjoespejo@gmail.com",
    "title": "Mr",
    "street_address": "Lombard St 10",
    "street_address2": "Apt 214",
    "postal_code": "90210",
    "city": "Beverly Hills",
    "region": "CA",
    "phone": "+34696909414",
    "country": "US"
  },
    "shipping_address": {
        "given_name": "John",
        "family_name": "Doe",
        "email": "john@doe.com",
        "title": "Mr",
        "street_address": "Lombard St 10",
        "street_address2": "Apt 214",
        "postal_code": "90210",
        "city": "Beverly Hills",
        "region": "CA",
        "phone": "333444555",
        "country": "US"
    },
    "order_amount": 40000,
    "order_tax_amount": 0, 
   "order_lines": [{
    "type": "physical",
    "reference": "REF1",
    "name": "Battery Power Pack",
    "quantity": 1,
    "unit_price": 40000,
    "tax_rate": 0,
    "total_amount": 40000,
    "total_discount_amount": 0,
    "total_tax_amount": 0
  }],
    "merchant_urls": {
        "confirmation": "https://example.com/confirmation",
        "notification": "https://example.com/pending" 
    },
    "merchant_reference1": "45aa52f387871e3a210645d4"
}
```
## Captured
An order to be captured needs to be previously placed.
1. POST REQUEST to create the capture
```JS
POST /ordermanagement/v1/orders/{order_id}/captures
{
    "captured_amount": 40000,
    "description": "string",
    "shipping_info": [
        {
            "shipping_company": "DHL US",
            "shipping_method": "Home",
            "tracking_number": "63456415674545679874",
            "tracking_uri": "http://shipping.example/findmypackage?63456415674545679874",
            "return_shipping_company": "DHL US",
            "return_tracking_number": "93456415674545679888",
            "return_tracking_uri": "http://shipping.example/findmypackage?93456415674545679888"
        }
    ],
    "shipping_delay": 0
}
```
## Cancelled
An order to be cancelled needs to be previously placed.
1. POST REQUEST to cancell an order
```JS
POST /ordermanagement/v1/orders/{order_id}/cancel
```
## Refunded
An order to be refunded needs to be previously placed and captured.
1. POST REQUEST to refund an order
```JS
POST /ordermanagement/v1/orders/{order_id}/refunds
{
  "refunded_amount": 40000,
  "description": "Refunding the total amount",
  "order_lines": [
    {
      "type": "physical",
      "reference": "19-402-USA",
      "name": "Batery Power Pack",
      "quantity": 1,
      "unit_price": 40000,
      "tax_rate": 0,
      "total_amount": 40000,
      "total_tax_amount": 0
    }
  ]
}
```
## Notes
Please note the bellow points:
* All POST requests are authenticated using the given credentials with a Basic Access Authentication 
* All requests are using HTTPS
* All amounts in the requests are ISO 4217 - Minor units compliant as docummented

