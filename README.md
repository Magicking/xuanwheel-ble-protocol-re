# XuanWheel bluetooth protocol RE

The XuanWheel is a commercial bicycle wheel LED light "SPOKE". See https://www.amazon.com/dp/B0752Y8DSZ/ for more information.

This project aims to reverse engineer the protocol use to communicate with the
wheel in order to be able to send GIFs, bitmaps and receive telemetry without
of the need of the commercial APP.

## Wheel types

 * Fast Wheel (S?)
 * XuanWheel (legacy? X?)

## Codes (message control)

delay(time? 2.bytes): 7 bytes

 ```
 4.bytes header, value: 0x10700002 (big endian)
 2.bytes delay, value: delay time? TODO: units
 1 byte  checksum?, value: (0xa2 + delay[0]) + delay[1] truncated to 8 bits
 ```

length(length byte): 8 bytes

 ```
 5.bytes header, value: 0x1031000200 (big endian)
 1 byte length, value: payload size in byte?
 1 byte checksum?, value: TODO more work needed
 ```

## Wheel data return format

 ```
 3.bytes unknown, value: 0x7a7dff (big endian)
 [...]
 2.bytes unknown, value: 0x7fff (big endian)
 ```

## Common Commands list

### Battery

 ```
 6.bytes header, value: 0x106002000174 (big endian)
 ```

TODO: Description

## XuanWheel (Legacy)

### Name

 name(name [4;12].bytes)
 ```
 0-[4;12].bytes name, value: "BLE+".
 ```

Define wheel name

### Angle

 angle(angle 2.bytes (degree?))
 ```
 4.bytes header, value: 0x10420002 (big endian)
 2.bytes angle, value: angle (big endian)
 1 byte checksum?, value: TODO more work needed
 ```

Define angle of the wheel

### LowPowerMode

Content TODO

### GIFUpload

Upload modified GIF data to a wheel
TODO: gif data format gif

### BitmapUpload

Upload bitmap to a wheel

## Fast Wheel

### Name

 name(name [0;16[.bytes)
 ```
 3.bytes header, value: 0x7a7dff (big endian)
 2.bytes length, value: name.length (big endian)
 [0;16[.bytes name, value: C string
 3.bytes footer, value: 0x7e7fff
 ```

Define wheel name

### Angle

Define angle of the wheel

### LowPowerMode

Content TODO

### GIFUpload

Upload modified GIF data to a wheel
TODO: gif data format gif

### BitmapUpload

 upload_static(bitmap X.bytes)
 ```
 9.bytes header, value: 0x7a7dff300caf000100 (big endian)
 3240.bytes image data, (60x54 px?)
 1 byte checksum, value: chk
 3.bytes footer, value: 0x7e7fff (big endian)
 ```

 checksum calculation:
 1 byte chk
 1 byte B (pixel)
 chk = 0xEC
 for each B in bitmap
   chk = chk + B


Upload bitmap to a wheel

# TODO

 * Describe commands data structure
 * Do packet analysis
 * Describe image format

# Contribute

Use any APK decompiler or the iOS equivalent to look at 
the XuanWheel App and do documentation on how the app communicate
with the wheels.

Interesting files:
 - utils/bluetooth/command/CommandBuilder.java

Android: https://play.google.com/store/apps/details?id=com.ixuanlun.xuanwheel
iOS: https://itunes.apple.com/us/app/xuanwheel-your-riding-company/id1044544133
