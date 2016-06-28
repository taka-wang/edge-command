# Data Type

## Layer1
- psmb -> modbusd
    - decimal uint16 word array, ex: [255, 123, 456]
    - so convert decimal/hex string to decimal uint16 word array (2bytes array)

- psmb <- modbusd
    - decimal uint16 word array, ex: [255, 123, 456]

## Layer2

- psmb -> web service
    - first of all, convert word array to byte array
    - byte array response
    - big/little endian hex string, ex: "AB12EF34"
        - input: "112C004F"
        - output: "4F002C11"
    - variable conveter
        - uint16/int16 array (2's)
        - uint32/int32 array (4's)
        - 32-bit float array (4's)
    - scale value array (0, 65536) -> (0, 10)

- web service -> psmb
    - decimal string, ex: "255,123,456"
    - hex string, ex: "AB12EF34" (4n)

- ws -> psmb
    same as web service -> psmb

- psmb -> ws
    same as psmb -> web service

- mqtt -> psmb
    same as web service -> psmb

- psmb -> mqtt
    same as psmb -> web service

## Layer3

- web service -> ui (swap at ui)
    same as psmb -> web service   

- ui -> web service
    same as web service -> psmb

## Layer3

- human -> ui
    - decimal string, ex: "255,123,456" convert to 16bit hex array (0x00FF, 0x00FA,..)
    - hex string, ex: "AB12EF34" (4n)

- ui -> human
    - uint16/int16 array (2's)
    - uint32/int32 array (4's)
    - 32-bit float array (4's)
    - scale value array
    - hex word array (big/little endian)
    - decimal array

---
## Example:

112C004F (4396, 79)

- float (ABCD) big endian
    112C004F (1.35685027e-28)
- float (DCBA) little endian
    4F002C11 (2150371580)
- float (BADC)
    2C114F00 (2.06495931e-12)
- float (CDAB)
    004F112C (7.261164e-39)
- uint32 (ABCD) big endian
    112C004F (288096335)
- uint32 (DCBA)
    4F002C11 (1325411345)
- uint32 (BADC)
    2C114F00 (739331840)
- uint32 (CDAB)
    004F112C (5181740)
- int32 (ABCD) big endian
    112C004F (288096335)
- int32 (DCBA)
    4F002C11 (1325411345)
- int32 (BADC)
    2C114F00 (739331840)
- int32 (CDAB)
    004F112C (5181740)
- int16 (AB) big endian
    112C004F (4396,79)
- int16 (BA) little endian
    2C114F00 (11281,20224)
- uint16 (AB) big endian
    112C004F (4396, 79)
- uint16 (BA) little endian
    2C114F00 (11281,20224)

- 16bit hex
    - min: 0000, max: FFFF
    - value: 0, 100