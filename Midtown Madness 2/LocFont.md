# LocFont
A LocFont is a string such as `Arial, 20, 36, 0, 400` used inside the MMLANG file.

These parameters map to
`Font name, small size, large size,  char set, weight`

The small size is used when the screen *width* is less than 640. The large size is used when the screen *width* is greater than or equal to 640.

For an explanation on the char set, weight, and the way size works, see cHeight/cWeight/iCharSet parameters here: https://learn.microsoft.com/en-us/windows/win32/api/wingdi/nf-wingdi-createfonta
