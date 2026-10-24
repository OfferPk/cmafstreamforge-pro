# CMAFStreamForge Pro (v1.0.0)

### ISO/IEC 23000-19 CMAF Chunking, Low-Latency DASH/HLS & MP4 Box Inspector Studio

---

## Overview

**CMAFStreamForge Pro** is an operational video streaming engineering workbench, MP4 box inspector, and automated manifest testing utility designed for OTT broadcasting networks, live sports streaming platforms, CDN edge providers, and video packager vendors. It implements Common Media Application Format chunking, synchronized dual manifest generation (LL-DASH & Apple LL-HLS), glass-to-glass latency budget modeling, and normative compliance auditing conforming to:

- **ISO/IEC 23000-19:2020**: Common Media Application Format (CMAF) for Segmented Media  
- **ISO/IEC 14496-12**: ISO Base Media File Format (ISOBMFF)  
- **IETF RFC 8216 / RFC 8216bis**: HTTP Live Streaming (HLS) & Apple Low-Latency HLS  
- **DVB-DASH (ETSI TS 103 285\)**: DVB Profile for MPEG-DASH with Low-Latency Extensions  
- **DASH-IF IOP v5.0**: Guidelines for Low-Latency Live Service Architecture  
- **SCTE 35 / SCTE 214-1**: In-Band Event Messaging via ISOBMFF `emsg` Box

---

## Key Capabilities

1. **CMAF Fragment & MP4 Box Inspector**:  
   - Analyzes ISOBMFF box hierarchies for initialization fragments (`ftyp` brand `cmfc`, `moov`, `mvhd`, `trak`, `mvex`) and media chunk fragments (`styp` brand `cmfs`, `prft`, `emsg`, `moof`, `mdat`).  
   - Decodes Producer Reference Time (`prft`) boxes binding 64-bit NTP timestamps to media decode clocks to detect live drift.  
   - Embeds and validates in-band SCTE-35 ad splice cue markers (`emsg` v1).  
2. **Glass-to-Glass Latency Budget Calculator**:  
   - Models end-to-end broadcast delay comparing legacy HLS (3 segments behind live edge \= 18–24s latency) with CMAF chunked transfer encoding (sub-second 0.8–1.5s latency).  
   - Generates visual latency budget breakdowns across encoder chunking, packager buffering, CDN HTTP Chunked Transfer, and player jitter buffers.  
3. **Synchronized Dual Manifest Studio**:  
   - Synthesizes compliant **Low-Latency MPEG-DASH (MPD)** manifests with `availabilityTimeComplete="false"` and `availabilityTimeOffset`.  
   - Synthesizes compliant **Apple Low-Latency HLS (m3u8)** playlists with `#EXT-X-SERVER-CONTROL:CAN-BLOCK-RELOAD=YES`, `#EXT-X-PART-INF`, `#EXT-X-PART`, and `#EXT-X-PRELOAD-HINT`.  
   - 1-click clipboard copy and file downloads (`.mpd` and `.m3u8`).  
4. **10-Point Normative Compliance Auditor**:  
   - Automated test harness validating CMAF brand declarations, single initialization headers, chunk sequence numbering, `prft` clocks, `emsg` v1 schemas, chunk duration bounds, and dual manifest directives.  
5. **Production Python CLI (`cmaf-cli.py`)**:  
   - Standalone CLI utility for automated OTT release pipelines, supporting `inspect`, `manifests`, `budget`, and `audit` operations.

---

## Getting Started

### Web Studio

Open `source-code/index.html` in any web browser. Runs 100% locally with zero external network or cloud dependencies.

### Command-Line Interface

\# Inspect CMAF box hierarchy

python3 build/cmaf-cli.py inspect \--chunk 200 \--seq 1042

\# Generate synchronized LL-DASH and LL-HLS manifests

python3 build/cmaf-cli.py manifests \--chunk 0.200 \--segment 2.0

\# Calculate glass-to-glass latency budget

python3 build/cmaf-cli.py budget \--segment 2.0 \--chunk 0.200

\# Execute normative compliance audit

python3 build/cmaf-cli.py audit

### Automated Testing

python3 test\_cmaf.py