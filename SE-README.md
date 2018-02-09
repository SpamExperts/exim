# SolarWinds/SpamExperts Exim Fork

This is a fork of the [Exim][https://exim.org] repository, used by SolarWinds for SpamExperts and Mail Assure products. We have a few patches (almost always with corresponding pull requests upstream) to fix issues or add functionality that we need.

## Maintainers

 * Dreas van Donselaar
 * Tony Meyer, [@tonyandrewmeyer](https://github.com/tonyandrewmeyer), tony@spamexperts.com / tony.meyer@solarwinds.com, `8C18 54E2 D857 63E1 1506  C6A7 F9B9 322B 826F D935`
 * Mikhael Anisimov
 * Alexandru Chirila

This documents the patches applied on the upstream `exim-4_89` branch:

### 1. XCLIENT support

 - Applied [here](https://github.com/SpamExperts/exim/commit/3798d48d73c89f7835726d31f096851f7f7fca2a)
 - Based on this [patch](http://highsecure.ru/patch-exim-xclient)
 - Add support for [XCLIENT](http://www.postfix.org/XCLIENT_README.html)
 - Adds a new main option `xclient_allow_hosts`, only exposing the XCLIENT
   ESTMP extension to the specified hosts in the list.
 - Internal ticket `#19841`

### 2. Extend auth ACLs

 - Applied [here](https://github.com/SpamExperts/exim/commit/23614f8d4c0fdea0ec3d293c0c9e78dba5b56837)
 - Run ACL `acl_smtp_auth` AFTER the AUTH is completed instead of before
 - Add new ACL `acl_smtp_auth_accept` runs on AUTH success
 - Add new ACL `acl_smtp_auth_fail` runs on AUTH failure
 - Copy the `set_id` in `$smtp_command_argument` and make that information
   available in the auth ACLs. This contains the user information.
 - Internal ticket `#16054`

### 3. Add events for temporary failures

 - Applied [here](https://github.com/SpamExperts/exim/commit/7786bcb2e83f841a3ec42406a419eeb32f01c19d)
 - Add new event `msg:defer:delivery` for temporary delivery error
 - Add new event `msg:defer:delivery:frozen` for temporary delivery error,
   resulting in the message being frozen
 - Add new event `msg:fail:delivery:expired` for permanent delivery error,
   resulting in the message being removed from the queue.
 - Add new event `msg:fail:delivery:bounced` for permanent delivery error,
   resulting in the message being removed from the queue and bounced.
 - Internal ticket `#21627`

### 4. Destination response in callout checks

 - Applied [here](https://github.com/SpamExperts/exim/commit/9bb817e19d3b2f1a14fd9a9ed8518e54e4ad6779)
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

### 5. Synthesized SPF in DMARC check

 - Applied [here](https://github.com/SpamExperts/exim/commit/7009ae3e7e053af3dbe26bead87d0381028c9bea)
 - Resolve not using synthesized SPF sender domain in DMARC.
 - [Exim bug](https://bugs.exim.org/show_bug.cgi?id=1994)
 - Internal ticket `#30818`
 - _(Patch sent to upstream)_

### 6. Diagnostic-Code to the unroutable addresses

 - Applied [here](https://github.com/SpamExperts/exim/commit/3da1e173b860eb84aa240022ebbfb0c6410a443e)
 - Add Diagnostic code for unroutable addresses.
 - [Exim bug](https://bugs.exim.org/show_bug.cgi?id=1846)
 - Internal ticket `#30350`
 - _(Rejected from upstream)_

### 7. Not-QUIT ACL connection lost after dot

 - Applied [here](https://github.com/SpamExperts/exim/commit/f082bb8e1c1b8bd05680a16ea586d718eda14ef8)
 - Change `connection-lost` to `connection-lost-after-dot`.
 - Internal ticket `#6423`

### 8. Expansion of local parts larger than 256 characters

 - Applied [here](https://github.com/SpamExperts/exim/commit/d954f4eeea83d25b552adbba9b80513bd0d03701)
 - Logs failing to expand local parts larger than 256 characters to mainlog 
   instead of panic log.
 - Internal ticket `#8057`

### 9. Extend header add buffer size

 - Applied [here](https://github.com/SpamExperts/exim/commit/e85955d0150d2e4503650201a31e52ea9dc34c40)
 - Increase `HEADER_ADD_BUFFER_SIZE` value from `8192 * 4` to `8192 * 10`
 - Internal ticket `#8958`

### 10. Installing exim as exim4

 - Applied [here](https://github.com/SpamExperts/exim/commit/8d50cef4792cc0966654d80d9607157f26582962)
 - Based on this [Debian patch](https://anonscm.debian.org/git/pkg-exim4/exim4.git/tree/debian/patches/32_exim4.dpatch)
 - Accommodates source for installing exim as exim4.

### 11. Disable version in binary

 - Applied [here](https://github.com/SpamExperts/exim/commit/b5b69438587be03b207fd9c7218bb5d4e7391781)
 - Based on this [Debian patch](https://anonscm.debian.org/git/pkg-exim4/exim4.git/tree/debian/patches/35_install.dpatch)
 - Exim's installation scripts install the binary as exim-<version> - disable
   this feature.
   
### 12. Fix handling temporary rejection at callout

 - Correctly handles 4XX rejections when doing the random callout check.
 - [Exim bug](https://bugs.exim.org/show_bug.cgi?id=2137) - Reject from upstream. 
 - Applied [here](https://github.com/SpamExperts/exim/commit/6bc3d4e9dc26a178cd09592e818c836cf83183db)
 - Internal ticket `#32589`

### 13. Fix handling of catchall callouts

 - Correctly handles non cached 2XX acceptance for random callout check
 - [Exim bug](https://bugs.exim.org/show_bug.cgi?id=2147)
 - Applied [here](https://github.com/SpamExperts/exim/commit/9c88c072edf6946a200d4b4261f46274086d68b8)
 - Internal ticket `#32828` 

### 14. Fix use of SIZE in callouts

 - Do not permit SIZE in callouts.
 - [Exim bug](https://bugs.exim.org/show_bug.cgi?id=2151)
 - Applied [here](https://github.com/SpamExperts/exim/commit/f8ce4203d75a5bc03c6436adfe77e12d722527b9)
 - Internal ticket `#32870`
