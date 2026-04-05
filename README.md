# NMSF: Neural Video Codec Packaging for MOQT Streaming Format

**Internet-Draft:** `draft-herz-moq-nmsf-00`
**Author:** Erik Herz, Vivoh, Inc.
**Status:** Individual Draft
**Working Group:** Media Over QUIC (moq)

## Overview

NMSF defines a new packaging type for the MOQT Streaming Format (MSF)
that enables Neural Video Codecs (NVCs) to be transported over Media
over QUIC (MoQ). It follows the same extension pattern as CMSF (which
adds CMAF packaging to MSF), registering `"nvc"` as a new packaging
value alongside `"loc"` and `"cmaf"`.

Neural video codecs like DCVC-RT, SSF, and FVC produce compressed
bitstreams that are fundamentally different from traditional codecs -
entropy-coded latent tensors rather than NAL units or fMP4 boxes.
NMSF provides a purpose-built packaging format that carries this data
natively over any standard MoQ relay without relay-side codec awareness.

## Key Design

- **Neural keyframes (Intra)** map to the first Object in each MoQ Group
- **Neural delta frames (Inter)** are subsequent Objects within the Group
- **17-byte fixed header** per Object (frame type, sequence, dimensions, payload length)
- **Codec-agnostic** - works with any NVC that produces Intra/Inter frames
- **Relay transparent** - standard MoQ relays forward Objects without modification

## Documents

| File | Description |
|---|---|
| [`draft-herz-moq-nmsf-00.xml`](draft-herz-moq-nmsf-00.xml) | RFC 7991 XML source |
| [`draft-herz-moq-nmsf-00.txt`](draft-herz-moq-nmsf-00.txt) | Plaintext rendering |

## Building

```bash
pip install xml2rfc
xml2rfc draft-herz-moq-nmsf-00.xml --text    # plaintext
xml2rfc draft-herz-moq-nmsf-00.xml --html    # HTML
```

Or use the IETF Author Tools: https://author-tools.ietf.org

## Related Specifications

- [MSF (MOQT Streaming Format)](https://datatracker.ietf.org/doc/draft-ietf-moq-msf/) - Base streaming format
- [CMSF (CMAF for MSF)](https://datatracker.ietf.org/doc/draft-ietf-moq-cmsf/) - CMAF packaging extension
- [MoQ Transport](https://datatracker.ietf.org/doc/draft-ietf-moq-transport/) - Transport protocol

## Discussion

Feedback and discussion on this draft takes place on the
[MoQ working group mailing list](mailto:moq@ietf.org)
([archive](https://mailarchive.ietf.org/arch/browse/moq/)).

## Reference Implementation

A reference implementation (publisher + subscriber) is available at
[nvcmoq.com](https://nvcmoq.com).
