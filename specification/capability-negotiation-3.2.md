# IRCv3 Client Capability Negotiation

Copyright (c) 2004-2012 Kevin L. Mitchell <klmitch@mit.edu>.

Copyright (c) 2004-2012 Perry Lorier <isomer@undernet.org>.

Copyright (c) 2004-2012 Lee Hardy <lee@leeh.co.uk>.

Copyright (c) 2009-2012 William Pitcock <nenolod@atheme.org>.

Copyright (c) 2014 Attila Molnar <attilamolnar@hush.com>.

Unlimited redistribution and modification of this document is allowed
provided that the above copyright notice and this permission notice
remains in tact.

## New in version 3.2

### Version in `CAP LS`

The LS subcommand now has an additional argument which is the raw version
number of the latest capability negotiation protocol supported by the client.

This document describes version `3.2`.

Servers MUST NOT send messages described by this document if the client
only supports version 3.1.

The client SHOULD send the latest raw version of the capability negotiation
protocol it supports as the next argument of the command after LS.
If no other arguments are present, version 3.1 is assumed.

### Capabilities with values

Servers MAY specify additional data for each capability using the
`<name>[=<value>]` format.

The meaning of the value (if present) depends on the capability in question.

### Multi-line replies to `CAP LS`

Servers MAY reply with multiple lines in response to `CAP LS`.
If the reply consists of multiple lines (due to IRC line length limitations)
all but the last subcommand MUST have a parameter containing only an asterisk
(`*`) preceding the capability list.

### The `version` dummy capability

If the server supports version 3.2 or a later version of the capability
negotiation protocol, it MUST include a dummy capability named `version`.
The dummy capability MUST have a value, which MUST be the version of
the capability negotiation protocol supported by the server.

Clients MUST be prepared to handle the case when there is no `version`
capability in the reply, even though they are requesting version 3.2 or a
later version of the capability negotiation protocol. This is because
3.1-only servers never send a `version` capability.

## Examples

Example where the client uses CAP 3.2 and the reply contains a cap with
a value:

    Client: CAP LS 3.2
    Server: CAP * LS :multi-prefix sasl=EXTERNAL version=3.2

Example where the client uses CAP 3.2 and gets a multiline reply:

    Client: CAP LS 3.2
    Server: CAP * LS * :multi-prefix extended-join account-notify batch invite-notify tls version=3.2
    Server: CAP * LS * :cap-notify server-time example.org/dummy-cap=dummyvalue example.org/second-dummy-cap
    Server: CAP * LS :userhost-in-names sasl=EXTERNAL,DH-AES,DH-BLOWFISH,ECDSA-NIST256P-CHALLENGE,PLAIN
