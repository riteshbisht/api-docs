# Overview

## Introduction

Mifos X is a secure, multi-tenanted microfinance platform.

The goal of the Mifos X API is to empower developers to build apps on top of the Mifos X Platform. The [reference app](https://demo.openmf.org) (username: mifos, password: password) works on the same demo 'tenant' as the interactive links in this documentation.

The API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer).

The API is designed to have:

* predictable, resource-oriented URLs
* to use HTTP response codes to indicate API errors
* to use built-in HTTP features, like HTTP authentication and HTTP verbs, which can be understood by off-the-shelf HTTP clients.

[JSON](http://www.json.org/) is returned in all responses from the API, including errors.

Much of the API presentation and design ideas are owed to the excellent [Apigee "Web API Design" eBook/PDF](http://info.apigee.com/Portals/62317/docs/web%20api.pdf) and the very good [Stripe API reference](https://stripe.com/docs/api).

## Try API From Your Browser

Try The API From Your Browser
GET (read) examples can be run directly from this documentation. It is just a matter of clicking a link. Most browsers display the output on the same page (or another tab if you right click and select that option). Internet Explorer will probably treat the output as if you wanted to download a file. In that case just elect to open the output in a text editor.

If you want to check out the POST, PUT and DELETE (update) examples a good approach is to take a moment to install a REST plugin for your browser e.g.

* [RESTClient](https://addons.mozilla.org/en-US/firefox/addon/restclient/) for Mozilla FireFox
* [Postman](https://chrome.google.com/webstore/detail/postman-rest-client-packa/fhbjgbiflinjbdggehcddcbncdddomop) for Google Chrome

The REST plugins will allow you to

* Select the "Verb" (e.g. POST)
* Enter the resource name (e.g. offices)
* Add a header to indicate you are sending JSON data as part of the request body (Content-Type: application/json)
* Add a header to indicate your 'tenant' (X-Mifos-Platform-TenantId: default)
* Paste the example JSON into a Request Body
* Send the Request (and receive a Response)
