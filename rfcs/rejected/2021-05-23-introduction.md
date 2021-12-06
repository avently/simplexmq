# SMP agent introduction

## Problem

Allow an agent client to connect two connections it has directly, with the agent acting as an out-of-band channel.

It can be used both separately as part of some client functionality, and as part of group protocol.

## Solution

A protocol with commands and message envelopes to exchange the information between parties to establish connection.

### Commands and messages

Below commands are for the scenario when A introduces B to M.

- command `C:idAB INTRO C:idAM infoM` - initiate introduction of the connection cIdB to connection cIdM (response is `C:idAB OK`)
- message `C:idBA REQ C:invId infoM` - notification to confirm introduction
- command `C:idBM? ACPT C:invId` - accept offer to be introduced (response is `C:idBM OK`, followed by `C:idBM CON`)
- message `C:idBM CON` - confirmation that connection is established to both introduced parties
- message `C:idAB CON C:idAM` - confirmation that connection is established to the introducer

### Agent envelopes

- `INTRO C:extIntroIdM infoM` - new introduction offered by introducer
- `INV C:extIntroIdB prv:invBM infoB` - invitation to join connection from B to M sent via A
- `REQ C:extIntroIdB prv:invBM infoB` - new introduction forwarded by the introducer
- `CON C:extIntroId` - confirmation that the connection is established sent by both introduced parties to the introducer

## Namespace

Given that the introduction objects are short lived, they should not reuse the same commands or share the same namespace as connections, broadcasts and groups, but they probably should share the namespace with group and connection invitations.

## Introduction protocol costs

5 messages + cost to establish a connection


The [sequence digram for introduction](https://mermaid.ink/img/eyJjb2RlIjoic2VxdWVuY2VEaWFncmFtXG4gIHBhcnRpY2lwYW50IEEgYXMgQWxpY2UgKEEpIC0gdGhlIGludHJvZHVjZXJcbiAgcGFydGljaXBhbnQgQUEgYXMgQWxpY2Unczxicj5hZ2VudCAoQUEpXG4gIHBhcnRpY2lwYW50IEIgYXMgQm9iIChCKSAtIGludHJvZHVjZWRcbiAgcGFydGljaXBhbnQgQkEgYXMgQm9iJ3M8YnI-YWdlbnQgKEJBKVxuICBwYXJ0aWNpcGFudCBNIGFzIE1hcmsgKE0pIC0gaW50cm9kdWNlZCB0b1xuICBwYXJ0aWNpcGFudCBNQSBhcyBNYXJrJ3M8YnI-YWdlbnQgKE1BKVxuXG4gIG5vdGUgb3ZlciBBLCBBQTogMS4gY3JlYXRlIGludHJvZHVjdGlvblxuICBBIC0-PiBBQTogQzppZEFCIElOVFJPIEM6aWRBTSBpbmZvTTxicj4oaWRBQiAtIGNvbm4gYWxpYXMgQSBoYXMgZm9yIEIsPGJyPmlkQU0gLSBmb3IgTSlcbiAgQUEgLT4-IEE6IEM6aWRBQiBPS1xuXG4gIG5vdGUgb3ZlciBBLCBCQTogMi4gc2VuZCBpbnRybyB0byBCb2JcblxuICBBQSAtPj4gQkE6IHZpYSBpZEFCOiBJTlRSTyBDOmV4dEludHJvSWRNIGluZm9NXG4gIEJBIC0-PiBCOiBDOmlkQkEgUkVRIEM6aW50SW50cm9JZE0gaW5mb01cbiAgQiAtPj4gQkE6IEM6aWRCTT8gQUNQVCBDOmludEludHJvSWRNXG4gIEJBIC0-PiBCOiBDOmlkQk0gT0tcblxuICBub3RlIG92ZXIgQkE6IDMuIGNyZWF0ZSBjb25uZWN0aW9uIGZvciAgTSBpZEJNXG5cbiAgQkEgLT4-IEFBOiB2aWEgaWRCQTogSU5WIEM6ZXh0SW50cm9JZE0gcHJ2OmludkJNIGluZm9CXG5cbiAgbm90ZSBvdmVyIEFBLCBNOiA0LiBzZW5kIGludHJvIHRvIE1hcmtcblxuICBBQSAtPj4gTUE6IHZpYSBpZEFNOiBSRVEgQzpleHRJbnRyb0lkQiBwcnY6aW52Qk0gaW5mb0JcblxuICBub3RlIG92ZXIgTUEsIEI6IDUuIE1hcmsgY29ubmVjdHMgdG8gQm9iXG5cbiAgTUEgLT4-IE06IEM6aWRNQSBSRVEgQzppbnRJbnRyb0lkQiBpbmZvQlxuICBNIC0-PiBNQTogQzppZE1CPyBBQ1BUIEM6aW50SW50cm9JZEJcbiAgTUEgLT4-IE06IEM6aWRNQiBPS1xuXG4gIE1BIC0-PiBCQTogIGVzdGFibGlzaCBjb25uZWN0aW9uIGlkQk0gLT4gaWRNQlxuXG4gIG5vdGUgb3ZlciBBLCBNQTogNi4gbm90aWZ5IGFsbCBjbGllbnRzXG5cbiAgTUEgLT4-IE06IEM6aWRNQiBDT05cbiAgTUEgLT4-IEFBOiB2aWEgaWRNQTogQ09OIEM6ZXh0SW50cm9JZEJcbiAgQkEgLT4-IEI6IEM6aWRCTSBDT05cbiAgQkEgLT4-IEFBOiB2aWEgaWRCQTogQ09OIEM6ZXh0SW50cm9JZEJcbiAgQUEgLT4-IEE6IEM6aWRBQiBDT04gQzppZEFNXG4iLCJtZXJtYWlkIjp7fSwidXBkYXRlRWRpdG9yIjpmYWxzZX0), the source is [here](./intro.mmd).

![sequence digram for introduction](https://mermaid.ink/svg/eyJjb2RlIjoic2VxdWVuY2VEaWFncmFtXG4gIHBhcnRpY2lwYW50IEEgYXMgQWxpY2UgKEEpIC0gdGhlIGludHJvZHVjZXJcbiAgcGFydGljaXBhbnQgQUEgYXMgQWxpY2Unczxicj5hZ2VudCAoQUEpXG4gIHBhcnRpY2lwYW50IEIgYXMgQm9iIChCKSAtIGludHJvZHVjZWRcbiAgcGFydGljaXBhbnQgQkEgYXMgQm9iJ3M8YnI-YWdlbnQgKEJBKVxuICBwYXJ0aWNpcGFudCBNIGFzIE1hcmsgKE0pIC0gaW50cm9kdWNlZCB0b1xuICBwYXJ0aWNpcGFudCBNQSBhcyBNYXJrJ3M8YnI-YWdlbnQgKE1BKVxuXG4gIG5vdGUgb3ZlciBBLCBBQTogMS4gY3JlYXRlIGludHJvZHVjdGlvblxuICBBIC0-PiBBQTogQzppZEFCIElOVFJPIEM6aWRBTSBpbmZvTTxicj4oaWRBQiAtIGNvbm4gYWxpYXMgQSBoYXMgZm9yIEIsPGJyPmlkQU0gLSBmb3IgTSlcbiAgQUEgLT4-IEE6IEM6aWRBQiBPS1xuXG4gIG5vdGUgb3ZlciBBLCBCQTogMi4gc2VuZCBpbnRybyB0byBCb2JcblxuICBBQSAtPj4gQkE6IHZpYSBpZEFCOiBJTlRSTyBDOmV4dEludHJvSWRNIGluZm9NXG4gIEJBIC0-PiBCOiBDOmlkQkEgUkVRIEM6aW50SW50cm9JZE0gaW5mb01cbiAgQiAtPj4gQkE6IEM6aWRCTT8gQUNQVCBDOmludEludHJvSWRNXG4gIEJBIC0-PiBCOiBDOmlkQk0gT0tcblxuICBub3RlIG92ZXIgQkE6IDMuIGNyZWF0ZSBjb25uZWN0aW9uIGZvciAgTSBpZEJNXG5cbiAgQkEgLT4-IEFBOiB2aWEgaWRCQTogSU5WIEM6ZXh0SW50cm9JZE0gcHJ2OmludkJNIGluZm9CXG5cbiAgbm90ZSBvdmVyIEFBLCBNOiA0LiBzZW5kIGludHJvIHRvIE1hcmtcblxuICBBQSAtPj4gTUE6IHZpYSBpZEFNOiBSRVEgQzpleHRJbnRyb0lkQiBwcnY6aW52Qk0gaW5mb0JcblxuICBub3RlIG92ZXIgTUEsIEI6IDUuIE1hcmsgY29ubmVjdHMgdG8gQm9iXG5cbiAgTUEgLT4-IE06IEM6aWRNQSBSRVEgQzppbnRJbnRyb0lkQiBpbmZvQlxuICBNIC0-PiBNQTogQzppZE1CPyBBQ1BUIEM6aW50SW50cm9JZEJcbiAgTUEgLT4-IE06IEM6aWRNQiBPS1xuXG4gIE1BIC0-PiBCQTogIGVzdGFibGlzaCBjb25uZWN0aW9uIGlkQk0gLT4gaWRNQlxuXG4gIG5vdGUgb3ZlciBBLCBNQTogNi4gbm90aWZ5IGFsbCBjbGllbnRzXG5cbiAgTUEgLT4-IE06IEM6aWRNQiBDT05cbiAgTUEgLT4-IEFBOiB2aWEgaWRNQTogQ09OIEM6ZXh0SW50cm9JZEJcbiAgQkEgLT4-IEI6IEM6aWRCTSBDT05cbiAgQkEgLT4-IEFBOiB2aWEgaWRCQTogQ09OIEM6ZXh0SW50cm9JZEJcbiAgQUEgLT4-IEE6IEM6aWRBQiBDT04gQzppZEFNXG4iLCJtZXJtYWlkIjp7fSwidXBkYXRlRWRpdG9yIjpmYWxzZX0)