# Project: Matrix E2E Bridge

This project is (shall become) a dynamic group of people with the goal to add support for bridged & E2E encrypted communication to the Matrix spec and its major clients.

## Communication

- Matrix Space (all relevant rooms & people grouped together): [#matrixe2ebridge:matrix.org][matrix-space]
- Matrix Rooms (if your client does not support spaces yet):
    - Announcements (moderated; [#matrixe2ebridge-announcements:matrix.org][matrix-announcements])
    - General ([#matrixe2ebridge-general:matrix.org][matrix-general])

## How To Contribute

If you want to share your ideas, issues you see, solutions or anything else related to the project, for now please write into the [general Matrix room][matrix-general].

If you want to join me/us in the journey, you can write in the [general Matrix room][matrix-gneral] as well. Please submit your GitHub account name/link as well, so you may be added to organization.

## What's the problem?

Many chat services & protocols (Matrix, WhatsApp, Signal, Threema, …) support End-to-End encryption between users of the same platform.
Matrix supports bridging one-to-one messages and groups between itself and other services, mostly transparently after first setup.
However, no End-to-End encryption is possible due to all services using different encryption schemes and lack of support in the Matrix spec & clients.
So far, only End-to-Bridge encryption is supported, which is better than nothing but still requires you to trust the bridge & its security mechanisms.

It is understandable that the Matrix spec so far has no built-in support for that.
The current bridging experience is way better than nothing and adding support for End-to-End encryption either requires both services to use & support the exactly same encryption methods or requires implementing the other encryption schemes into one of both services' clients.
That is why I started this project: I think it may be much work to implement, but I belive it is possible and makes Matrix and its services more attractive.

## What might be the solution?

In short, the solution might be:

- bridges will stop encrypting & decrypting messages they relay
- bridges will expect (if the user enables it in his supported client) the Matrix client to encrypt messages properly
- bridges will have the required *configuration* (see below), append it to their respective (portal) rooms for clients to look up
- clients (which may support the required cryptography) will request the *configuration* and use it to encrypt / decrypt messages
- clients will generate the required keys themselves and store them securly at the Matrix server for all other clients to see
- clients will store public keys of the recipients in the portal room along with their verification status
- clients may support the verification process like the native clients (if it exists) according to the *configuration*

### Configuration

To work properly for all services, the so called *configuration* is required to be as versatile as possible (to support most services) but still manageable for Matrix clients so they be able to support them.

The *configuration* might be
- a reference to a commonly known crypthograpic algorithm
- a (simple) combination of cryptograhpy primitives
- cross-plattform executable code
- *more ideas are welcomed*

So *configurations*:
- depend on the service targeted by the bridge
- are required by the client to perform the encryption & decryption of messages
- are defined & held by the bridge, will be appended to / referenced by the portal rooms

### Requirements

- The bridge should only require the same level of trust as the Matrix server and the service behind, i.e.:
    - The bridge should not be able to read the messages it relays
    - It should become possible for a user to use a bridge provided by its server provider without sacrificing privacy and without trusting him more than already
- There should be as less room as possible for the encryption being
    - insecure implemented & executed
    - hijackable by the bridge (e.g. bridge declare that further messages must not be encrypted)
- The process should be as user-friendly as possible and integrate seemingless into the current user experience.
- Aside of cryptographical primitives, as it is now, clients should not be required to implement special support for the other messaging service. Only bridges should be required to have special support for the service they are bridging to.
- Clients not supporting the required cryptographical primitives for bridging E2E must be able to inform their user about that.
- Messages which couldn't relayed to the service because of missing client support must be marked and the bot should at least inform the sending user about that issue.
- Users should be easily able to enforce using E2E bridging in direct messages and then should be informed if the room they use only supports End-to-Bridge encryption.
- Groups should be able to enforce their users to use E2E bridging like they enforce use common E2E.
- For users trusting their bridge and its security and using clients not supporting E2E bridging, it should still be possible to use End-to-Bridge encryption.
- For groups which don't want their users to enforce supporting E2E encryption at the client side, it should still be possible to use End-to-Bridge encryption.
- Bridging to services, which don't (properly) support E2E encrpytion, should still be possible using End-To-Bridge encryption


## License

All work on the upcoming proposals to the Matrix spec, the implentations to different Matrix clients and other repositories belonging to this organization are, if not stated otherwise in the respective repository, licensed under the Apache License, Version 2.0.
You can find a copy of the license terms [here][license].


<!-- Links -->

[license]: https://github.com/matrix-e2e-bridge/.github/blob/main/LICENSE
[matrix-announcements]: https://matrix.to/#/#matrixe2ebridge-announcements:matrix.org
[matrix-general]: https://matrix.to/#/#matrixe2ebridge-general:matrix.org
[matrix-space]: https://matrix.to/#/#matrixe2ebridge:matrix.org
