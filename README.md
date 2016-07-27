= NFC Tag Auth =

* WARNING: SUPER WIP *

NVC tag auth is a standard and sample implementation of basic autorization using inexpensive, commonly available NFC-compatable components

ntagAuth makes the following assumptions,
    the signed data on a card is not considered secret, and only has value when it is validated by a reader, no claim is made of privacy, or handshaking.
    authCards are valid from, or until a speicific time, and should be updated (either after the card is read, or at a self service kiosk)


Data Format
|Order|Type|Data|
|----:|---:|:---|
|1|uint16|length of signed package|
|2|uint64|orginazation ID|
|3|uint16|parent ID|
|4|byte[64]|signature|
|5|string|subject|
|6|uint64|Not Valid Until Epoch|
|7|uint64|Not Valid After Epoch|
|8|uint16|payloadType|
|9|byte[n]|payload|

== Length of signed package ==
The lenght, in bytes of the signed package, which includes the start/stop epochs  of valididity, as well as the payloads (and included entitlements)

== Organization ID ==
A machine redable ID, while not required to be unique, cards with mismatching IDs will be told to keep quiet, and will not be recorded as login failures (but large numbers of them are a concern
Considder using a mac address concatinated with a serial as a unique value source

== Parent ID ==
A number ID corresponding to a unique certificate that the payload is signed with, this is to be administrated by the organization

== Signature ==
A SHA-512 signature of the card's unique identifier (e.g. Serial Number), concatinated with the signed package (fields 5+).

== Subject ==
A unique identifer for the Card (e.g. serial number), prefixed with its length (max length 256)

== Not Valid Before/After Epoch ==
Times in the UNIX epoch (Zulu, no DST) that define the range of time for which the package is considered valid.

== Payload Type ==
The type of the following paylod, value FFFF is reserved for experimentation, values F000-FFFE are reserved for individual use by orginazations, all other values are reserved for future use

== Payload ==
The data here contains entitlements (spec TBD), this area of data is what is verified and bound by the above header, this data is NOT considered secret, and could be encrypted, the header spec does not define this data.
