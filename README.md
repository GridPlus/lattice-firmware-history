# Lattice Firmware History

The following document contains a list of all publicly released Lattice firmware updates. Generally they contain descriptions of major features as well as changelogs of specific pull requests and commits that were added to the codebase in that version.

## v0.11.0

*Published August 12 2021*

**Features:**

* Adds support for new Ethereum transaction types (EIP1559 and EIP2930)

**Bug Fixes:**

* Add zero-initialization to parameter only affected by optimization
* Ensure cases where `chainId` is large work with pre-hashed transaction requests
* Ensure SafeCard seed always deletes, even when Lattice seed is the same

**Changelog:**

* Fixes zero-initialization bug that only manifests in optimized code (#2078)
* Fixes edge case where `chainId` is large and also the payload is pre-hashed (#2111)
* Fixes issue in delete SafeCard seed when SafeCard and Lattice seeds are the same (#2106)
* Add support for EIP1559 and EIP2930 transaction types (#2112)
* Optimize remaining firmware code (#2093)
* Update to v0.11.0 (#2115)
* Fixes pointer scoping issue in EIP1559 implementation (#2116)
* Optimize one last file (pairing manager) (#2120)
* Fix typo in screen header (#2118)

## v0.10.11

*Published July 27 2021*

**Changelog:**

* Adds missing types to EIP712 decoding (#2089)
* Fixes edge case related to printing ABI markdown of `bytes` types (#2091)
* Bump version (#2092)
* Fix compiler warning related to unknown return value (#2098)
* Shrink code space usage of EIP712 type selection (#2101)

## v0.10.10

*Published July 13 2021*

**Features:**

* Allows pre-hashed EIP712 and personal_sign messages in cases where the messages are too large to fit in allocated firmware memory. A warning screen is displayed as is the case for Ethereum transactions, which already support this feature.

**Changelog:**

* Adds pre-hash mechanism for ETH_MSG (#2085)

## v0.10.9

*Published July 6 2021*

**Features:**

* Displays native unit for non-Ethereum chains in transaction request screen. Supported chain units include: MATIC, BNB, FTM as well as Ethereum testnets Ropsten, Rinkeby, Kovan, and Goerli. All other chains display the unit `?TOKEN` to indicate the chain is unknown (the signing is unaffected).
* Updates to the ABI parser adding support for signed integer types and fixes an edge case for parsing 0-length `bytes` types.

**Changelog:**

* Display native unit for EVM chain based on `chainId` (#2066)
* ABI: support signed int types (#2071)
* ABI decoding edge case: handle 0-length `bytes` types (#2062)
* Bump to v0.10.9 (#2074)
* Fix issue introduced in EVM unit display (#2076) 
* Fix issue introduced in signed int support (#2077)

## v0.10.8

*Publised June 11 2021*

**Features:**

* Updates to Ethereum ABI functionality: new types, updated struct, new files (breaking changes)
* Support for sending to Bitcoin bech32 addresses (using p2wpkh)
* Ability to remove SafeCard wallet from firmware UI
* Allow pre-hashed ETH transactions when data is too large (prints a red warning screen)
* Various minor updates/fixes

**Changelog:**

* Accounting for ABI edge case where parameter has no name (#2025)
* Add all non-standard uint ABI types (e.g. uint24) (#2030)
* Remove gas constraints for ETH transactions (#2033)
* Add support for sending to bech32 Bitcoin addresses (#2017)
* Display remaining SafeCard PIN attempts whenever an incorrect PIN is entered (#2038)
* Add ability to remove wallet on SafeCard from UI (#2045)
* Various firmware-based changes to ABI structure (breaking change) (#2054)
* Allow pre-hashed ETH txs when data is too large; red warning screen is displayed (#2051)
* Update language when asking for optional 25th seed word (i.e. passphrase) (#2056)
* Revert changes in previous version for skipping wifi setup if SSID exists (#2035)
* Version bump (#2055)
* Adds missing display of SafeCard PIN attempts remaining (#2060)

## v0.10.7

*Published May 7 20201*

**Features:**
* Screen to validate authenticity of the Lattice's secure enclave chip. Generates a QR code linking to a gridplus.io page and containing signature + certificate on signer.

**Bug fixes:**
* Fixes bug in UI callback function that was introduced in previous version
* Allows for EIP712 edge case that was previously not supported, but is allowable by the spec

**Changelog:**
* Fixes UI bug introduced in previous version (#2012)
* Adds screen to validate authenticity of Lattice's secure enclave (#2006)
* Adjusts screen constants that got mis-aligned (#2015)
* Adds support for EIP712 edge case related to 0-length parameters (#2016)
* Additional support for tamper mesh in latest hardware version (#2020)

## v0.10.6

*Published May 3 2021*

**Features:**
* Support for EIP712 transactions.
* Support for derivation paths between 2-5 indices instead of requiring 5 indices.
* Adds ability to scroll between used addresses on the Lattice UI. Currently only BTC addresses, which have a gap limit, can display more than the root address but more may be added later.

**Bug Fixes:**
* Add timeout mechanism to PIN screen, which previously blocked transition to the UI sleeping after inactivity.
* Skip wifi setup step when the Lattice system is reset and the user still has wifi configured.
* Fixes invalid callback handling which made canceling a seed restore entry more difficult than it needed to be.

**Changelog:**

* Support for derivation paths <5 indices (#1974)
* Modularize vendor libs and optimize compilation (#1975)
* Move RLP module to GridPlus handle and swap submodules (#1976)
* Update address display mechanism to allow scrolling between cached addresses (#1954)
* Support for EIP712 requests (#1982)
* Additional keyring support for hardware v3 (#1978)
* Add missing timeout to PIN screen (#1984)
* Skip wifi setup when wifi is already set (#1985)
* Adds support for non-standard EIP712 domain headers since apps are already breaking the standard ????  (#1991)
* Adds missing "cancel" callback handler to restore screen (#1995)
* Replaces invalid header button type on BIP39 password screen (#1996)
* Hides unnecessary screen builder constants to avoid exporting constants we don't need to export (#1997)
* Fixes bug in exporting certain address paths (#2008)

## v0.10.4

*Published March 31 2021*

**Features:**

* Updates `signTransaction` request to allow for requests larger than one frame. Previous limit was 1550 bytes for data (same limit for a `personal_sign`. message). This has now been updated to 1550 bytes + 1x 1500 byte frame for a total of 3050 bytes. (#1945, #1948)
* Updates the look of the PIN screen, most notably using a larger font for the number buttons. (#1946)
* Adds BIP32 signer path to transaction request screen (#1971)

**Bug Fixes:**

* Blocks requests while ABI confirmation screen is drawn. This fixes a bug in which encrypted requests could still be made but could not be filled because the ABI confirmation screen was blocking the UI. (#1947)
* Forces SafeCard unlock before accessing a cached request when the device is unlocked. This fixes a bug that allowed the device to sign a transaction from a SafeCard without requiring the user to unlock that SafeCard first. This bug was only relevant for locked devices with cached requests. (#1949)

**Changelog:**

* #1945: Updates `signTransaction` route and all ETH transaction middleware to handle chained requests, effectively increasing the maximum transaction size by using multiple requests.
* #1948: Increases `extraData` frame size and stack size to support a significantly larger `signTransaction` payload under the new mechanism.
* #1947: Adds hook into ABI commit screen preventing inbound encrypted requests while the screen is drawn.
* #1949: Adds more context to system unlock and adds specificity over `home_ui` redirect when tearing down loading screen.
* #1946: Updates PIN screen to use larger font
* #1950: Bump to version 0.10.4
* #1955: Minor code cleanup to adjust comments and remove dead code
* #1957: Adds missing cache clearing mechanism which should have been included in #1948
* #1964: Fixes timeout mechanism when resetting router
* #1971: Prints BIP32 signer path on ETH and ETH_MSG signing request screens

## v0.10.3

*Published March 17 2021*

**Features:**

* Better detection of the SafeCard. If a SafeCard is inserted when the device boots, it is now detected. If a SafeCard is inserted in a device that is unlocked, the SafeCard now also needs to be unlocked.
* Various UI improvements such as a new cancel button added to the pairing secret screen.
* Optional BIP39 password (a.k.a. "25th word") is now allowed when a wallet is generated.
* ABI updates: ABI_V2 (released in mainnet Dec '20) now supported and tuple types (increasingly utilized by more modern smart contracts) are also now supported.
* Uniswap router V2 ABI definitions now pre-loaded.

**Bug fixes:**
* Invalid characters (non-ASCII) now represented with a single square character (????)). Previously these characters were either skipped or shown as screen artifacts.
* Cleanup of invalid `strncat` usage in various places.
* Fixed derivation of public keys from non-standard paths (i.e. for unknown coin types).
* Added a missing zero-initialization of buffer.

**Changelog:**

* #1863: Add max PIN length boundary to screen builder (ebd1c94ef1e04aa488b3ea05e776cf8451f96b72)
* #1861: Fix issue with unlocking the card on insert (0dc251bd73a3ff6a24d9680544bf3d464dac70d0)
* #1869: Check EVM1 at boot/login and prompt user to unlock SafeCard if one is inserted (daefde01ebd2a380a4cb61249153502e928ac2b7)
* #1874: Increase number of pairing slots and add pagination to the pairings list screen (b1e2aa9f4f28a00946a0d1f64254874aa1d557ed)
* #1877: Move compile guards to fix bug (6a2905823d69ac538ecb221023a99f39f1549f81)
* #1875: Add length boundaries to PIN screens (aa3199150681a5879e002306b9dc5771234c8cb0)
* #1876: Fixes bugs related to forgetting a wifi network (9446bef511f3aaf87309ae98e0bc0486297e8880)
* #1878: Fixes bug in resetting wifi credentials after a router reset (6e79e58b4992580c0a842865ad5c006b07bf61bc)
* #1879: Fixes syntax errors (4f4f82a77e1100edd9c78c2a5f832e89e5c5d4fd)
* #1880: Adds cancel button to the pairing screen to improve UX (bdfc035b956efa5324ae54018d03bb7c14f07471)
* #1881: Adds missing mechanism to exit clone card process (e6932bf925157dbfb589adfd0320c99d12f846e4)
* #1893: Adds compile guards to settings UI (126261d3722c3ff9397d36079da1133ef828cd8f)
* #1896: Fixes derivation of public keys from non-standard paths (daf93e5a619897b0023b2d9c8a03a7eda0020d00)
* #1887: Updates lvgl to print square character when unknown character is encountered (5c06bd29dc4d07f481458b579897076b486802d9)
* #1897: Fixes lvgl update from previous PR (4b220851289f918974b2a136bfee9ef8108e435f)
* #1882: Fixes several erroneous strcat instances (1877b5dc8f440266302eb44685b5f1bcd2d8a6fb)
* #1900: Fixes lvgl update again (db1a3d927baa2ddf835497d5974ee3cdcc66afc4)
* #1895: Updates language on a few screens (91859fd1e409a70d614af38df8df73b62dc77911)
* #1901: Add tuple support to ABI decoding/printing (f5eadf9662b5e6961e3bbee031e4dd5387a5bdb6)
* #1910: Fixes off-by-one bug introduced in previous PR (9c95b3b631c0d5089707b5144c2c5b9005599dfd)
* #1914: Zero-inits pairing secret buffer to avoid screen artifacts (56c4cc99ab4a2ae989d57ce48971d40ce490fd77)
* #1911: Adds BIP password option to generated wallets (b06f3db9db7d1a31ed03fef440ce2cab181e7557)
* #1904: Fixes UI hooks in reset Lattice setting (443ae86c7b9a70366608e7dd0a3fdd8d0b14d306)
* #1912: Bump firmware to v0.10.3 (d6bd538b01ea878030c454dd4cf432cbcde225ed)
* #1916: Add compile guards to wallet UI (ca449668e5933bb7d693e85d81a6c60002e668cf)
* #1917: Adds Uniswap router V2 ABI definitions to preload list (863d6cd19fd4de651a1c8f24d8f57e57e62abc07)
* #1923: Add HSM and GCE versions to welcome screen for easier factory debugging (47183371ffe32964d62107589314351a8773a1a7)
* #1928: Add optimization to lvgl in order to build release (e95c8ace9afa21dbc0b1ed7a8c91640227771ed2)
* #1930: Decreases timer on reset router mechanism to better align with GCE reset timing (d9930ee5da0344eefeefbd4a41fbe5af0623825b)
* 239ab31c52041acc470859c2a449d3411241ca94: Removal of defunct unit tests
* #1931: Fix mistaken size calculation on screen displaying number of ABI defs loaded (2112031dfa2c52ce521cff4c5dbcbef85b97da26)

## v0.10.2

*Published Jan 20 2021*

This release contains a fix to prevent random tamper events in the field related to an overly sensitive tamper mesh. 

**Changelog:**

* #1870: Revision to tamper mesh config to prevent invalid triggering in v2 hardware revs.

## v0.10.1

*Published Jan 13 2021*

This release includes a fix relative to v0.10.0.

**Changelog:**

* #1860: Adds missing call to clear staged ABI definitions at ABI filesystem init. This functionally means that users had to approve preloaded definitions before, but no longer need to.

## v0.10.0

*Published Jan 12 2021*

This is a minor release which contains features, minor UI tweaks, and bug fixes.

**Larger Features:**

* Human readable Ethereum transactions via smart contract ABI definition loading and parsing
* Permissions v0: time and spending limits on simple ETH and BTC transfers

**Changelog:**

* #1738: Cleans up various build configs.
* #1739: Removes the staging config added previously based on our observation that it would not be useful.
* #1740: Fix associated build flags in some unit tests in light of the changes to the `DEBUG` compile flag.
* #1741: Fix typo in `CMakeLists.txt`.
* #1749: lvgl `#define` renames
* #1751: Update lvgl submodule
* #1754: System hook to subscribe to card manager events
* #1759: Reset sleep timer on card insertion/removal
* #1764: Import `cb0r` lib
* #1772: Add cbor print util
* #1781: Add bignum printing to cbor print util
* #1787: Allows for display of larger Ethereum `value` amounts -- this supports display of `value`s up to `UINT256_MAX`
* #1813: Permissions v0
* #1817: Wake device on signing request
* #1821: Compile flag for clone card feature
* #1822: Compile flag for update manager
* #1823: Language update to tamper screen
* #1824: Ethereum transaction data markdown using ABIs
* #1825: Compile flag for permissions v0
* #1827: Bug fixes and corrections to permissions
* #1828: Allow paired requesters to fetch addresses outside of the device's cache
* #1830: Fixes incorrect buffer size
* #1831: Adds additional warning text when card has one PIN attempt remaining
* #1833: Updates to submodules
* #1836: One more submodule update to add more keyboard characters to lvgl's special keyboard
* #1837: Bump version to v0.10.0 and updates several constants
* #1838: Fix test runner and add missing compile flag
* #1839: Blocks permissioned auto-signing of Ethereum transactions with non-zero data payloads
* #1842: Fix bug in `system_card_insertion_hook`
* #1843: Fix bug in permissions that was preventing permissioned requesters from making ETH_MSG requests
* #1846: Comment out erroneous screen drawing function for the time being. Will be fixed at a later time.
* #1849: Updates Ethereum ABI parser module to handle edge case
* #1850: Updates Ethereum ABI parser module once more to remove erroneous sanity check from previous commit

## v0.9.8

*Published Dec 4 2020*

* #1763: Add option to display `extraText` left-justified (defaults to the legacy center alignment) and also fixes bug in calculating the size of the pages when line breaks are introduced.
* #1770: Allows our Ethereum transaction parser to parse large `chainID` values, which were previously encoded as `uint8_t`s. Sidechains, testnets, and various other networks will use larger values for the `chainID`, which this PR enables.
* #1775: Fixes race condition in `wallet_task` that occurs during the card cloning process
* #1783: Fixes a nasty bug where the device times out and blanks the screen while the user is writing down their seed phrase. We should have had a block on that screen, which has been added in this PR.
* #1786: Adds more visual cues to the different lock screens (card vs system) to give users more clarity. We have had a few confused users who haven't been able to differentiate between the screens.
* #1795: Displays GCE SSH login credentials on the device info screen, even for production devices. Almost all of our user bugs/issues have arisen on the GCE side and not being able to debug those could lead to massive support issues. We have thus elected to display the information a user needs to SSH into the device on the device info screen. This also has the benefit of enticing developers to "hack" around with their device.
* #1796: Various language updates throughout the UI.
* #1798: Fixes bug introduced by me in #1795 ???? 
* #1799: More language updates to the UI
* #1800: Fixes an oversight from #1770. We were not skipping over the chainID in the `data` field in the transaction display screen and this PR fixes that.

## v0.9.6

*Published Sep 30 2020*

* Our first version ????
* Core Lattice firmware functionality
