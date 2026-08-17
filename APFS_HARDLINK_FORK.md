# APFS Hard-Link Transport Fork

Branch: `main`

Upstream base: `fde790d8ea41283606d1c56b557608a17e455f59` (`v2.2B4`)

This branch adds native Windows hard-link transport to WinFsp for APFS for
Windows. It extends the existing fixed-size interface by consuming one reserved
callback slot, transports `FileLinkInformation` and `FileLinkInformationEx`,
reports link counts, advertises `FILE_SUPPORTS_HARD_LINKS`, and maps FUSE
`link` operations.

Current verification:

- `git diff --check` passes.
- `winfsp-x64.dll` and import library build with Visual Studio 2022.
- Driver project now targets the latest installed Windows 10 SDK and declares
  the Windows 10 API level required by `FileLinkInformationEx`.
- APFS for Windows builds with `/W4 /WX` against this ABI.
- APFS for Windows CTest passes 12/12 with this runtime DLL.
- Copied APFS core plus macOS kernel mount and three native `fsck_apfs` hard-link
  tests pass.

Production blockers:

- clean WDK kernel-driver build and kernel test suite coverage;
- exact driver/DLL ABI lifecycle and native `CreateHardLinkW` test evidence;
- dedicated regression coverage for `FileLinkInformationEx`;
- Microsoft production driver signing without Test Mode.

Do not install or distribute this branch as a production driver until those
gates are closed.
