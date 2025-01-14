# Mattermost changelog

[Mattermost](https://mattermost.com) is an open source platform for secure collaboration across the entire software development lifecycle. 

```{Important}
From Mattermost v9.2, this changelog summarizes updates for the latest cloud and self-hosted versions of Mattermost to be [deployed and upgraded on infrastructure you control](https://docs.mattermost.com/guides/deployment.html).

- See the [Important Upgrade Notes](https://docs.mattermost.com/upgrade/important-upgrade-notes.html) documentation for details on upgrading.
- See the [changelog in progress](https://bit.ly/2nK3cVf) for details about the upcoming release.
- **Self-Hosted Releases Prior to v9.2**: See the [Mattermost Legacy Self-Hosted Changelog](https://docs.mattermost.com/deploy/legacy-self-hosted-changelog.html) for details.
- **Cloud Releases Prior to v9.2**: See the [Mattermost Legacy Cloud Changelog](https://docs.mattermost.com/deploy/legacy-cloud-changelog.html) for details.
```

## Release v9.8 - [Feature Release](https://docs.mattermost.com/upgrade/release-definitions.html#feature-release)

**Release day: 2024-05-16**

### Compatibility
 - Updated minimum required Edge and Chrome versions to 122+.

```{Important}
If you upgrade from a release earlier than v9.7, please read the other [Important Upgrade Notes](https://docs.mattermost.com/upgrade/important-upgrade-notes.html).
```

### Improvements

See [this walkthrough video](https://mattermost.com/video/mattermost-v9-8-changelog/) on some of the improvements in our latest release below.

#### User Interface (UI)
 - Pre-packaged Playbooks version [v1.39.3](https://github.com/mattermost/mattermost-plugin-playbooks/releases/tag/v1.39.3).
 - Pre-packaged GitLab plugin version [v1.8.1](https://github.com/mattermost/mattermost-plugin-gitlab/releases/tag/v1.8.1).
 - Pre-packaged Calls version [v0.26.2](https://github.com/mattermost/mattermost-plugin-calls/releases/tag/v0.26.2).
 - Combined Desktop and Mobile notifications in the user settings modal.
 - Added a **Don't Clear** option for Do Not Disturb.
 - Enhanced the user interface for channel introductions.
 - Added an ephemeral message for non-team member mentions in channels.
 - Added emoji tooltips on hover in post message.
 - Made the appearance of several tooltips more consistent.
 - Updated theme colors for onboarding tour points.
 - Updated the right-hand side Thread view to use relative timestamps to be more consistent with the global Threads view.
 - Added a total reply count to the right-hand side thread view.
 - Added Channel Bookmarks (disabled by default).

#### Administration
 - Added safety limit error message in compiled Team Edition and Enterprise Edition deployments when enterprise scale and access control automation features are unavailable, and message count exceeds 5 million posts. ERROR_SAFE_LIMITS_EXCEEDED.
 - Downloading a support packet is now extensible with plugins. If a plugin can add content to the support packet, it will be displayed in the commercial support modal. Administrators will have the option to include/exclude that from the support package.
 - Upgraded Nodejs to v20.11.
 - Added Channel Bookmarks permissions to the channel user role and to the channel moderation system.
 - Added progress logs for attachments in bulk exports.
 - Added a **System Console** option to rebuild Elasticsearch channels indexes.
 - Obfuscated ``ReplicaLagSettings`` in the Support Packet.
 - Improved license loading errors.
 - Updated the keycloak docker configs and added a ``make`` command.
 - Removed unused ``IsOAuth`` field from ``AppError``.
 - ``bool`` is now used for ``license_is_trial`` in the Support Packet.
 - Bulk export: added functionality to export roles and permissions schemes.
 - A new flag (``extract-content``) was added to the mmctl import process that allows the server to skip content extraction during the import phase.

### API Changes
 - Added a create channel bookmark endpoint at ``/api/v4/channels/{channel_id}/bookmarks``.
 - Added additional query params to channel endpoints to include channel bookmarks.
 - Added update channel bookmark endpoint at ``/api/v4/channels/{channel_id}/bookmarks/{bookmark_id}``.
 - Added list channel bookmarks endpoint at ``/api/v4/channels/{channel_id}/bookmarks``.
 - Added delete channel bookmark endpoint at ``/api/v4/channels/{channel_id}/bookmarks/{bookmark_id}``.
 - Added update channel bookmark sort order endpoint at ``/api/v4/channels/{channel_id}/bookmarks/{bookmark_id}/sort_order``.
 - Exposed a local-mode only API for reattaching plugins, primarily to facilitate mock-free unit testing.
 - Exposed ``UpdateUserRoles`` in ``pluginapi``.
 - Exposed ``pluginapi.ProfileImageBytes`` to simplify bot setup from a plugin.
 - For ``POST /channels``, added a validation for ``display_name`` to not pass validation if the display name is empty.

### Bug Fixes
 - Fixed an issue with context cancellation for integration requests.
 - Fixed an issue preventing the retrieval of SAML metadata.
 - Fixed an issue causing an empty channel switcher after converting a group message to a private channel.
 - Fixed an issue where System Admins were not allowed to LDAP sync SAML users when ``SamlSettings.EnableSyncWithLdap`` was set to **true**.
 - Fixed an issue with markdown in the AD job status table.
 - Fixed an issue with a control character in the group list modal.
 - Fixed an issue where the auto-complete channels API returned archived channels in response.
 - Fixed a crash issue in the **System Console**.
 - Fixed an issue where links included in notifications were truncated and not clickable.
 - Fixed using local requests instead of HTTP requests in the flow library.
 - Fixed an issue where ``support_packet.yaml`` wasn’t generated even if an error occurred.
 - Fixed an issue where outgoing webhooks did not trigger when using multiple callback URLs.
 - Fixed an issue where it was not possible to clear plugin settings with a default value in the **System Console**.
 - Fixed an issue where ``MaxUsersForStatistics`` wasn’t ignored when generating a Support Packet.
 - Fixed an issue where the ``EnsureBot`` function did not recreate the bot if it had been manually deleted.
 - Fixed an issue where users couldn't look up a user by their ID in the **System Console** anymore.
 - Fixed an accessibility issue where the focus didn’t go back to the originating button when a modal was closed.
 - Fixed an issue where end users were not allowed to fetch the group members list of groups that allow ``@-mentions``.

### config.json
New setting option were added to ``config.json``. Below is a list of the additions and their default values on install. The settings can be modified in ``config.json``, or the System Console when available.

#### Changes to all plans:
 - Under ``FileSettings`` in ``config.json``:
   - Added ``AmazonS3UploadPartSizeBytes`` and ``ExportAmazonS3UploadPartSizeBytes`` to control the part size used to upload files to an S3 store.
 - Under ``ServiceSettings`` in ``config.json``:
   - Increased the default payload size limit (``MaximumPayloadSizeBytes``) from 100 kB to 300 kB.
 - Under ``ClusterSettings`` in ``config.json``:
   - Removed unused settings ``StreamingPort``, ``MaxIdleConns``, ``MaxIdleConnsPerHost`` and ``IdleConnTimeoutMilliseconds``.

 #### Changes to Professional and Enterprise plans:
 - Under ``ExperimentalSettings`` in ``config.json``:
   - Removed the ``UseNewSAMLLibrary`` experimental setting.

### Go Version
 - v9.8 is built with Go ``v1.21.8``.

### Known Issues
 - Status may sometimes get stuck as **Away** or **Offline** with IP Hash turned off.
 - Searching stop words in quotation marks with Elasticsearch enabled returns more than just the searched terms.
 - Slack import through the CLI fails if email notifications are enabled.
 - Push notifications don't always clear on iOS when running Mattermost in High Availability mode.
 - The Playbooks left-hand sidebar doesn't update when a user is added to a run or playbook without a refresh.
 - If a user isn't a member of a configured broadcast channel, posting a status update might fail without any error feedback. As a temporary workaround, join the configured broadcast channels, or remove those channels from the run configuration.
 
### Contributors
 - [agarciamontoro](https://github.com/agarciamontoro), [agnivade](https://github.com/agnivade), [Amir-Helali](https://github.com/Amir-Helali), [amyblais](https://github.com/amyblais), [andrleite](https://github.com/andrleite), [angeloskyratzakos](https://github.com/angeloskyratzakos), [annaos](https://github.com/annaos), [apshada](https://github.com/apshada), [Aryakoste](https://github.com/Aryakoste), [asaadmahmood](https://github.com/asaadmahmood), [aszakacs](https://github.com/aszakacs), [BarbUk](https://github.com/BarbUk), [BenCookie95](https://github.com/BenCookie95), [Blaieet](https://github.com/Blaieet), [calebroseland](https://github.com/calebroseland), [coltoneshaw](https://github.com/coltoneshaw), [cpoile](https://github.com/cpoile), [crspeller](https://github.com/crspeller), [ctlaltdieliet](https://translate.mattermost.com/user/ctlaltdieliet), [cwarnermm](https://github.com/cwarnermm), [cyrusjc](https://github.com/cyrusjc), [daran9](https://github.com/daran9), [devharipragaz007](https://github.com/devharipragaz007), [devinbinnie](https://github.com/devinbinnie), [dsspence](https://github.com/dsspence), [Eleferen](https://translate.mattermost.com/user/Eleferen), [EltonGohJH](https://github.com/EltonGohJH), [emdecr](https://github.com/emdecr), [enahum](https://github.com/enahum), [ezekielchow](https://github.com/ezekielchow), [fmartingr](https://github.com/fmartingr), [gabrieljackson](https://github.com/gabrieljackson), [gitairman](https://github.com/gitairman), [grundleborg](https://github.com/grundleborg), [hanzei](https://github.com/hanzei), [harshilsharma63](https://github.com/harshilsharma63), [hmhealey](https://github.com/hmhealey), [hossain-sazzad](https://github.com/hossain-sazzad), [ifoukarakis](https://github.com/ifoukarakis), [inconnu1](https://github.com/inconnu1), [isacikgoz](https://github.com/isacikgoz), [jasonblais](https://github.com/jasonblais), [jespino](https://github.com/jespino), [johnsonbrothers](https://github.com/johnsonbrothers), [jones](https://translate.mattermost.com/user/jones), [josephjose](https://github.com/josephjose), [jprusch](https://github.com/jprusch), [JulienTant](https://github.com/JulienTant), [jupenur](https://github.com/jupenur), [jwilander](https://github.com/jwilander), [kaakaa](https://github.com/kaakaa), [kaoski](https://github.com/kaoski), [Karimaljandali](https://github.com/Karimaljandali), [kayazeren](https://github.com/kayazeren), [KrisSiegel](https://github.com/KrisSiegel), [Kshitij-Katiyar](https://github.com/Kshitij-Katiyar), [larkox](https://github.com/larkox), [lbr88](https://github.com/lbr88), [lieut-data](https://github.com/lieut-data), [lindalumitchell](https://github.com/lindalumitchell), [lynn915](https://github.com/lynn915), [M-ZubairAhmed](https://github.com/M-ZubairAhmed), [mahdiirar](https://github.com/mahdiirar), [majo](https://translate.mattermost.com/user/majo), [manojmalik20](https://github.com/manojmalik20), [master7](https://translate.mattermost.com/user/master7), [matt-w99](https://github.com/matt-w99), [matthew-w](https://translate.mattermost.com/user/matthew-w), [matthewbirtch](https://github.com/matthewbirtch), [MeHow25](https://github.com/MeHow25), [mentz](https://translate.mattermost.com/user/mentz), [mgdelacroix](https://github.com/mgdelacroix), [mickmister](https://github.com/mickmister), [milotype](https://github.com/milotype), [movion](https://github.com/movion), [mvitale1989](https://github.com/mvitale1989), [nickmisasi](https://github.com/nickmisasi), [Nityanand13](https://github.com/Nityanand13), [nmnj](https://translate.mattermost.com/user/nmnj), [Obbi89](https://github.com/Obbi89), [pacop](https://github.com/pacop), [phoinixgrr](https://github.com/phoinixgrr), [Pkarle](https://github.com/Pkarle), [poppfredslund](https://translate.mattermost.com/user/poppfredslund), [potatogim](https://github.com/potatogim), [raghavaggarwal2308](https://github.com/raghavaggarwal2308), [rahimrahman](https://github.com/rahimrahman), [rOt779kVceSgL](https://translate.mattermost.com/user/rOt779kVceSgL), [RS-labhub](https://github.com/RS-labhub), [Rutam21](https://github.com/Rutam21), [s-krishnaraju](https://github.com/s-krishnaraju), [saturninoabril](https://github.com/saturninoabril), [sbishel](https://github.com/sbishel), [Sharuru](https://github.com/Sharuru), [sri-byte](https://github.com/sri-byte), [stafot](https://github.com/stafot), [streamer45](https://github.com/streamer45), [stylianosrigas](https://github.com/stylianosrigas), [Syed-Ali-Abbas-Zaidi](https://github.com/Syed-Ali-Abbas-Zaidi), [tanmaythole](https://github.com/tanmaythole), [ThrRip](https://github.com/ThrRip), [tnir](https://github.com/tnir), [toninis](https://github.com/toninis), [topolovac](https://github.com/topolovac), [varghesejose2020](https://github.com/varghesejose2020), [wetneb](https://github.com/wetneb), [wiersgallak](https://github.com/wiersgallak), [wiggin77](https://github.com/wiggin77), [yasserfaraazkhan](https://github.com/yasserfaraazkhan), [yomiadetutu1](https://github.com/yomiadetutu1), [zsrv](https://github.com/zsrv)

## Release v9.7 - [Feature Release](https://docs.mattermost.com/upgrade/release-definitions.html#feature-release)

- **9.7.4, released 2024-05-15**
  - Fixed an issue with context cancellation for integration requests [MM-58019](https://mattermost.atlassian.net/browse/MM-58019).
  - Fixed some plugin settings with defaults not changing value [MM-58102](https://mattermost.atlassian.net/browse/MM-58102).
  - Mattermost v9.7.4 contains no database or functional changes.
- **9.7.3, released 2024-04-30**
  - Fixed an issue where creating a Direct Message channel with synthetic users failed [MM-58019](https://mattermost.atlassian.net/browse/MM-58019).
  - Mattermost v9.7.3 contains no database or functional changes.
- **9.7.2, released 2024-04-25**
  - Mattermost v9.7.2 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.7.2 contains no database or functional changes.
  - Pre-packaged Playbooks version [v1.39.3](https://github.com/mattermost/mattermost-plugin-playbooks/releases/tag/v1.39.3).
  - Increased the default payload size limit (``MaximumPayloadSizeBytes``) from 100 kB to 300 kB.
  - Fixed an issue where it was not possible to clear the plugin settings with a default value in the System Console.
- **9.7.1, released 2024-04-16**
  - Fixed an issue with a noisy log entry for permalink post notifications.
  - Mattermost v9.7.1 contains no database or functional changes.
- **9.7.0, released 2024-04-16**
  - Original 9.7.0 release.

```{Important}
If you upgrade from a release earlier than v9.6, please read the other [Important Upgrade Notes](https://docs.mattermost.com/upgrade/important-upgrade-notes.html).
```

### Improvements

See [this walkthrough video](https://mattermost.com/video/mattermost-v9-7-changelog/) on some of the improvements in our latest release below.

#### User Interface (UI)
 - Added Mattermost [AI plugin](https://github.com/mattermost/mattermost-plugin-ai) to pre-packaged plugins.
 - Pre-packaged Calls version [v0.25.1](https://github.com/mattermost/mattermost-plugin-calls/releases/tag/v0.25.1).
 - Pre-packaged Playbooks version [v1.39.2](https://github.com/mattermost/mattermost-plugin-playbooks/releases/tag/v1.39.2).
 - Pre-packaged GitHub plugin version [v2.2.0](https://github.com/mattermost/mattermost-plugin-github/releases/tag/v2.2.0).
 - The first emoji is now auto-selected in the emoji picker.
 - Added Markdown support for batched email notifications.
 - Users’ timezone is now used in batched email notifications.
 - Removed a conflicting class (``help-text``) from the interactive dialog field description to resolve the black text color in the dark theme.
 - Updated the user interface of **Team Settings** modal.
 - Promoted Simplified Chinese to Beta, and downgraded Hungarian and Spanish languages to Beta.
 - Improved the opening animation of the user settings modal.

#### Administration
 - Upgraded ``@mattermost/client`` and ``@mattermost/types`` to support TypeScript v5.x.
 - Enforced safety limit in compiled Team Edition and Enterprise Edition deployments when enterprise scale and access control automation features are unavailable, and when the count of users who are registered, but not deactivated, exceeds 11,000. ERROR_SAFE_LIMITS_EXCEEDED.
 - Dropped pre-packaged plugins for unsupported OS and architectures.
 - Implemented a new **Export Settings** page in the **System Console** to allow Cloud administrators to customize their dedicated export S3 buckets.
 - LDAP job details are no longer shown until the job runs.
 - Added more logging to the ``NotificationsLog``.
 - A message is now logged when a user tries to log in using an incorrect password.
 - Posts from deactivated users are now included in **Direct Message** channel exports. Also the ``--include-archived-channels`` flag is now respected for **Direct Message** channels.
 - Changed the cache headers for file endpoints to cache privately for 24 hours, instead of not caching at all.
 - Improved the performance of the ElasticSearch indexing job in PostgreSQL installations.
 - Moved following functions from server to public utils:
    - ``ResetReadTimeout``
    - ``AppendMultipleStatementsFlag``
    - ``SetupConnection``
    - ``SanitizeDataSource``

#### mmctl
 - mmctl can now be used to download a Support Packet using ``--local mode``.
 - mmctl system ping will now return detailed server status even if the server status is unhealthy.

### Bug Fixes
 - Fixed an issue where the Desktop App login flow would erroneously show the landing page for first time users.
 - Fixed an issue where a right-hand side card was not reloaded when the card body was updated.
 - Fixed an issue where ``en-AU`` language selection was not allowed.
 - Fixed an issue with the position of text in the default profile picture.
 - Fixed an issue with the group search to parse the display name.
 - Fixed an issue where items with longer text did not widen the user guide dropdown to its max-width.
 - Fixed an issue where the configuration could not be updated from the **System Console** in cloud environments.

### config.json
A new setting option was added to ``config.json``. Below is a list of the addition and its default value on install. The setting can be modified in ``config.json``, or the System Console when available.

#### Changes to all plans:
 - Under ``CloudSettings`` in ``config.json``:
   - Added a new configuration setting ``Disable`` (via config.json, or environment variable), default ``false``. When set to ``true``, all requests to the Mattermost Customer Portal from a workspace will be disabled.

### Open Source Components
 - Added ``stylelint`` to https://github.com/mattermost/mattermost/.

### Go Version
 - v9.7 is built with Go ``v1.20.7``.

### Known Issues
 - Status may sometimes get stuck as **Away** or **Offline** in High Availability mode with IP Hash turned off.
 - Searching stop words in quotation marks with Elasticsearch enabled returns more than just the searched terms.
 - Slack import through the CLI fails if email notifications are enabled.
 - Push notifications don't always clear on iOS when running Mattermost in High Availability mode.
 - The Playbooks left-hand sidebar doesn't update when a user is added to a run or playbook without a refresh.
 - If a user isn't a member of a configured broadcast channel, posting a status update might fail without any error feedback. As a temporary workaround, join the configured broadcast channels, or remove those channels from the run configuration.
 
### Contributors
 - [2017Yasu](https://github.com/2017Yasu), [agarciamontoro](https://github.com/agarciamontoro), [agnivade](https://github.com/agnivade), [amyblais](https://github.com/amyblais), [andriumm](https://github.com/andriumm), [angeloskyratzakos](https://github.com/angeloskyratzakos), [annaos](https://github.com/annaos), [apshada](https://github.com/apshada), [asaadmahmood](https://github.com/asaadmahmood), [ayusht2810](https://github.com/ayusht2810), [azigler](https://github.com/azigler), [BenCookie95](https://github.com/BenCookie95), [Blaieet](https://github.com/Blaieet), [calebroseland](https://github.com/calebroseland), [cpoile](https://github.com/cpoile), [crspeller](https://github.com/crspeller), [ctlaltdieliet](https://translate.mattermost.com/user/ctlaltdieliet), [cwarnermm](https://github.com/cwarnermm), [devinbinnie](https://github.com/devinbinnie), [dipaksinha1](https://github.com/dipaksinha1), [doc-sheet](https://github.com/doc-sheet), [Eleferen](https://translate.mattermost.com/user/Eleferen),  [emdecr](https://github.com/emdecr), [enahum](https://github.com/enahum), [ezekielchow](https://github.com/ezekielchow), [gabrieljackson](https://github.com/gabrieljackson), [grundleborg](https://github.com/grundleborg), [hannaparks](https://github.com/hannaparks), [hanzei](https://github.com/hanzei), [harshilsharma63](https://github.com/harshilsharma63), [hereje](https://github.com/hereje), [hmhealey](https://github.com/hmhealey), [hossain-sazzad](https://github.com/hossain-sazzad), [ifoukarakis](https://github.com/ifoukarakis), [isacikgoz](https://github.com/isacikgoz), [iyampaul](https://github.com/iyampaul), [jasonblais](https://github.com/jasonblais), [jespino](https://github.com/jespino), [johndavidlugtu](https://github.com/johndavidlugtu), [johnsonbrothers](https://github.com/johnsonbrothers), [jones](https://translate.mattermost.com/user/jones), [jprusch](https://github.com/jprusch), [JulienTant](https://github.com/JulienTant), [jwilander](https://github.com/jwilander), [kaakaa](https://github.com/kaakaa), [kayazeren](https://github.com/kayazeren), [Kshitij-Katiyar](https://github.com/Kshitij-Katiyar), [larkox](https://github.com/larkox), [lieut-data](https://github.com/lieut-data), [lindalumitchell](https://github.com/lindalumitchell), [Linkinlog](https://github.com/Linkinlog), [lynn915](https://github.com/lynn915), [M-ZubairAhmed](https://github.com/M-ZubairAhmed), [majo](https://translate.mattermost.com/user/majo), [marianunez](https://github.com/marianunez), [master7](https://translate.mattermost.com/user/master7), [matthew-w](https://translate.mattermost.com/user/matthew-w), [matthewbirtch](https://github.com/matthewbirtch), [mickmister](https://github.com/mickmister), [morgancz](https://github.com/morgancz), [mozi47](https://github.com/mozi47), [mvitale1989](https://github.com/mvitale1989), [nab-77](https://github.com/nab-77), [nachtjasmin](https://github.com/nachtjasmin), [natalie-hub](https://github.com/natalie-hub), [neflyte](https://github.com/neflyte), [nickmisasi](https://github.com/nickmisasi), [phoinixgrr](https://github.com/phoinixgrr), [poppfredslund](https://translate.mattermost.com/user/poppfredslund), [pvev](https://github.com/pvev), [raghavaggarwal2308](https://github.com/raghavaggarwal2308), [RyoKub](https://github.com/RyoKub), [saturninoabril](https://github.com/saturninoabril), [sbishel](https://github.com/sbishel), [Sharuru](https://translate.mattermost.com/user/Sharuru), [sinansonmez](https://github.com/sinansonmez), [sri-byte](https://github.com/sri-byte), [stafot](https://github.com/stafot), [streamer45](https://github.com/streamer45), [stylianosrigas](https://github.com/stylianosrigas), [Syed-Ali-Abbas-Zaidi](https://github.com/Syed-Ali-Abbas-Zaidi), [tanmaythole](https://github.com/tanmaythole), [ThrRip](https://github.com/ThrRip), [toninis](https://github.com/toninis), [varghesejose2020](https://github.com/varghesejose2020), [vish9812](https://github.com/vish9812), [vishal-rathod-07](https://github.com/vishal-rathod-07), [wiersgallak](https://github.com/wiersgallak), [wiggin77](https://github.com/wiggin77), [Wing0515](https://github.com/Wing0515), [yasserfaraazkhan](https://github.com/yasserfaraazkhan), [yomiadetutu1](https://github.com/yomiadetutu1)

## Release v9.6 - [Feature Release](https://docs.mattermost.com/upgrade/release-definitions.html#feature-release)

- **9.6.2, released 2024-04-25**
  - Mattermost v9.6.2 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.6.2 contains no database or functional changes.
  - Pre-packaged Playbooks version [v1.39.3](https://github.com/mattermost/mattermost-plugin-playbooks/releases/tag/v1.39.3).
  - Fixed an issue where it was not possible to clear the plugin settings with a default value in the System Console.
- **9.6.1, released 2024-03-26**
  - Mattermost v9.6.1 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.6.1 contains no database or functional changes.
  - Fixed an issue where the configuration could not be updated from the System Console in cloud environments.
- **9.6.0, released 2024-03-15**
  - Original 9.6.0 release.

### Compatibility
 - Updated minimum required Edge and Chrome versions to 120+.

```{Important}
If you upgrade from a release earlier than v9.5, please read the other [Important Upgrade Notes](https://docs.mattermost.com/upgrade/important-upgrade-notes.html).
```

### Improvements

See [this walkthrough video](https://mattermost.com/video/changelog-v9-6/) on some of the improvements in our latest release below.

#### User Interface (UI)
 - Pre-packaged Calls version [v0.24.0](https://github.com/mattermost/mattermost-plugin-calls/releases/tag/v0.24.0).
 - Pre-packaged GitLab plugin version [v1.8.0](https://github.com/mattermost/mattermost-plugin-gitlab/releases/tag/v1.8.0).
 - Added the [Outgoing OAuth Connections](https://mattermost.com/pl/outgoing-oauth-connections) integration type.
 - Re-designed the **System Console > User Management** screen, and added the ability to batch export users in CSV format (Professional and Enterprise plans). On MySQL, users cannot view live results of the batch export in the user interface.
 - Improved the appearance of profile/account menus.
 - Added support for checkbox types in the **System Console** settings.
 - Added support for WebP image previews in the web app similar to PNG and other image formats.
 - Several pre-packaged plugins were removed.

#### Administration
 - Removed some unused Redux actions and reducers, including ``state.entities.posts.selectedPostId``.
 - Limited the number of user preference updates to 10 per call.
 - Clarified that the LDAP profile picture setting is optional.

#### mmctl
 - Extended mmctl with support for user preferences.

### Bug Fixes
 - Fixed an issue with switching to a **Direct Message** channel with a shared channel user (user from another server).
 - Fixed an issue with extra space getting added to code blocks in search results.
 - Fixed an issue where deactivated members were not included in a favorited **Direct Message** channel export.
 - Fixed an issue where password strength settings wouldn't be disabled if they were set through environment variables.
 - Fixed an issue where post mentions would grow outside the viewport on small devices.
 - Fixed an issue with draft removal after deleting the post.
 - Fixed a markdown issue where, on some occasions, extra space was found before a list.
 - Fixed an issue where a sender to a custom group would also receive the message notification themselves.
 - Fixed a web app crash when a System Admin clicked on a link to a private channel that they were not a member of.
 - Fixed ``ChannelHasBeenCreated`` plugin hook not being called when a group channel was created.
 - Fixed thread notifications so that if a user had **Thread Reply Notifications** disabled for your account and **Automatically follow threads in this channel** enabled for a channel, the user wouldn't receive thread notifications for that channel per global setting.

### config.json
 - Multiple setting options were added to ``config.json``. Below is a list of the additions and their default values on install. The settings can be modified in ``config.json``, or the System Console when available.

#### Changes to the Enterprise plan:
 - Under ``ServiceSettings`` in ``config.json``:
    - Added ``EnableOutgoingOAuthConnections`` configuration setting for Outgoing OAuth Connections integration type.

### Open Source Components
 - Added ``@floating-ui/react``, and removed ``@floating-ui/react-dom`` and ``@floating-ui/react-dom-interactions`` from https://github.com/mattermost/mattermost/.

### Go Version
 - v9.6 is built with Go ``v1.20.7``.

### Known Issues
 - Users' initial status is not always loaded correctly [MM-56966](https://mattermost.atlassian.net/browse/MM-56966).
 - Status may sometimes get stuck as **Away** or **Offline** in High Availability mode with IP Hash turned off.
 - Searching stop words in quotation marks with Elasticsearch enabled returns more than just the searched terms.
 - Slack import through the CLI fails if email notifications are enabled.
 - Push notifications don't always clear on iOS when running Mattermost in High Availability mode.
 - The Playbooks left-hand sidebar doesn't update when a user is added to a run or playbook without a refresh.
 - If a user isn't a member of a configured broadcast channel, posting a status update might fail without any error feedback. As a temporary workaround, join the configured broadcast channels, or remove those channels from the run configuration.
 
### Contributors
 - [abdesslamhouioui](https://github.com/abdesslamhouioui), [agarciamontoro](https://github.com/agarciamontoro), [agnivade](https://github.com/agnivade), [Alpha-4](https://github.com/Alpha-4), [amyblais](https://github.com/amyblais), [andrleite](https://github.com/andrleite), [angeloskyratzakos](https://github.com/angeloskyratzakos), [apshada](https://github.com/apshada), [arush-vashishtha](https://github.com/arush-vashishtha), [asaadmahmood](https://github.com/asaadmahmood), [avas27JTG](https://github.com/avas27JTG), [ayusht2810](https://github.com/ayusht2810), [azigler](https://github.com/azigler), [BenCookie95](https://github.com/BenCookie95), [bewing](https://github.com/bewing), [calebroseland](https://github.com/calebroseland), [carydrew](https://github.com/carydrew), [Chlbek](https://translate.mattermost.com/user/Chlbek), [compiledsound](https://github.com/compiledsound), [cpatulea](https://github.com/cpatulea), [cpoile](https://github.com/cpoile), [crspeller](https://github.com/crspeller), [ctlaltdieliet](https://github.com/ctlaltdieliet), [cwarnermm](https://github.com/cwarnermm), [devinbinnie](https://github.com/devinbinnie), [DHaussermann](https://github.com/DHaussermann), [edu-ap](https://github.com/edu-ap), [Eleferen](https://translate.mattermost.com/user/Eleferen), [emdecr](https://github.com/emdecr), [enahum](https://github.com/enahum), [esarafianou](https://github.com/esarafianou), [ewwollesen](https://github.com/ewwollesen), [gabrieljackson](https://github.com/gabrieljackson), [gourav-varma](https://github.com/gourav-varma), [Gregesp](https://github.com/Gregesp), [grundleborg](https://github.com/grundleborg), [hannaparks](https://github.com/hannaparks), [hanzei](https://github.com/hanzei), [harshilsharma63](https://github.com/harshilsharma63), [hereje](https://github.com/hereje), [hmhealey](https://github.com/hmhealey), [iabdousd](https://github.com/iabdousd), [ifoukarakis](https://github.com/ifoukarakis), [isacikgoz](https://github.com/isacikgoz), [it33](https://github.com/it33), [jespino](https://github.com/jespino), [jlandells](https://github.com/jlandells), [johndavidlugtu](https://github.com/johndavidlugtu), [jones](https://translate.mattermost.com/user/jones), [jprusch](https://github.com/jprusch), [JulienTant](https://github.com/JulienTant), [juliovillalvazo](https://github.com/juliovillalvazo), [jwilander](https://github.com/jwilander), [kaakaa](https://github.com/kaakaa), [larkox](https://github.com/larkox), [lieut-data](https://github.com/lieut-data), [lucassabreu](https://github.com/lucassabreu), [lynn915](https://github.com/lynn915), [M-ZubairAhmed](https://github.com/M-ZubairAhmed), [majo](https://translate.mattermost.com/user/majo), [marianunez](https://github.com/marianunez), [master7](https://translate.mattermost.com/user/master7), [matt-w99](https://github.com/matt-w99), [matthew-w](https://translate.mattermost.com/user/matthew-w), [matthewbirtch](https://github.com/matthewbirtch), [mickmister](https://github.com/mickmister), [milotype](https://github.com/milotype), [MixeroTN](https://translate.mattermost.com/user/MixeroTN), [mjnagel](https://github.com/mjnagel), [morgancz](https://translate.mattermost.com/user/morgancz), [mvitale1989](https://github.com/mvitale1989), [nickmisasi](https://github.com/nickmisasi), [nokedajunky](https://github.com/nokedajunky), [olavinto](https://github.com/olavinto), [oOoBenoitoOo](https://github.com/oOoBenoitoOo), [phoinixgrr](https://github.com/phoinixgrr), [pvev](https://github.com/pvev), [raghavaggarwal2308](https://github.com/raghavaggarwal2308), [rOt779kVceSgL](https://translate.mattermost.com/user/rOt779kVceSgL), [sadohert](https://github.com/sadohert), [saturninoabril](https://github.com/saturninoabril), [sbishel](https://github.com/sbishel), [Sharuru](https://translate.mattermost.com/user/Sharuru), [sinansonmez](https://github.com/sinansonmez), [sohzm](https://github.com/sohzm), [sri-byte](https://github.com/sri-byte), [stafot](https://github.com/stafot), [streamer45](https://github.com/streamer45), [stylianosrigas](https://github.com/stylianosrigas), [svelle](https://github.com/svelle), [Syed-Ali-Abbas-Zaidi](https://github.com/Syed-Ali-Abbas-Zaidi), [TealWater](https://github.com/TealWater), [ThrRip](https://github.com/ThrRip), [titanventura](https://github.com/titanventura), [toninis](https://github.com/toninis), [trangology](https://github.com/trangology), [trivikr](https://github.com/trivikr), [tsabi](https://github.com/tsabi), [Utsav-Ladani](https://github.com/Utsav-Ladani), [varghesejose2020](https://github.com/varghesejose2020), [vidhisaini10](https://github.com/vidhisaini10), [wiggin77](https://github.com/wiggin77), [yasserfaraazkhan](https://github.com/yasserfaraazkhan), [yeoji](https://github.com/yeoji)

## Release v9.5 - [Extended Support Release](https://docs.mattermost.com/upgrade/release-definitions.html#extended-support-release-esr)

- **9.5.5, released 2024-05-15**
  - Fixed an issue where the user status would incorrectly get stuck to online after the user closed their tab [MM-57885](https://mattermost.atlassian.net/browse/MM-57885).
  - Mattermost v9.5.5 contains no database or functional changes.
- **9.5.4, released 2024-04-25**
  - Mattermost v9.5.4 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.5.4 contains no database or functional changes.
  - Pre-packaged Playbooks version [v1.39.3](https://github.com/mattermost/mattermost-plugin-playbooks/releases/tag/v1.39.3).
  - Increased the default payload size limit (``MaximumPayloadSizeBytes``) from 100 kB to 300 kB.
  - Fixed an issue with context cancellation for integration requests.
  - Fixed an issue where end users were not allowed to fetch the group members list of groups that allow ``@-mentions``.
- **9.5.3, released 2024-03-26**
  - Mattermost v9.5.3 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.5.3 contains no database or functional changes.
  - Improved the performance of the ElasticSearch indexing job in PostgreSQL installations.
- **9.5.2, released 2024-03-06**
  - Mattermost v9.5.2 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.5.2 contains no database or functional changes.
  - Fixed ``ChannelHasBeenCreated`` plugin hook not being called when a group channel was created.
- **9.5.1, released 2024-02-16**
  - Mattermost v9.5.1 contains low to high severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.5.1 contains no database or functional changes.
- **9.5.0, released 2024-02-16**
  - Original 9.5.0 release.

### Important Upgrade Notes
 - We have stopped supporting MySQL v5.7 since it's at the end of life. We urge customers to upgrade their MySQL instance at their earliest convenience.

```{Important}
If you upgrade from a release earlier than v9.4, please read the other [Important Upgrade Notes](https://docs.mattermost.com/upgrade/important-upgrade-notes.html).
```

### Improvements

See [this walkthrough video](https://www.youtube.com/watch?v=b1M2BGGF578&feature=youtu.be) on some of the improvements in our latest release below.

#### User Interface (UI)
 - Pre-packaged Calls version [v0.23.1](https://github.com/mattermost/mattermost-plugin-calls/releases/tag/v0.23.1).
 - Pre-packaged Jira plugin version [v4.1.0](https://github.com/mattermost/mattermost-plugin-jira/releases/tag/v4.1.0).
 - Improved the behavior of suggestion boxes when changing the caret position.
 - Changed the time for tomorrow in the **Do Not Disturb** timer and post reminder to refer to the next day at 9:00am instead of 24hrs from the time of activation.
 - Updated message timestamp tooltips to include seconds.
 - Added a new Wrangler feature to be able to move threads (Experimental). Moving threads requires a Professional/Enterprise license to activate. This feature is not yet recommended for production use. A new feature flag ``MoveThreadsEnabled`` was added and is default OFF. Changing this value to ON will enable the experimental **Move Threads** feature.
 - Applied a wording change for active and activated users in the **System Console** user list.
 - Applied a wording change for active and activated users in the **Team Statistics** page.

#### Administration
 - Added safety limit error message in compiled Team Edition and Enterprise Edition deployments when enterprise scale and access control automation features are unavailable and count of users who are registered and not deactivated exceeds 10,000. ERROR_SAFE_LIMITS_EXCEEDED.
 - The ``where`` field is now rendered in ``model.AppError`` only when it's present.
 - Added Outgoing Oauth implementation ``Get``/``List`` logic.
 - The mmctl bulk import process command in local mode now supports processing an import file without actually uploading it to the server. Simply pass the file path to the import file and the server will directly read from it, and pass the ``--bypass-upload`` flag. There is no need to use the import upload command. NOTE: all of this is applicable only in local mode.
 - Added **Monthly Active Users** (MAU) as part of the true-up report.
 - Prometheus metrics are now available under the Source Available License.

#### Performance
 - Optimized ``createPost`` performance.
 - Improved the performance of emoji uploads.
 - Made small optimizations in several database calls:
     - ``App.HasPermissionToChannel``
     - ``getPostsForChannelAroundLastUnread``
     - ``publishWebsocketEventForPermalinkPost``
     - ``countMentionsFromPost``

#### Plugins
 - Plugins are now allowed to register user settings.
 - Plugins can now register an action in the **User Settings** section. Plugins can also now disable a section in their **User Settings**.
 - Included session id in request payload of the ``WebSocketMessageHasBeenPosted`` plugin hook.

### Bug Fixes
 - Fixed an issue where the right-hand side stopped getting the focus when navigating from **Global Threads** or **Global Drafts**.
 - Fixed a theme issue in the notification settings.
 - Fixed a regression in compliance exports which did not allow the export job to be canceled gracefully on server shutdown.
 - Fixed an error where posts dismissed by a plugin were not properly removed from the view.
 - Fixed an issue where if there were multiple websocket connections from a single user, then in case one connection got removed during a broadcast, there was a possibility that the other good connection would not get the event.
 - Fixed an issue with true-up reports sending active users and not activated users.
 - Fixed an issue where users were not able to navigate through links to private channels they are member of with certain configurations.

### config.json
 - Multiple setting options were added to ``config.json``. Below is a list of the additions and their default values on install. The settings can be modified in ``config.json``, or the System Console when available.

#### Changes to all plans:
 - Under ``ServiceSettings`` in ``config.json``:
     - Added ``MaximumPayloadSizeBytes`` to add a limit to the payload size of API endpoints passing in arrays.
 - Added a configuration setting ``OutgoingIntegrationRequestsDefaultTimeout`` for integration requests.

#### Changes to the Professional and Enterprise plans:
 - Under ``WranglerSettings`` in ``config.json``:
    - Added ``AllowedEmailDomain`` - a CSV list of strings, where each is an email domain that is allowed to use the feature (e.g. - on community.mattermost.com, ``mattermost.com`` would allow staff to move a thread, while non-staff cannot).
    - ``MoveThreadMaxCount`` - a number representing the maximum number of posts that can be in a thread for it to be moveable.
    - ``MoveThreadToAnotherTeamEnable`` - a boolean value representing whether moving should work across teams.
    - ``MoveThreadFromPrivateChannelEnable`` - a boolean value representing whether moving should work from within a private channel.
    - ``MoveThreadFromDirectMessageChannelEnable`` - a boolean value representing whether moving should be allowed from within a group message.

#### Changes to the Enterprise plan:
 - Under ``DataRetentionSettings`` in ``config.json``:
    - Added two new configuration settings, ``MessageRetentionHours`` and ``FileRetentionHours``, in order to support setting your global retention time in hours. ``DataRetentionSettings.MessageRetentionDays`` and ``DataRetentionSettings.FileRetentionDays`` are deprecated but we will continue to use their value until you set something for their hours equivalent. If Days are set then the hours configuration must be 0 and if hours is set then the days config must be 0. We do not support hours for granular retention policies. Due to how our Elasticsearch indexes are stored, Data retention will now also remove elastic search indexes equal to the day of the retention cut-off time.

### API Changes
 - Added a new API endpoint ``POST /api/v4/posts/<post ID>/move``.
 - Added ``UpdateChannelMembersNotifications`` plugin API.
 - Added plugin APIs and hooks for accessing the **Shared Channels** service via plugins.
 - Added a limit to the payload size of API endpoints passing in arrays.
 - Added ``PreferencesHaveChanged`` plugin hook.
 - Added ``GetPreferenceForUser`` plugin API.
 - Added a new API endpoint ``GET /api/v4/users/report`` for system admin user reporting.
 - Added a new API endpoint ``GET /api/v4/reports/users/count``.

### Open Source Components
 - Added ``@tanstack/react-table`` and ``prometheus/client_model`` to https://github.com/mattermost/mattermost/.

### Go Version
 - v9.5 is built with Go ``v1.20.7``.

### Known Issues
 - User autocomplete no longer stays closed after pressing ESC key [MM-56748](https://mattermost.atlassian.net/browse/MM-56748).
 - Status may sometimes get stuck as **Away** or **Offline** in High Availability mode with IP Hash turned off.
 - Searching stop words in quotation marks with Elasticsearch enabled returns more than just the searched terms.
 - Slack import through the CLI fails if email notifications are enabled.
 - Push notifications don't always clear on iOS when running Mattermost in High Availability mode.
 - The Playbooks left-hand sidebar doesn't update when a user is added to a run or playbook without a refresh.
 - If a user isn't a member of a configured broadcast channel, posting a status update might fail without any error feedback. As a temporary workaround, join the configured broadcast channels, or remove those channels from the run configuration.
 - The Playbooks left-hand sidebar does not update when a user is added to a run or playbook without a refresh.
 
### Contributors
 - [agarciamontoro](https://github.com/agarciamontoro), [agnivade](https://github.com/agnivade), [akbarkz](https://translate.mattermost.com/user/akbarkz), [amyblais](https://github.com/amyblais), [andriuspetrauskis](https://github.com/andriuspetrauskis), [andriuspre](https://github.com/andriuspre), [angeloskyratzakos](https://github.com/angeloskyratzakos), [asaadmahmood](https://github.com/asaadmahmood), [ayusht2810](https://github.com/ayusht2810), [azigler](https://github.com/azigler), [azistellar](https://translate.mattermost.com/user/azistellar), [azizthegit](https://github.com/azizthegit), [bbodenmiller](https://github.com/bbodenmiller), [BenCookie95](https://github.com/BenCookie95), [c0d33ngr](https://github.com/c0d33ngr), [catenacyber](https://github.com/catenacyber), [cedricongjh](https://github.com/cedricongjh), [Chlbek](https://translate.mattermost.com/user/Chlbek), [chriswachira](https://github.com/chriswachira), [coltoneshaw](https://github.com/coltoneshaw), [cpoile](https://github.com/cpoile), [cripton](https://github.com/cripton), [crspeller](https://github.com/crspeller), [ctlaltdieliet](https://github.com/ctlaltdieliet), [cwarnermm](https://github.com/cwarnermm), [cyberjam](https://github.com/cyberjam), [devinbinnie](https://github.com/devinbinnie), [duttakapil](https://github.com/duttakapil), [Eleferen](https://translate.mattermost.com/user/Eleferen), [enahum](https://github.com/enahum), [GabrielCasaro](https://github.com/GabrielCasaro), [gabrieljackson](https://github.com/gabrieljackson), [grundleborg](https://github.com/grundleborg), [hanzei](https://github.com/hanzei), [harshilsharma63](https://github.com/harshilsharma63), [heisdinesh](https://github.com/heisdinesh), [hmhealey](https://github.com/hmhealey), [hynex](https://translate.mattermost.com/user/hynex), [ifoukarakis](https://github.com/ifoukarakis), [isacikgoz](https://github.com/isacikgoz), [jespino](https://github.com/jespino), [johndavidlugtu](https://github.com/johndavidlugtu), [johnsonbrothers](https://github.com/johnsonbrothers), [jones](https://translate.mattermost.com/user/jones), [jprusch](https://github.com/jprusch), [JulienTant](https://github.com/JulienTant), [kaakaa](https://github.com/kaakaa), [kerochelo](https://github.com/kerochelo), [Kshitij-Katiyar](https://github.com/Kshitij-Katiyar), [larkox](https://github.com/larkox), [lieut-data](https://github.com/lieut-data), [lindalumitchell](https://github.com/lindalumitchell), [M-ZubairAhmed](https://github.com/M-ZubairAhmed), [majo](https://translate.mattermost.com/user/majo), [marianunez](https://github.com/marianunez), [master7](https://translate.mattermost.com/user/master7), [matoro](https://github.com/matoro), [matt-w99](https://github.com/matt-w99), [matthew-w](https://translate.mattermost.com/user/matthew-w), [mgdelacroix](https://github.com/mgdelacroix), [mickmister](https://github.com/mickmister), [milotype](https://translate.mattermost.com/user/milotype), [mkaraki](https://github.com/mkaraki), [mvitale1989](https://github.com/mvitale1989), [nickmisasi](https://github.com/nickmisasi), [Nityanand13](https://github.com/Nityanand13), [norma596](https://translate.mattermost.com/user/norma596), [Omar8345](https://github.com/Omar8345), [phoinixgrr](https://github.com/phoinixgrr), [raghavaggarwal2308](https://github.com/raghavaggarwal2308), [Rutam21](https://github.com/Rutam21), [RyoKub](https://github.com/RyoKub), [sapnasivakumar](https://github.com/sapnasivakumar), [saturninoabril](https://github.com/saturninoabril), [sbishel](https://github.com/sbishel), [ShrootBuck](https://github.com/ShrootBuck), [SkyDusH](https://translate.mattermost.com/user/SkyDusH), [sonichigo](https://github.com/sonichigo), [sri-byte](https://github.com/sri-byte), [stafot](https://github.com/stafot), [streamer45](https://github.com/streamer45), [stylianosrigas](https://github.com/stylianosrigas), [Sudhanva-Nadiger](https://github.com/Sudhanva-Nadiger), [Syed-Ali-Abbas-Zaidi](https://github.com/Syed-Ali-Abbas-Zaidi), [TealWater](https://github.com/TealWater), [thinkGeist](https://github.com/thinkGeist), [thomasbrq](https://github.com/thomasbrq), [ThrRip](https://github.com/ThrRip), [titanventura](https://github.com/titanventura), [toninis](https://github.com/toninis), [trangology](https://github.com/trangology), [tsabi](https://translate.mattermost.com/user/tsabi), [Utsav-Ladani](https://github.com/Utsav-Ladani), [varghesejose2020](https://github.com/varghesejose2020), [vish9812](https://github.com/vish9812), [VishalB98](https://github.com/VishalB98), [wiggin77](https://github.com/wiggin77), [Willy-Wakam](https://github.com/Willy-Wakam), [yasserfaraazkhan](https://github.com/yasserfaraazkhan), [yaz](https://translate.mattermost.com/user/yaz), [yomiadetutu1](https://github.com/yomiadetutu1)

## Release v9.4 - [Feature Release](https://docs.mattermost.com/upgrade/release-definitions.html#feature-release)

- **9.4.5, released 2024-03-26**
  - Mattermost v9.4.5 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.4.5 contains no database or functional changes.
- **9.4.4, released 2024-03-06**
  - Mattermost v9.4.4 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.4.4 contains no database or functional changes.
- **9.4.3, released 2024-02-14**
  - Mattermost v9.4.3 contains low to high severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.4.3 contains no database or functional changes.
  - Pre-packaged Jira plugin version [v4.1.0](https://github.com/mattermost/mattermost-plugin-jira/releases/tag/v4.1.0).
- **9.4.2, released 2024-01-30**
  - Mattermost v9.4.2 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Fixed an issue with true-up reports sending active users and not activated users. Added **Monthly Active Users** (MAU) as part of the true-up reports.
  - Mattermost v9.4.2 contains no database or functional changes.
- **9.4.1, released 2024-01-16**
  - Fixed an issue where ``getChannelMemberOnly`` failed to fetch data when certain fields were NULL.
- **9.4.0, released 2024-01-16**
  - Original 9.4.0 release.

### Important Upgrade Notes
 - MySQL v5.7 is at end of life. We recommend all customers to upgrade to at least 8.x. For now, we are logging a warning. From Mattermost v9.5, which is the next Extended Support Release, we will stop supporting MySQL v5.7 altogether.

```{Important}
If you upgrade from a release earlier than v9.3, please read the other [Important Upgrade Notes](https://docs.mattermost.com/upgrade/important-upgrade-notes.html).
```

### Compatibility
 - Updated the minimum required Edge version to v118+.

### Improvements

See [this walkthrough video](https://www.youtube.com/watch?v=bEMp4vYLi6c&feature=youtu.be&ab_channel=Mattermost) on some of the improvements in our latest release below.

#### User Interface (UI)
 - Updated the pre-packaged GitHub plugin version to [v2.1.7](https://github.com/mattermost/mattermost-plugin-github/releases/tag/v2.1.7).
 - Pre-packaged Calls plugin version [v0.22.2](https://github.com/mattermost/mattermost-plugin-calls/releases/tag/v0.22.2).
 - Improved the user interface of the channel notifications modal.
 - Emojis are now enlarged in emoji tooltips on mouse hover.
 - Added a gap of 8px between buttons in the modal footer when opened in the mobile web view.
 - Updated empty states to align with new branding and made changes to the empty state copy.
 - Adjusted the position of the suggestion list in "Add <user> to a channel" modal to be below or above the text field.

#### Administration
 - Added support for IP Filtering in Cloud (Cloud Enterprise plan) (this feature is disabled by default and behind a feature flag).
 - Added support for Bring Your Own Key (BYOK) Encryption (Cloud Enterprise plan).
 - An optional dedicated filestore is now used for compliance exports if configured (Cloud Enterprise plan).
 - ``MessageExportSettings.GlobalRelaySettings.CustomerType`` now supports "CUSTOM".
 - Added new ``ServerMetrics`` hook to allow plugins to register a custom HTTP endpoint to serve their metrics under the server's metrics HTTP listener.
 - Admins now have the ability to pipe the output of ``mmctl websocket`` into the JSON parser.
 - Added stores for OAuth **Outgoing Connections**.
 - Added last login timestamp for users, and added a materialized view and a refresh job to keep track of post stats for PostgreSQL.
 - Allowed plugin requests to include **Authorization** headers from external systems.
 - Added a mmctl command ``mmctl system supportpacket`` to download the **Support Packet**.
 - Added a new mmctl command ``oauth list`` for listing registered OAuth2 applications.

### Bug Fixes
 - Fixed an issue with the emoji reaction toggle behavior.
 - Fixed an issue with the spacing between Playbooks and the separator in the Apps bar.

### config.json
 - Multiple setting options were added to ``config.json``. Below is a list of the additions and their default values on install. The settings can be modified in ``config.json``, or the System Console when available.

#### Changes to all plans:
 - Under ``RefreshPostStatsRunTime`` in ``config.json``:
    - Added ``RefreshPostStatsRunTime`` to add last login timestamp for users and to add materialized view and refresh job to keep track of post stats for PostgreSQL.
  
#### Changes to the Enterprise plan:
 - Under ``GlobalRelayMessageExportSettings`` in ``config.json``:
    - Added two new configuration settings ``CustomSMTPServerName`` and ``CustomSMTPPort`` to allow setting a custom URL and port for Global Relay export. This enables compliance export to integrate with Proofpoint.

### Open Source Components:
 - Added ``@mattermost/desktop-api`` and ``ipaddr.js`` to https://github.com/mattermost/mattermost/.

### Go Version
 - v9.4 is built with Go ``v1.20.7``.

### Known Issues
 - Non-channel-admin users can no longer use message links in private channels [MM-56575](https://mattermost.atlassian.net/browse/MM-56575).
 - Preview doesn't work when editing a channel header [MM-56572](https://mattermost.atlassian.net/browse/MM-56572).
 - The channel member count shows as zero in the **Browse channels** modal [MM-56266](https://mattermost.atlassian.net/browse/MM-56266).
 - Adding an @mention at the start of a post draft and pressing the left or right arrow key can clear the post draft and the undo history [MM-33823](https://mattermost.atlassian.net/browse/MM-33823).
 - Status may sometimes get stuck as **Away** or **Offline** in High Availability mode with IP Hash turned off.
 - Searching stop words in quotation marks with Elasticsearch enabled returns more than just the searched terms.
 - Slack import through the CLI fails if email notifications are enabled.
 - Push notifications don't always clear on iOS when running Mattermost in High Availability mode.
 - The Playbooks left-hand sidebar doesn't update when a user is added to a run or playbook without a refresh.
 - If a user isn't a member of a configured broadcast channel, posting a status update might fail without any error feedback. As a temporary workaround, join the configured broadcast channels, or remove those channels from the run configuration.
 - The Playbooks left-hand sidebar does not update when a user is added to a run or playbook without a refresh.
 
### Contributors
 - [AayushChaudhary0001](https://github.com/AayushChaudhary0001), [aditipatelpro](https://github.com/aditipatelpro), [agarciamontoro](https://github.com/agarciamontoro), [agnivade](https://github.com/agnivade), [akbarkz](https://github.com/akbarkz), [Alpha-4](https://github.com/Alpha-4), [amyblais](https://github.com/amyblais), [andrius](https://translate.mattermost.com/user/andrius), [andriuspetrauskis](https://github.com/andriuspetrauskis), [andrleite](https://github.com/andrleite), [arthurhrg](https://github.com/arthurhrg), [arush-vashishtha](https://github.com/arush-vashishtha), [asaadmahmood](https://github.com/asaadmahmood), [avas27JTG](https://github.com/avas27JTG), [ayusht2810](https://github.com/ayusht2810), [azigler](https://github.com/azigler), [BenCookie95](https://github.com/BenCookie95), [caotanduc99](https://github.com/caotanduc99), [CI-YU](https://github.com/CI-YU), [codejagaban](https://github.com/codejagaban), [cpoile](https://github.com/cpoile), [crspeller](https://github.com/crspeller), [ctlaltdieliet](https://github.com/ctlaltdieliet), [cwarnermm](https://github.com/cwarnermm), [cyberjam](https://github.com/cyberjam), [danielsischy](https://github.com/danielsischy), [Dev-A-Line](https://translate.mattermost.com/user/Dev-A-Line), [devinbinnie](https://github.com/devinbinnie), [DHaussermann](https://github.com/DHaussermann), [dkkb](https://github.com/dkkb), [Eleferen](https://translate.mattermost.com/user/Eleferen), [enahum](https://github.com/enahum), [fmartingr](https://github.com/fmartingr), [FokinAleksandr](https://github.com/FokinAleksandr), [GabrielCasaro](https://github.com/GabrielCasaro), [gabrieljackson](https://github.com/gabrieljackson), [gabsfrancis](https://translate.mattermost.com/user/gabsfrancis), [grundleborg](https://github.com/grundleborg), [hanzei](https://github.com/hanzei), [harsh4723](https://github.com/harsh4723), [harshilsharma63](https://github.com/harshilsharma63), [hasancankucuk](https://github.com/hasancankucuk), [hereje](https://github.com/hereje), [hmhealey](https://github.com/hmhealey), [ifoukarakis](https://github.com/ifoukarakis), [isacikgoz](https://github.com/isacikgoz), [jasonblais](https://github.com/jasonblais), [jespino](https://github.com/jespino), [johnsonbrothers](https://github.com/johnsonbrothers), [jprusch](https://translate.mattermost.com/user/jprusch), [jwilander](https://github.com/jwilander), [kaakaa](https://github.com/kaakaa), [Kshitij-Katiyar](https://github.com/Kshitij-Katiyar), [larkox](https://github.com/larkox), [lieut-data](https://github.com/lieut-data), [lindalumitchell](https://github.com/lindalumitchell), [ludvigbolin](https://github.com/ludvigbolin), [lynn915](https://github.com/lynn915), [M-ZubairAhmed](https://github.com/M-ZubairAhmed), [majo](https://translate.mattermost.com/user/majo), [master7](https://translate.mattermost.com/user/master7), [matt-w99](https://github.com/matt-w99), [matthew-w](https://translate.mattermost.com/user/matthew-w), [matthewbirtch](https://github.com/matthewbirtch), [mgdelacroix](https://github.com/mgdelacroix), [mickmister](https://github.com/mickmister), [morgancz](https://github.com/morgancz), [mvitale1989](https://github.com/mvitale1989), [neflyte](https://github.com/neflyte), [nickmisasi](https://github.com/nickmisasi), [Paul-Stern](https://github.com/Paul-Stern), [pgteekens](https://translate.mattermost.com/user/pgteekens), [phoinixgrr](https://github.com/phoinixgrr), [PromoFaux](https://github.com/PromoFaux), [PulkitGarg-code](https://github.com/PulkitGarg-code), [raghavaggarwal2308](https://github.com/raghavaggarwal2308), [rajatdangat](https://github.com/rajatdangat), [relwell](https://github.com/relwell), [roaslin](https://github.com/roaslin), [rohan-kapse](https://github.com/rohan-kapse), [rohitkbc](https://github.com/rohitkbc), [Rutam21](https://github.com/Rutam21), [RyoKub](https://github.com/RyoKub), [saakshiraut28](https://github.com/saakshiraut28), [San4es](https://github.com/San4es), [sapnasivakumar](https://github.com/sapnasivakumar), [saturninoabril](https://github.com/saturninoabril), [sbishel](https://github.com/sbishel), [Sharuru](https://github.com/Sharuru), [ShlokJswl](https://github.com/ShlokJswl), [sinansonmez](https://github.com/sinansonmez), [srappan](https://github.com/srappan), [sri-byte](https://github.com/sri-byte), [srisri332](https://github.com/srisri332), [stafot](https://github.com/stafot), [streamer45](https://github.com/streamer45), [stylianosrigas](https://github.com/stylianosrigas), [Sudhanva-Nadiger](https://github.com/Sudhanva-Nadiger), [svelle](https://github.com/svelle), [Syed-Ali-Abbas-Zaidi](https://github.com/Syed-Ali-Abbas-Zaidi), [tanmaythole](https://github.com/tanmaythole), [TealWater](https://github.com/TealWater), [thomasbrq](https://github.com/thomasbrq), [ThrRip](https://github.com/ThrRip), [toninis](https://github.com/toninis), [tsabi](https://github.com/tsabi), [umrkhn](https://github.com/umrkhn), [varghesejose2020](https://github.com/varghesejose2020), [Vinecreeper888](https://github.com/Vinecreeper888), [weblate](https://github.com/weblate), [wiggin77](https://github.com/wiggin77), [yasserfaraazkhan](https://github.com/yasserfaraazkhan), [yomiadetutu1](https://github.com/yomiadetutu1), [ZubairImtiaz3](https://github.com/ZubairImtiaz3)

## Release v9.3 - [Feature Release](https://docs.mattermost.com/upgrade/release-definitions.html#feature-release)

- **9.3.3, released 2024-03-06**
  - Mattermost v9.3.3 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.3.3 contains no database or functional changes.
- **9.3.2, released 2024-02-14**
  - Mattermost v9.3.2 contains low to high severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.3.2 contains no database or functional changes.
  - Pre-packaged Jira plugin version [v4.1.0](https://github.com/mattermost/mattermost-plugin-jira/releases/tag/v4.1.0).
- **9.3.1, released 2024-01-30**
  - Mattermost v9.3.1 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.3.1 contains no database or functional changes.
- **9.3.0, released 2023-12-15**
  - Original 9.3.0 release.

### Important Upgrade Notes
 - Please read the [Important Upgrade Notes](https://docs.mattermost.com/upgrade/important-upgrade-notes.html) before upgrading.

### Compatibility
 - Updated minimum required Firefox version to v115+.
 - Updated minimum supported Chromium version to 118+.

### Improvements

See [this walkthrough video](https://www.youtube.com/watch?v=eXA8emM97Bo) on some of the improvements in our latest release below.

#### User Interface (UI)
 - Updated pre-packaged Playbooks plugin version to [v1.39.1](https://github.com/mattermost/mattermost-plugin-playbooks/releases/tag/v1.39.1).
 - Updated pre-packaged Calls version to [v0.21.1](https://github.com/mattermost/mattermost-plugin-calls/releases/tag/v0.21.1).
 - Updated pre-packaged Jira plugin version to [v4.0.1](https://github.com/mattermost/mattermost-plugin-jira/releases/tag/v4.0.1). Also see [v4.0.0](https://github.com/mattermost/mattermost-plugin-jira/releases/tag/v4.0.0) for recent breaking changes.
 - Added Vietnamese (Beta) as a new language.
 - Added the ability to passively track keywords with highlights without triggering a notification (Professional and Enterprise plans).
 - Updated the **Settings** modal with an improved user interface.
 - Added a new **Jump to recents** banner when a channel is scrolled up.
 - Modified the behavior of the code button (Ctrl+Alt+C) to create inline codes or code blocks.
 - Disabled markdown keybindings within code blocks.
 - Added a **Back** button to the ``/access_problem`` page.
 - Added a default limit of the number of reactions per post.

#### Performance
 - Removed pre-fetch preference and set new prefetch limits for the webapp.
 - Improved websocket event marshaling performance.
 - Batched loading of recently used emojis on initial load.

#### Administration
 - The tooltip on the announcement bar in the **System Console** is now widened.
 - Improved the error message when trying to activate a plugin in an unsupported environment.
 - Added a file storage permission check to the workspace health dashboard.
 - Performed a cleanup in preparation for adding support for multi-word keywords that trigger notification.
 - Added a warning log message when the app runs as root.
 - Removed all uses of the ``ExperimentalTimezone`` setting. The Timezone feature is now always enabled and no longer behind a configuration setting.
 - Added support for previewing WebVTT attachments.
 - Introduced separate ``AdvancedLogging`` levels for LDAP messages.
 - Introduced trace logging level for LDAP messages.
 - Added a new way to modify ``WebSocket`` messages sent to individual connections.
 - Added a new server side hook ``MessagesWillBeConsumed`` to allow modifying post objects after they are grabbed from the database but before they are delivered to the client. This is behind a feature flag and disabled by default.
 - Users and posts are now pretty-printed in the logs.
 - Improved file extraction logging.
 - Exposed ``ThreadView`` and ``AdvancedCreateComment`` components in the webapp plugin exported components list.
 - Added **Logging > Advanced Logging** setting to the **System Console** to allow admins to configure custom log targets via the user interface.

### Bug Fixes
 - Fixed an issue where marking a Group Message as unread didn't show the badge count correctly.
 - Fixed an issue where ``invite_id`` was being reset on all team changes.
 - Fixed an issue where interactive dialog elements with subtype ``number`` didn't handle a ``0`` value properly.
 - Fixed an issue with the download link in channel file search items when including a path in the **Site URL** setting.
 - Fixed an issue with the formatting of special mentions in the right-hand side.
 - Fixed ``MessageWillBeUpdated`` plugin hook to allow rejections.
 - Fixed an issue with some shortcuts not working as expected.
 - Fixed the message history not clearing the input on the center channel.
 - Fixed an issue where a higher contrast was generated for some usernames.
 - Fixed an issue where newly created Group Messages showed having 0 members.
 - Fixed an issue where an incorrect timestamp was assigned to support packet files.
 - Fixed an issue where the **Reset Password** link was not displayed if only LDAP/AD was enabled.
 - Fixed an issue where **Recent Mentions** showed posts for other similar named users.
 - Fixed an error that appeared when updating the header of Group Messages.
 - Fixed an issue that caused the server to get stuck during shutdown due to a deadlock in a dependency.
 - Fixed an issue where Desktop App clients would be shown an error when trying to open file preview links.
 - Fixed an issue with double URL encoding of Oauth redirect URI params.
 - Fixed an issue where users couldn't at-mention custom groups in group constrained teams and channels.
 - Fixed an issue where the channel admin wasn't being set when converting a Group Message to a private channel.

### config.json
 - Multiple setting options were added to ``config.json``. Below is a list of the additions and their default values on install. The settings can be modified in ``config.json``, or the System Console when available.

#### Changes to all plans:
 - Removed ``DisplaySettings.ExperimentalTimezone`` setting.
 - Under ``ServiceSettings`` in ``config.json``:
    - Added ``DefaultUniqueReactionsPerPost`` and ``MaxUniqueReactionsPerPost`` to fix an issue where invalid reactions could be added to posts and to add a default limit for the number of reactions per post.

### API Changes
 - Added an API to batch requests for custom emojis on page load.

### Database Changes
 - ``NextSyncAt`` and ``Description`` columns are removed from the ``SharedChannelsRemotes`` table. Migration impact is considered to be minimal considering the possible table size.

### Go Version
 - v9.3 is built with Go ``v1.20.7``.

### Known Issues
 - Mattermost Omnibus: Unable to install omnibus due to unmet dependencies [MM-56080](https://mattermost.atlassian.net/browse/MM-56080).
 - Adding an @mention at the start of a post draft and pressing the left or right arrow key can clear the post draft and the undo history [MM-33823](https://mattermost.atlassian.net/browse/MM-33823).
 - Status may sometimes get stuck as **Away** or **Offline** in High Availability mode with IP Hash turned off.
 - Searching stop words in quotation marks with Elasticsearch enabled returns more than just the searched terms.
 - Slack import through the CLI fails if email notifications are enabled.
 - Push notifications don't always clear on iOS when running Mattermost in High Availability mode.
 - The Playbooks left-hand sidebar doesn't update when a user is added to a run or playbook without a refresh.
 - If a user isn't a member of a configured broadcast channel, posting a status update might fail without any error feedback. As a temporary workaround, join the configured broadcast channels, or remove those channels from the run configuration.
 - The Playbooks left-hand sidebar does not update when a user is added to a run or playbook without a refresh.

### Contributors
 - [agarciamontoro](https://github.com/agarciamontoro), [agnivade](https://github.com/agnivade), [AirGoatOne](https://github.com/AirGoatOne), [akbarkz](https://github.com/akbarkz), [amigo7kr](https://github.com/amigo7kr), [amyblais](https://github.com/amyblais), [anneschuth](https://github.com/anneschuth), [ARJ2160](https://github.com/ARJ2160), [Arslan-work](https://github.com/Arslan-work), [arthurh](https://translate.mattermost.com/user/arthurh), [arthurhrg](https://github.com/arthurhrg), [Aryakoste](https://github.com/Aryakoste), [asaadmahmood](https://github.com/asaadmahmood), [AshishDhama](https://github.com/AshishDhama), [avas27JTG](https://github.com/avas27JTG), [AvaterClasher](https://github.com/AvaterClasher), [ayusht2810](https://github.com/ayusht2810), [azigler](https://github.com/azigler), [BandhiyaHardik](https://github.com/BandhiyaHardik), [BenCookie95](https://github.com/BenCookie95), [Benjamin-Loison](https://github.com/Benjamin-Loison), [calebroseland](https://github.com/calebroseland), [catenacyber](https://github.com/catenacyber), [cedarice](https://github.com/cedarice), [CI-YU](https://github.com/CI-YU), [coltoneshaw](https://github.com/coltoneshaw), [cpoile](https://github.com/cpoile), [crspeller](https://github.com/crspeller), [ctlaltdieliet](https://github.com/ctlaltdieliet), [cwarnermm](https://github.com/cwarnermm), [Davut97](https://github.com/Davut97), [deepakumarvu](https://github.com/deepakumarvu), [devinbinnie](https://github.com/devinbinnie), [Dhoni77](https://github.com/Dhoni77), [DimitriDR](https://translate.mattermost.com/user/DimitriDR), [edwardnguyen225](https://github.com/edwardnguyen225), [Eleferen](https://translate.mattermost.com/user/Eleferen), [emdecr](https://github.com/emdecr), [Emil-Carlsson](https://github.com/Emil-Carlsson), [enahum](https://github.com/enahum), [escofresco](https://github.com/escofresco), [fandour](https://translate.mattermost.com/user/fandour), [fazil-syed](https://github.com/fazil-syed), [fmartingr](https://github.com/fmartingr), [gabrieljackson](https://github.com/gabrieljackson), [hanzei](https://github.com/hanzei), [harshal2030](https://github.com/harshal2030), [harshilsharma63](https://github.com/harshilsharma63), [heisdinesh](https://github.com/heisdinesh), [hmhealey](https://github.com/hmhealey), [ifoukarakis](https://github.com/ifoukarakis), [imamimam113](https://github.com/imamimam113), [imkrishnasarathi](https://github.com/imkrishnasarathi), [isacikgoz](https://github.com/isacikgoz), [jasonblais](https://github.com/jasonblais), [jespino](https://github.com/jespino), [johndavidlugtu](https://github.com/johndavidlugtu), [johnsonbrothers](https://github.com/johnsonbrothers), [jonathanwiemers](https://github.com/jonathanwiemers), [jprusch](https://github.com/jprusch), [JulienTant](https://github.com/JulienTant), [jwilander](https://github.com/jwilander), [kaakaa](https://github.com/kaakaa), [kapdev](https://translate.mattermost.com/user/kapdev), [kayazeren](https://github.com/kayazeren), [Kimbohlovette](https://github.com/Kimbohlovette), [Kshitij-Katiyar](https://github.com/Kshitij-Katiyar), [KuSh](https://github.com/KuSh), [kyeongsoosoo](https://github.com/kyeongsoosoo), [larkox](https://github.com/larkox), [LeonardJouve](https://github.com/LeonardJouve), [lieut-data](https://github.com/lieut-data), [lindy65](https://github.com/lindy65), [linkvn](https://github.com/linkvn), [ludvigbolin](https://github.com/ludvigbolin), [M-ZubairAhmed](https://github.com/M-ZubairAhmed), [m1lt0n](https://github.com/m1lt0n), [majo](https://translate.mattermost.com/user/majo), [manojmalik20](https://github.com/manojmalik20), [master7](https://translate.mattermost.com/user/master7), [matt-w99](https://github.com/matt-w99), [matthew-w](https://translate.mattermost.com/user/matthew-w), [matthewbirtch](https://github.com/matthewbirtch), [maxtrem271991](https://github.com/maxtrem271991), [mickmister](https://github.com/mickmister), [milotype](https://github.com/milotype), [mozi47](https://github.com/mozi47), [mvitale1989](https://github.com/mvitale1989), [nathanaelhoun](https://github.com/nathanaelhoun), [newdominic](https://github.com/newdominic), [nickmisasi](https://github.com/nickmisasi), [nosyn](https://github.com/nosyn), [otilor](https://github.com/otilor), [pacop](https://github.com/pacop), [Paul-Stern](https://github.com/Paul-Stern), [Paul-vrn](https://github.com/Paul-vrn), [phoinixgrr](https://github.com/phoinixgrr), [proggga](https://github.com/proggga), [pvev](https://github.com/pvev), [raghavaggarwal2308](https://github.com/raghavaggarwal2308), [rahulsuresh-git](https://github.com/rahulsuresh-git), [rashmibharambe](https://github.com/rashmibharambe), [Reene-Simon](https://github.com/Reene-Simon), [rohan-kapse](https://github.com/rohan-kapse), [rohitkbc](https://github.com/rohitkbc), [rubinaga](https://github.com/rubinaga), [RyoKub](https://github.com/RyoKub), [san70sh](https://github.com/san70sh), [sapnasivakumar](https://github.com/sapnasivakumar), [sbishel](https://github.com/sbishel), [seoyeongeun](https://github.com/seoyeongeun), [Sharuru](https://github.com/Sharuru), [shivamjosh](https://github.com/shivamjosh), [sinansonmez](https://github.com/sinansonmez), [Sn-Kinos](https://github.com/Sn-Kinos), [sp6370](https://github.com/sp6370), [sri-byte](https://github.com/sri-byte), [stafot](https://github.com/stafot), [streamer45](https://github.com/streamer45), [stylianosrigas](https://github.com/stylianosrigas), [Sudhanva-Nadiger](https://github.com/Sudhanva-Nadiger), [sudheer121](https://github.com/sudheer121), [Syed-Ali-Abbas-Zaidi](https://github.com/Syed-Ali-Abbas-Zaidi), [tanmaythole](https://github.com/tanmaythole), [tejas161](https://github.com/tejas161), [thomasbrq](https://github.com/thomasbrq), [ThrRip](https://github.com/ThrRip), [TomerPacific](https://github.com/TomerPacific), [toninis](https://github.com/toninis), [trivikr](https://github.com/trivikr), [tsabi](https://github.com/tsabi), [turretkeeper](https://github.com/turretkeeper), [umrkhn](https://github.com/umrkhn), [vish9812](https://github.com/vish9812), [wcdfilll](https://translate.mattermost.com/user/wcdfilll), [wiebel](https://github.com/wiebel), [wiggin77](https://github.com/wiggin77), [yasserfaraazkhan](https://github.com/yasserfaraazkhan), [yomiadetutu1](https://github.com/yomiadetutu1)

## Release v9.2 - [Feature Release](https://docs.mattermost.com/upgrade/release-definitions.html#feature-release)

- **9.2.6, released 2024-02-14**
    - Mattermost v9.2.6 contains low to high severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
    - Mattermost v9.2.6 contains no database or functional changes.
    - Pre-packaged Jira plugin version [v4.1.0](https://github.com/mattermost/mattermost-plugin-jira/releases/tag/v4.1.0).
- **9.2.5, released 2024-01-30**
    - Mattermost v9.2.5 contains low to medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
    - Mattermost v9.2.5 contains no database or functional changes.
- **9.2.4, released 2024-01-09**
  - Mattermost v9.2.4 contains medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.2.4 contains the following functional changes:
     - Fixed an issue where invalid reactions could be added to posts. Added default limit of the number of reactions per post.
- **9.2.3, released 2023-11-29**
  - Mattermost v9.2.3 contains medium severity level security fixes. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Mattermost v9.2.3 contains no database or functional changes.
  - Pre-packaged Calls plugin version [v0.21.1](https://github.com/mattermost/mattermost-plugin-calls/releases/tag/v0.21.1).
- **9.2.2, released 2023-11-08**
  - Mattermost v9.2.2 contains a high severity level security fix. [Upgrading](https://docs.mattermost.com/upgrade/upgrading-mattermost-server.html) to this release is recommended. Details will be posted on our [security updates page](https://mattermost.com/security-updates/) 30 days after release as per the [Mattermost Responsible Disclosure Policy](https://mattermost.com/security-vulnerability-report/).
  - Pre-packaged Playbooks plugin version [v1.39.1](https://github.com/mattermost/mattermost-plugin-playbooks/releases/tag/v1.39.1).
  - Fixed an issue where the **About Mattermost** dialog reported an incorrect server version.
- **9.2.1, released 2023-11-06**
  - Fixed an issue where Ubuntu GLIBC errors were thrown on Ubuntu 20.04 and Debian Bullseye versions.
- **9.2.0, released 2023-11-02**
  - Original 9.2.0 release

### Important Upgrade Notes
 - Fixed data retention policies to run jobs when any custom retention policy is enabled even when the global retention policy is set to **keep-forever**. Before this fix, the enabled custom data retention policies wouldn’t run as long as the global data retention policy was set to **keep-forever** or was disabled. After the fix, the custom data retention policies will run automatically even when the global data retention policy is set to **keep-forever**. Once the server is upgraded, posts may unintentionally be deleted. Admins should make sure to disable all custom data retention policies before upgrading, and then re-enable them again after upgrading.

```{Important}
If you upgrade from a release earlier than v9.1, please read the other [Important Upgrade Notes](https://docs.mattermost.com/upgrade/important-upgrade-notes.html).
```

### Compatibility
 - Updated minimum required Edge version to 116+.

### Improvements

See [this walkthrough video](https://www.youtube.com/watch?v=udC2OCTGooc&feature=youtu.be&ab_channel=Mattermost) on some of the improvements in our latest release below.

#### User Interface (UI)
 - Improved readability by displaying system messages on multiple lines when editing a channel header.
 - Combined "joined/left" event types in system messages.
 - Added a new user preference to disable webapp prefetching via **Settings > Advanced > Allow Mattermost to prefetch channel posts**. You must enable **Client Performance Debugging** in the System Console by going to **Environment > Developer** in order for this setting to appear. This setting and Client Performance Debugging should only be enabled temporarily if users are experiencing performance issues.
 - Pre-packaged NPS plugin version [v1.3.3](https://github.com/mattermost/mattermost-plugin-nps/releases/tag/v1.3.3).
 - Pre-packaged Todo plugin version [v0.7.1](https://github.com/mattermost/mattermost-plugin-todo/releases/tag/v0.7.1).

#### Administration
 - JSON null value cases are now handled correctly by also checking that the pointer is no longer null when unmarshalling to a pointer.
 - An annotated logger is now used to capture LDAP and SAML logs.
 - Replaced ``github.com/mattermost/gziphandler`` with ``github.com/klauspost/compress/gzhttp``.
 - Performance metrics now contain information on if a given request was sent during a page load or a websocket reconnect.
 - Elasticsearch aggregation jobs no longer start when a bulk indexing job is currently running.
 - Added heap profile, CPU profile, and goroutines profile to the support package.
 - Merged WIP i18n locales, but disallowed selecting unsupported locales.

### Bug Fixes
 - Fixed a panic where a simple worker would crash if it failed to get a job.
 - Fixed post props on update to properly see channel links.
 - Fixed an issue where the API for drafts would return empty drafts.
 - Fixed the alignment of the **Help** menu in the global header.
 - Fixed a broken link in the **Edit Channel** header modal.
 - Fixed an issue that prevented users to be added to channels from the System Console.
 - Fixed an issue where the channel member count increased when adding an already present user.
 - Fixed an issue where plugin developers were unable to create a ``textarea`` in interactive dialogs.
 - Fixed an issue where copy pasting images from Chrome failed.

### config.json
 - Multiple setting options were added to ``config.json``. Below is a list of the additions and their default values on install. The settings can be modified in ``config.json``, or the System Console when available.

#### Changes to all plans:
 - Under ``LogSettings`` in ``config.json``:
   - Added a new configuration setting ``MaxFieldSize`` to add the ability to size-limit log fields during logging.

### API Changes
 - Added ``origin_client`` to the ``mattermost_api_time`` metrics.

### Go Version
 - v9.2 is built with Go ``v1.20.7``.

### Known Issues
 - Adding an @mention at the start of a post draft and pressing the left or right arrow key can clear the post draft and the undo history [MM-33823](https://mattermost.atlassian.net/browse/MM-33823).
 - Status may sometimes get stuck as **Away** or **Offline** in High Availability mode with IP Hash turned off.
 - Searching stop words in quotation marks with Elasticsearch enabled returns more than just the searched terms.
 - Slack import through the CLI fails if email notifications are enabled.
 - Push notifications don't always clear on iOS when running Mattermost in High Availability mode.
 - The Playbooks left-hand sidebar doesn't update when a user is added to a run or playbook without a refresh.
 - If a user isn't a member of a configured broadcast channel, posting a status update might fail without any error feedback. As a temporary workaround, join the configured broadcast channels, or remove those channels from the run configuration.
 - The Playbooks left-hand sidebar does not update when a user is added to a run or playbook without a refresh.

### Contributors
 - [aayushborkar14](https://github.com/aayushborkar14), [AayushChaudhary0001](https://github.com/AayushChaudhary0001), [AbhineshJha](https://github.com/AbhineshJha), [agarciamontoro](https://github.com/agarciamontoro), [agnivade](https://github.com/agnivade), [akaMrDC](https://github.com/akaMrDC), [akbarkz](https://github.com/akbarkz), [alejdg](https://github.com/alejdg), [Alphanum404](https://github.com/Alphanum404), [amigo7kr](https://translate.mattermost.com/user/amigo7kr), [amyblais](https://github.com/amyblais), [amynicol1985](https://github.com/amynicol1985), [andrew-delph](https://github.com/andrew-delph), [andrleite](https://github.com/andrleite), [angeloskyratzakos](https://github.com/angeloskyratzakos), [aniketh-varma](https://github.com/aniketh-varma), [anneschuth](https://translate.mattermost.com/user/anneschuth), [apshada](https://github.com/apshada), [ARJ2160](https://github.com/ARJ2160), [ArturBa](https://github.com/ArturBa), [asaadmahmood](https://github.com/asaadmahmood), [AsisRout](https://github.com/AsisRout), [avas27JTG](https://github.com/avas27JTG), [AvaterClasher](https://github.com/AvaterClasher), [ayrotideysarkar](https://github.com/ayrotideysarkar), [ayusht2810](https://github.com/ayusht2810), [balajik](https://github.com/balajik), [Bangik](https://github.com/Bangik), [bartaz](https://translate.mattermost.com/user/bartaz), [BaumiCoder](https://github.com/BaumiCoder), [BenCookie95](https://github.com/BenCookie95), [bishalpal](https://github.com/bishalpal), [calebroseland](https://github.com/calebroseland), [cedarice](https://translate.mattermost.com/user/cedarice), [cescpmantidfly](https://translate.mattermost.com/user/cescpmantidfly), [CI-YU](https://github.com/CI-YU), [Ciggzy1312](https://github.com/Ciggzy1312), [codeEmpress1](https://github.com/codeEmpress1), [coltoneshaw](https://github.com/coltoneshaw), [costa-neto](https://github.com/costa-neto), [cpoile](https://github.com/cpoile), [crspeller](https://github.com/crspeller), [ctlaltdieliet](https://github.com/ctlaltdieliet), [cwarnermm](https://github.com/cwarnermm), [danialkeimasi](https://github.com/danialkeimasi), [Delaney](https://github.com/Delaney),  [devinbinnie](https://github.com/devinbinnie), [DHaussermann](https://github.com/DHaussermann), [dhnlr](https://github.com/dhnlr), [dipandhali2021](https://github.com/dipandhali2021), [Eleferen](https://translate.mattermost.com/user/Eleferen), [emdecr](https://github.com/emdecr), [enahum](https://github.com/enahum), [escofresco](https://github.com/escofresco), [esethna](https://github.com/esethna), [fazil-syed](https://github.com/fazil-syed), [fmartingr](https://github.com/fmartingr), [frjaraur](https://github.com/frjaraur), [fyfirman](https://github.com/fyfirman), [gabrieljackson](https://github.com/gabrieljackson), [Gauravpadam](https://github.com/Gauravpadam), [gibsonliketheguitar](https://github.com/gibsonliketheguitar), [h1usertest](https://translate.mattermost.com/user/h1usertest), [hanzei](https://github.com/hanzei), [harsh-solanki21](https://github.com/harsh-solanki21), [harshal2030](https://github.com/harshal2030), [harshalkh](https://github.com/harshalkh), [harshilsharma63](https://github.com/harshilsharma63), [hmhealey](https://github.com/hmhealey), [ialorro](https://github.com/ialorro), [ifoukarakis](https://github.com/ifoukarakis), [imamimam113](https://translate.mattermost.com/user/imamimam113), [isacikgoz](https://github.com/isacikgoz), [iyampaul](https://github.com/iyampaul), [izruff](https://github.com/izruff), [janlengyel](https://github.com/janlengyel), [jannikbertram](https://github.com/jannikbertram), [jasonblais](https://github.com/jasonblais), [jespino](https://github.com/jespino), [jgilliam17](https://github.com/jgilliam17), [jlandells](https://github.com/jlandells), [johnsonbrothers](https://github.com/johnsonbrothers), [josephjosedev](https://github.com/josephjosedev), [jprusch](https://github.com/jprusch), [js029](https://github.com/js029), [jufab](https://github.com/jufab), [JulienTant](https://github.com/JulienTant), [kaakaa](https://github.com/kaakaa), [kalvdans](https://github.com/kalvdans), [kayazeren](https://github.com/kayazeren), [komodin](https://github.com/komodin), [Kritik-J](https://github.com/Kritik-J), [Kshitij-Katiyar](https://github.com/Kshitij-Katiyar), [KuSh](https://github.com/KuSh), [larkox](https://github.com/larkox), [letehaha](https://github.com/letehaha), [libklein](https://github.com/libklein), [lieut-data](https://github.com/lieut-data), [linkvn](https://github.com/linkvn), [ludvigbolin](https://github.com/ludvigbolin), [M-ZubairAhmed](https://github.com/M-ZubairAhmed), [majo](https://translate.mattermost.com/user/majo), [manojmalik20](https://github.com/manojmalik20), [ManuMinue](https://github.com/ManuMinue), [marianunez](https://github.com/marianunez), [master7](https://translate.mattermost.com/user/master7), [matt-w99](https://github.com/matt-w99),  [matthew-w](https://translate.mattermost.com/user/matthew-w), [matthewbirtch](https://github.com/matthewbirtch), [maxtrem271991](https://github.com/maxtrem271991), [mgdelacroix](https://github.com/mgdelacroix), [mickmister](https://github.com/mickmister), [milotype](https://translate.mattermost.com/user/milotype), [mishmanners](https://github.com/mishmanners), [MixeroTN](https://github.com/MixeroTN), [mnj93](https://github.com/mnj93), [mujpao](https://github.com/mujpao), [mustdiechik](https://github.com/mustdiechik), [mvitale1989](https://github.com/mvitale1989), [namanh-asher](https://github.com/namanh-asher), [Navystack](https://github.com/Navystack), [nickmisasi](https://github.com/nickmisasi), [Nico7as](https://translate.mattermost.com/user/Nico7as), [Nityanand13](https://github.com/Nityanand13), [NohaFahmi](https://github.com/NohaFahmi), [otilor](https://github.com/otilor), [Paul-vrn](https://github.com/Paul-vrn), [Peyo6565](https://github.com/Peyo6565), [phoinixgrr](https://github.com/phoinixgrr), [pvev](https://github.com/pvev), [qryptdev](https://github.com/qryptdev), [Quijuletim470](https://translate.mattermost.com/user/Quijuletim470), [returnedinformation](https://github.com/returnedinformation), [riteshmukim](https://github.com/riteshmukim), [rubinaga](https://github.com/rubinaga), [Rutam21](https://github.com/Rutam21), [saideepesh000](https://github.com/saideepesh000), [SaketKaswa20](https://github.com/SaketKaswa20), [saturninoabril](https://github.com/saturninoabril), [sbishel](https://github.com/sbishel), [seoyeongeun](https://translate.mattermost.com/user/seoyeongeun), [Sharuru](https://github.com/Sharuru), [sjcode99](https://github.com/sjcode99), [sondrekje](https://github.com/sondrekje), [sonu27](https://github.com/sonu27), [sp6370](https://github.com/sp6370), [sri-byte](https://github.com/sri-byte), [stafot](https://github.com/stafot), [StreakInTheSky](https://github.com/StreakInTheSky), [streamer45](https://github.com/streamer45), [stylianosrigas](https://github.com/stylianosrigas), [Sudhanva-Nadiger](https://github.com/Sudhanva-Nadiger), [sudheer121](https://github.com/sudheer121), [syedzubeen](https://github.com/syedzubeen), [Tahanima](https://github.com/Tahanima),[tanmaythole](https://github.com/tanmaythole), [this-is-tobi](https://github.com/this-is-tobi), [ThrRip](https://github.com/ThrRip), [TomerPacific](https://github.com/TomerPacific), [toninis](https://github.com/toninis), [trilopin](https://github.com/trilopin), [umrkhn](https://github.com/umrkhn), [varghesejose2020](https://github.com/varghesejose2020), [venugopal1234567](https://github.com/venugopal1234567), [vip2441](https://github.com/vip2441), [wiersgallak](https://github.com/wiersgallak), [wiggin77](https://github.com/wiggin77), [yasserfaraazkhan](https://github.com/yasserfaraazkhan), [yesbhautik](https://github.com/yesbhautik), [ylac](https://github.com/ylac), [ZubairImtiaz3](https://github.com/ZubairImtiaz3)
