## Release 2.5.0

- Structured validation errors: `error_handler::error(const validation_error &, const json &)`
  reports the failed JSON Schema `keyword` and a machine-readable `details` object alongside the
  existing location, instance and message. The previous three-argument callback remains
  source-compatible and is called by default. `required` and array-valued `dependencies`
  errors identify the missing property without repeating the complete keyword array in every
  error. Fixes #352, related to #321 and #322.
- Added `json_validator::is_valid()` for validity-only checks: one evaluation, no diagnostics,
  no default values.
- ABI change: `error_handler` gains a virtual function. Binaries linked against 2.4.x must be
  rebuilt; the library `SOVERSION` is unchanged.
- Logical combinations (`allOf`, `anyOf`, `oneOf`) evaluate their branches for validity first
  and report the summary error from that result; when the handler does not throw, the failed
  branches are then evaluated again to deliver their individual errors. Errors are no longer
  buffered; custom `format` or content checkers may therefore be called more than once for the
  same value when validation fails. The `allOf` summary is now reported at the combination's
  own location with a fixed message, like `anyOf` and `oneOf`; the failing branch's first error
  follows it instead of being embedded in the summary message.
- `additionalProperties` failures are reported individually at the offending property instead
  of as a single summary at the containing object. `additionalProperties`, `additionalItems`
  and `propertyNames` set to `false` report a direct message.
- Tuple `items` and `additionalItems` failures report the correct array index (previously
  always `/0`).
- Each violated numeric constraint produces a separate error (previously concatenated).

## Release 2.4.0

- Added CI job to publish GitHub release by @JohanMabille in <https://github.com/pboettch/json-schema-validator/pull/367>
- Maintenance to Fedora CI infrastructure by @LecrisUT and @JohanMabille in <https://github.com/pboettch/json-schema-validator/pull/363>
- Reference validation using contains() result rather than exception handling by @BalrogOfHell in <https://github.com/pboettch/json-schema-validator/pull/334>
- add support for $defs instead of definitions by rpatters1 in <https://github.com/pboettch/json-schema-validator/pull/338>
- Apply clang-format / fix "test / Check pre-commit" failures by @serge-s in <https://github.com/pboettch/json-schema-validator/pull/328>
- Adding verbose error messages for logical combinations by Csaba Imre Zempleni in <https://github.com/pboettch/json-schema-validator/pull/310>
- fix: issue-311 by andrejlevkovitch
- Fix cmake install target on windows by @barts-of in <https://github.com/pboettch/json-schema-validator/pull/315>
- error-messages: Numeric limit errors should show maximum precision by @pboettch
- Add Fedora packaging by @LecrisUT in <https://github.com/pboettch/json-schema-validator/pull/264>
- Improve and fix bugs in Conanfile by Jacob Crabill
