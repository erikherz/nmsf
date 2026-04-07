# NMSF: Neural Video Codec Packaging for MOQT Streaming Format

**Internet-Draft:** `draft-herz-moq-nmsf-01`\
**Author:** Erik Herz, Vivoh, Inc.\
**Status:** Individual Draft\
**Working Group:** Media Over QUIC (moq)

## Overview

NMSF defines a new packaging type for the MOQT Streaming Format (MSF)
that enables Neural Video Codecs (NVCs) to be transported over Media
over QUIC (MoQ). It follows the same extension pattern as CMSF (which
adds CMAF packaging to MSF), registering `"nvc"` as a new packaging
value alongside `"loc"` and `"cmaf"`.

Neural video codecs like DCVC-RT, SSF, and FVC produce compressed
bitstreams that are fundamentally different from traditional codecs —
entropy-coded latent tensors rather than NAL units or fMP4 boxes.
NMSF provides a purpose-built packaging format that carries this data
natively over any standard MoQ relay without relay-side codec awareness.

## Key Design

- **Two-track model** — separates the hyperprior (side information) and latent (frame data) into independent MoQ tracks with priority-based delivery
- **Neural keyframes (Intra)** map to the first Object in each MoQ Group
- **Neural delta frames (Inter)** are subsequent Objects within the Group
- **26-byte fixed header** per Object (frame type, QP, sequence, PTS timestamp, dimensions, payload length)
- **Per-frame PTS timestamp** (`pts_ms`) for end-to-end latency measurement
- **Per-frame quality parameter** (`qp`) for adaptive bitrate signaling
- **Codec-agnostic** — works with any NVC that produces Intra/Inter frames
- **Relay transparent** — standard MoQ relays forward Objects without modification
- **Single-track fallback** for simpler deployments

## Documents

| File | Description |
|---|---|
| [`draft-herz-moq-nmsf-01.xml`](draft-herz-moq-nmsf-01.xml) | RFC 7991 XML source (current) |
| [`draft-herz-moq-nmsf-00.xml`](draft-herz-moq-nmsf-00.xml) | Previous revision |
| [`draft-herz-moq-nmsf-00.txt`](draft-herz-moq-nmsf-00.txt) | Previous revision (plaintext) |

## Changes in -01

- Added two-track model: separate hyperprior and latent MoQ tracks with `nvcRole`, `depends`, and `priority` catalog fields
- Added per-frame PTS timestamp (`pts_ms`) to wire format header
- Added per-frame quality parameter (`qp`) to wire format header
- Wire format header expanded from 17 bytes to 26 bytes
- Added two-track decode sequence to decoder requirements
- Retained single-track mode as a simpler alternative
- Normalized payload sub-format with tensor shape metadata preceding each bitstream component

## Building

```bash
pip install xml2rfc
xml2rfc draft-herz-moq-nmsf-01.xml --text    # plaintext
xml2rfc draft-herz-moq-nmsf-01.xml --html    # HTML
```

Or use the IETF Author Tools: https://author-tools.ietf.org

## Related Specifications

- [MSF (MOQT Streaming Format)](https://datatracker.ietf.org/doc/draft-ietf-moq-msf/) — Base streaming format
- [CMSF (CMAF for MSF)](https://datatracker.ietf.org/doc/draft-ietf-moq-cmsf/) — CMAF packaging extension
- [MoQ Transport](https://datatracker.ietf.org/doc/draft-ietf-moq-transport/) — Transport protocol

## Discussion

Feedback and discussion on this draft takes place on the
[MoQ working group mailing list](mailto:moq@ietf.org)
([archive](https://mailarchive.ietf.org/arch/browse/moq/)).

## Reference Implementation

A reference implementation (publisher + subscriber) is available at
[nvcmoq.com](https://nvcmoq.com).
