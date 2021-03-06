[TOC]

# Tango REST API RC4

There are three parts in this proposal: URL specification; Implementation remarks; Implementation recommendations. The first one names valid URLs that must be handled by the implementation. 
Each URL is presented following this format:

 URL  | RESPONSE TYPE  | NOTE
----- | ---------- | ------
`METHOD url`      |  JSONArray/JSONObject/NULL | short comment          

Such table is followed by a JSON response's examples block:

`URL`:
`{'JSON response':'example'}`

In two following sections several implementation guidelines are highlighted.

In general API follows standard CRUD idiom:

HTTP verb | CRUD| collection | instance
----------|-----|------------|----------
GET  | READ | Read a list. 200 OK | Read the details of one instance. 200 OK
POST | CREATE | Create a new instance. 201 OK | -
PUT | UPDATE/CREATE | Full Update. 200 OK | Create an instance. 201 Created
DELETE | DELETE | - | Delete instance. 200 OK

POST create an instance of collection by the URI of this collection.
POST returns the URI and the id of the newly created instance.

# URL example driven specification

All URLs in this section omit protocol//host:port part: `http://host:port`. An implementation may or may not add this to the hrefs. 

For shortness all URLs use `<prefix>` for an API entry point: `/tango/rest/rc3/hosts/tango_host/tango_port`, or omit it completely. So `<prefix>/devices/sys/tg_test/1/attributes` (or `/devices/sys/tg_test/1/attributes`) actually means `/tango/rest/rc3/tango_host/tango_port/devices/sys/tg_test/1/attributes`, where _tango_host_ is a Tango host name, e.g. _hzgxenvtest_; _tango_port_ is a Tango database port number, e.g. _10000_.

_tango_host_ and _tango_port_ are not known in advance, as user may ask for an arbitrary Tango database. The database to which implementation connects at start can be specified via environmental variable, or any other way. 

# Implementation remarks

1. Implement async where possible (almost any PUT, POST and DELETE methods)
2. All constants and magic numbers in responses, i.e. data type, data format, writable, level must be replaced with their string representation
3. Image attributes must be handled on the server side, i.e. server safes image as jpeg (or tiff) and sends URL or embeds this image into response.
4. API must be allowed only for authorized users.
5. Optionally provide integration with TangoAccessControl
6. Optional shortcuts may be implemented to reduce data transfer
7. Provide access to _set_attribute_config()_ via admin panel
8. PUT attribute can be implemented as _write_read_ call.

# Implementation recommendations

1. Implementation must cache Tango proxy objects
2. Implementation must provide Expires response header value related requests (attribute value read)
3. Implementation must export configuration for all caches, i.e. how long keep read value

# Implementation references

1. [mTangoREST.server](https://bitbucket.org/hzgwpn/mtango/wiki/Home#markdown-header-getting-started-with-mtangorestserver)


# References

[1] [Brian Mulloy, Web API Design. Crafting Interfaces that Developers Love](https://pages.apigee.com/rs/apigee/images/api-design-ebook-2012-03.pdf)