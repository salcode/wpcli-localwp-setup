# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## Unreleased
### Removed
- Remove `sshlbf` script since websites no longer run inside docker this script is unnecessary.

### Changed
- Support LocalWP (branded name on latest version) rather than Local By Flywheel (previous version).
- Renamed script `wpcli-localwp-setup` (previously named `wpcli-lbf-setup`)
- Update script to take `socket` as input rather than Database host and port.
- Modify **DB_HOST** value in generated `wp-cli.local.php` files for LocalWP.
- Replace error_reporting value of `1` with `E_ERROR`

## [1.0.0] - 2021-09-24

- Initial release - compatible with Local By Flywheel which used Docker to run the websites.
