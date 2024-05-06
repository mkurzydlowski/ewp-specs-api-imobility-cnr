Incoming Mobility CNR API
=========================

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This document describes the **Incoming Mobility CNR API**. This API is
implemented by the sending institution if it wants to be notified whenever
*incoming* mobilities kept on their partner institutions' servers are changed.

CNR stands for Change Notification Receiver. For a detailed introduction on how
CNR APIs work, please read [this page][cnr-intro].


Request method
--------------

 * Requests MUST be made with HTTP POST method. Servers MAY reject all other
   request methods.


Request parameters
------------------

Parameters MUST be provided in the `application/x-www-form-urlencoded` format.


### `omobility_id` (repeatable, required)

A list of identifiers of Outgoing Mobility objects (no more than
`<max-omobility-ids>` items). These identify the Mobility objects which have
been recently updated on the caller's side.

This parameter is *repeatable*, so the request MAY contain multiple occurrences
of it. The server SHOULD process all of them. Server implementers provide their
own chosen value of `<max-omobility-ids>` via their manifest entry (see
[manifest-entry.xsd](manifest-entry.xsd)). Clients SHOULD parse this value (or
assume it's equal to `1`).


Security
--------

This version of this API uses [standard EWP Authentication and Security,
Version 2][sec-v2]. Server implementers choose which security methods they
support by declaring them in their Manifest API entry.

This API does not expose any sensitive data, it only notifies the server that
it should reload portions of its data. For this reason, it is RECOMMENDED for
server implementers to not be overly strict on security methods they require
(i.e. it is RECOMMENDED to *not* require extra layers of encryption in requests
and responses - TLS seems more than enough).


Handling of invalid parameters
------------------------------

 * General [error handling rules][error-handling] apply.

 * Servers MUST return a valid (HTTP 200) XML response whenever the request has
   been [properly received][bad-cnr-request]. Unless HTTP 200 is received,
   clients are RECOMMENDED to automatically retry the request after some time.


Response
--------

Servers MUST respond with a valid XML document described by the
[response.xsd](response.xsd) schema. See the schema annotations for further
information.


Safety measures
---------------

It is NOT guaranteed that all notifications will be delivered to you promptly.
Some notifications may also **not reach you at all**, e.g. due to
implementation errors on the calling institution's server.

Therefore, you - the sending HEI, the implementer of this CNR API - SHOULD
periodically verify if your copies are up-to-date, e.g. by periodically
fetching data for all your "live" `omobility_ids` directly from the `get`
endpoint of [Incoming Mobilities API][imobilities-api].


[develhub]: https://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management#statuses
[registry-spec]: https://github.com/erasmus-without-paper/ewp-specs-api-registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery
[echo]: https://github.com/erasmus-without-paper/ewp-specs-api-echo
[error-handling]: https://github.com/erasmus-without-paper/ewp-specs-architecture#error-handling
[institutions-api]: https://github.com/erasmus-without-paper/ewp-specs-api-institutions
[iias-api]: https://github.com/erasmus-without-paper/ewp-specs-api-iias
[imobilities-api]: https://github.com/erasmus-without-paper/ewp-specs-api-imobilities
[cnr-intro]: https://github.com/erasmus-without-paper/ewp-specs-architecture#cnr
[bad-cnr-request]: https://github.com/erasmus-without-paper/ewp-specs-architecture#bad-cnr-request
[sec-v2]: https://github.com/erasmus-without-paper/ewp-specs-sec-intro/tree/stable-v2
