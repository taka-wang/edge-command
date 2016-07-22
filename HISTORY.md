# Backlog / Wish List
- [ ] set bits from byte (FC15) (only support big endian, i.e., 0x12345678)
    - length: octets
    - heximal or decimal?
- [ ] get bytes from bits (FC1, FC2) (only support big endian, i.e., 0x12345678)
    - length: octets
    - heximal or decimal?
- [ ] byte swap (FC3, FC4) (default: big endian, little endian) :thumbsup:
- [ ] read heximal/decimal (FC3, FC4) :thumbsup:
- [ ] write heximal/decimal (FC6, FC15, FC16) :thumbsdown:
- [ ] get 32-bit IEEE-754 float (FC3, FC4)
    - length: even number
        - ABCD [Big Endian]
        - BADC (byte swap) [Mid-Big Endian]
        - CDAB (word swap) [Mid-Little Endian]
        - DCBA (byte and word swap) [Little Endian]
- [ ] set 32-bit IEEE-754 float (FC16) :thumbsdown:
    - length: even number
- [ ] on changed filter (bit, bits, word, words; FC1~FC4) :clap:
- [ ] threshold (FC3, FC4) :question:
- [ ] scaling :scream:
- [ ] number and address mapping :fearful:

# TODO

- [ ] support nickname for mqtt, websocket and web service