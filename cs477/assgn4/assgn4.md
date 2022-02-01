# CS 477 Assignment 4 - Stego

## Robert Detjens

---

### Examination

The subject of this look into steganography is a picture of Fergus the dog.
Taking this image at face value, this image is 511KB in size, which does not
match what would be expected for an only 437x611 image. Even if tis was an
uncompressed PNG, this would be 462KB, and as a JPG this should be significantly
smaller. Something is definitely hiding in this file.

### Investigation

Opening this file in a hex editor, we can tell that there are multiple files in
this image. The first file is the expected JPG, designated by the magic bytes
`JFIF`
