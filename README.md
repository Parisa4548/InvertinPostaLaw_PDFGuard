# InvertingPostaLaw_PDFGuard
## Problem Statement
The current implementation of the Robustness Principle (Postel’s Law) has long been a concern for security as well as a cause for software bloat. Currently, it is common practice for those in network administration to implement guards/firewalls to filter traffic that doesn’t meet certain rules in order to reduce the attack surface of the network. However, this type of guard is not typically available for specific file types.
## Purpose of the Project
By implementing a content filter for the PDF file format, based on ISO 32000, we will be able to measurably reduce lines of code activated in corresponding PDF applications. This reduction will also affect a corresponding reduction in an application's attack surface by making vulnerable features unavailable.
## Implementation
The guard is designed to only pass whitelisted PDF files that have no compression, no JavaScript, email links, URLs and in general no embedded action function in their meta data. Furthermore, the type of font and image are also limited to a few types. By reading PDF as a simple text file in the guard code, the meta data details are accessible. We read the file line by line and look for the specific types of font, image, compression filters, encryption filters and the action function tags. JPEG images are stored with /XObject tag in a decompressed PDF file. "ASCIIHexDecode", "ASCII85Decode", "LZWDecode", "FlateDecode", "RunLengthDecode", "CCITTFaxDecode", "JBIG2Decode", "DCTDecode", "JPXDecode", "Crypt" and "Encrypt" is the list of compression and encryption tags that were are looking for in the body of metadata to be rejected if the guard detects them. Action functions are represented as "URI" "Launch", "JavaScript", "GoTo", " GoToR", "GoToE" tags. The PDF files containing them will not pass by guard as well.