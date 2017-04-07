This documents the patches applied on the upstream `exim-4_86_2+fixes` branch:

### XCLIENT support

 - Applied [here](https://github.com/SpamExperts/exim/commit/e6feb53e8decf44deb5fc17157c14da81ac103aa#diff-7920db0a989e807c464ddea0d20a7202R1104)
 - Based on this [patch](http://highsecure.ru/patch-exim-xclient)
 - Add support for [XCLIENT](http://www.postfix.org/XCLIENT_README.html)
 - Adds a new main option `xclient_allow_hosts`, only exposing the XCLIENT
   ESTMP extension to the specified hosts in the list.
 - Internal ticket `#19841`

### Extend auth ACLs

 - Applied in [e6feb53e8decf44deb5fc17157c14da81ac103aa](https://github.com/SpamExperts/exim/commit/0731b18fe371a72821afabc030471b3c890695bd)
 - Run ACL `acl_smtp_auth` AFTER the AUTH is completed instead of before
 - Add new ACL `acl_smtp_auth_accept` runs on AUTH success
 - Add new ACL `acl_smtp_auth_fail` runs on AUTH failure
 - Copy the `set_id` in `$smtp_command_argument` and make that information
   available in the auth ACLs. This contains the user information.
 - Internal ticket `#16054`

### Add events for temporary failures

 - Applied in [e6feb53e8decf44deb5fc17157c14da81ac103aa](https://github.com/SpamExperts/exim/commit/0731b18fe371a72821afabc030471b3c890695bd)
 - Add new event `msg:defer:delivery` for temporary delivery error
 - Add new event `msg:defer:delivery:frozen` for temporary delivery error,
   resulting in the message being frozen
 - Add new event `msg:fail:delivery:expired` for permanent delivery error,
   resulting in the message being removed from the queue.
 - Add new event `msg:fail:delivery:bounced` for permanent delivery error,
   resulting in the message being removed from the queue and bounced.
 - Internal ticket `#21627`

### Destination response in callout checks

 - Applied in [e6feb53e8decf44deb5fc17157c14da81ac103aa](https://github.com/SpamExperts/exim/commit/0731b18fe371a72821afabc030471b3c890695bd)
 - Exposes the destination response for callout checks.
 - Adds a new variable `$recipient_verify_message`, containing the upstream
   response for SMTP callout verifications on the recipient.
 - Adds a new variable `$recipient_verify_cache`, set to True if the callout
   check was based on on the cache, False otherwise.
 - Adds a new variable `$sender_verify_message`, containing the upstream
   response for SMTP callout verifications on the sender.
 - Adds a new variable `$sender_verify_cache`, set to True if the callout
   check was based on on the cache, False otherwise.
 - Internal ticket `#11866`, `#16480`

### Not-QUIT ACL connection lost after dot

 - Applied [here](https://github.com/SpamExperts/exim/commit/e6feb53e8decf44deb5fc17157c14da81ac103aa#diff-9e69cea182c1366d6c233904d02dd6f5R3940)
 - Run the `acl_smtp_notquit` ACL after the connection is lost after dot.
 - Introduces a new `smtp_notquit_reason`: `connection-lost-after-dot`
 - [Discussion](https://lists.exim.org/lurker/message/20100429.041922.9368f358.en.html)
   in mailing list
 - [Exim bug](https://bugs.exim.org/show_bug.cgi?id=1872)
 - Internal ticket `#6423`

### Expansion of local parts larger than 256 characters

 - Applied [here](https://github.com/SpamExperts/exim/commit/e6feb53e8decf44deb5fc17157c14da81ac103aa#diff-00e3894f6b3b9588a830015de0bb06edR1796)
 - Logs failing to expand local parts larger than 256 characters to mainlog
   instead of panic log.
 - Internal ticket `#8057`

### Extend header add buffer size

 - Applied in e6feb53e8decf44deb5fc17157c14da81ac103aa
 - Increase `HEADER_ADD_BUFFER_SIZE` value from `8192 * 4` to `8192 * 10`
 - Internal ticket `#8958`

### Installing exim as exim4

 - Applied in [e6feb53e8decf44deb5fc17157c14da81ac103aa](https://github.com/SpamExperts/exim/commit/0731b18fe371a72821afabc030471b3c890695bd)
 - Based on this [Debian patch](https://anonscm.debian.org/git/pkg-exim4/exim4.git/tree/debian/patches/32_exim4.dpatch)
 - Accommodates source for installing exim as exim4.

### Disable version in binary

 - Applied in [e6feb53e8decf44deb5fc17157c14da81ac103aa](https://github.com/SpamExperts/exim/commit/0731b18fe371a72821afabc030471b3c890695bd)
 - Based on this [Debian patch](https://anonscm.debian.org/git/pkg-exim4/exim4.git/tree/debian/patches/35_install.dpatch)
 - Exim's installation scripts install the binary as exim-<version> - disable
   this feature.


