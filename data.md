# Data Type

- psmb -> modbusd
    - decimal array, ex: [255, 123, 456]
    - so convert decimal/hex string to decimal uint16 array

- modbusd -> psmb
    - decimal array, ex: [255, 123, 456]

- web service -> psmb
    - decimal string, ex: "255,123,456"
    - hex string, ex: "AB12EF34" (4n)

- psmb -> web service
    - first convert word array to byte array
    - big/little endian hex string, ex: "AB12EF34"
        - input: "112C4F"
        - output: "4F2C11"
    - variable conveter
        - uint16/int16 array (2's)
        - uint32/int32 array (4's)
        - 32-bit float array (4's)
    - scale value array (0, 65536) -> (0, 10)

- web service -> ui (swap at ui)
    same as psmb -> web service   

- ws -> psmb
    same as web service -> psmb

- psmb -> ws
    same as psmb -> web service

- mqtt -> psmb
    same as web service -> psmb

- psmb -> mqtt
    same as psmb -> web service

- ui -> web service
    same as web service -> psmb

