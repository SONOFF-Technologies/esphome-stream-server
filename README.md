# ESPHome Stream Server

A maintained fork of [oxan/esphome-stream-server](https://github.com/oxan/esphome-stream-server), a custom ESPHome component that exposes a UART stream over Wi-Fi or Ethernet.

## About this fork

The original `esphome-stream-server` project has been useful to us, but the upstream repository has not been actively maintained while newer ESPHome releases have introduced compatibility changes.

We use this component in our official ESPHome-based builds, so we created this fork to keep it compatible with newer ESPHome versions and to address issues encountered in our own use.

This repository is publicly available, and anyone is welcome to use it in their own ESPHome projects.

## Changes in this fork

Compared with the upstream repository, this fork currently includes the following fixes:

### ESPHome compatibility

- Replaced version-specific handling of `uart.request_wake_loop_on_rx()` with feature detection.
- Updated network address handling for the breaking change to `get_use_address()` introduced in ESPHome 2026.7.
- Added support for the newer `network::get_use_address_to()` API where required.

### TCP client cleanup

- Improved handling of stale and half-open TCP clients.
- Added handling for `ENOTCONN` and `EPIPE` socket errors in addition to the existing connection reset handling.
- Affected clients are marked as disconnected and removed through the existing cleanup logic.

### Fix TCP latency caused by empty iovec

- Avoid passing an empty second `iovec` to `writev()` when the ring buffer does not wrap.
- Pass the actual number of populated `iovec` entries while preserving two-part writes when the ring buffer wraps.
- This fixes severe latency observed with request/response protocols such as XMODEM, particularly when using Windows clients.

## Usage

Configuration and usage are unchanged from the upstream project.

Please refer to the upstream documentation for usage examples, sensors, multiple stream servers, buffer configuration, and other options:

[Upstream README: oxan/esphome-stream-server](https://github.com/oxan/esphome-stream-server#readme)

When using this fork, make sure your `external_components` source points to this repository instead of the upstream repository.

For example:

```yaml
external_components:
  - source: github://SONOFF-Technologies/esphome-stream-server

stream_server:
```

## Upstream project

This project is based on [oxan/esphome-stream-server](https://github.com/oxan/esphome-stream-server).

We sincerely thank **Oxan van Leeuwen** and the contributors to the original project for their work. The original project provides the foundation for this maintained fork.

For issues specifically related to this fork or compatibility with newer ESPHome versions, please report them in this repository.

## Contributions

This repository is primarily maintained for our own ESPHome-based products, but it is also available for community use.

Bug reports, compatibility fixes, and pull requests are welcome.

## License

This project retains the license and copyright notices of the upstream project.

See [LICENSE.txt](LICENSE.txt) for the complete license terms.