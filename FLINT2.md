# Flint 2 ARM64 build

This fork can package upstream rs-matter examples as minimal ARM64 containers.
It does not currently implement Home Assistant's Matter Server WebSocket API.
The default `onoff_light_switch` example is a binding-based Matter switch that
must be commissioned and bound by another controller. It is not a replacement
for Home Assistant's Matter Server.

The workflow builds on changes to the container files on the
`flint2-arm64-container` branch. Once the workflow is also present on the fork's
default branch, it can be dispatched manually from the Actions tab with a
choice of example. The workflow uses a
native ARM64 runner, compiles with the RustCrypto backend inherited from
upstream, verifies there is no dynamic ELF interpreter, and publishes a
`scratch` image to GHCR. Image layers use gzip and Docker media types for the
Flint 2's Docker 20. No compiler or Rust toolchain is installed on the router.

The image tag includes the selected example and the source commit. Dependencies
are resolved during the build; commits alone do not pin dependency versions or
the builder image yet.

Use host networking for IPv6 and mDNS. Bind a directory on the checked USB disk
to `/data`; the upstream example stores its persistent state in
`/data/rs-matter`. The default switch example accepts keyboard input, so use an
interactive container when testing it. These examples use test device
attestation credentials. Treat them as experiments, not production accessories.

For a Home Assistant-compatible Rust controller, the next implementation must
provide persistent controller credentials, commissioning, operational device
discovery, attribute reads/writes, commands and subscriptions, and the Matter
Server WebSocket commands and events used by Home Assistant. That controller
binary then replaces the example as the container's build target.
