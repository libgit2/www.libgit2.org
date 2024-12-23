---
title: Security (libgit2)
layout: default
---

# Security Information

Information about security advisories affecting libgit2 and the
releases that provide resolution.

In case you think to have found a security issue with libgit2, please
do not open a public issue. Instead, you can report the issue to the
private mailing list [security@libgit2.org](mailto:security@libgit2.org).

Previous security releases:

* **[libgit2 v1.7.2](https://github.com/libgit2/libgit2/releases/tag/v1.7.2)** and **[libgit2 v1.6.5](https://github.com/libgit2/libgit2/releases/tag/v1.6.5)**, Feb 6, 2024

   - CVE-2024-24575: a bug in git_revparse_single is fixed that could
     have caused the function to enter an infinite loop given
     well-crafted inputs, potentially causing a Denial of Service
     attack in the calling application.

   - CVE-2024-24577: a bug in git_index_add is fixed that could have
     caused the function to corrupt its heap and possibly lead to
     arbitrary code execution.

   - A bug in the smart transport negotiation could have caused an
     out-of-bounds read when a remote server did not advertise
     capabilities.

* **[libgit2 v1.5.1](https://github.com/libgit2/libgit2/releases/tag/v1.5.1)** and **[libgit2 v1.4.5](https://github.com/libgit2/libgit2/releases/tag/v1.4.5)**, Jan 20, 2023

   - When using an SSH remote with the optional, included libssh2
     backend, libgit2 does not perform certificate checking by default.
     Prior versions of libgit2 require the caller to set the
     certificate_check field of libgit2's git_remote_callbacks
     structure - if a certificate check callback is not set, libgit2
     does not perform any certificate checking. This means that by
     default - without configuring a certificate check callback,
     clients will not perform validation on the server SSH keys and
     may be subject to a man-in-the-middle attack.

     Beginning in libgit2 v1.4.5 and v1.5.1, libgit2 will now perform
     host key checking by default. Users can still override the default
     behavior using the certificate_check function.

* **[libgit2 v1.4.4](https://github.com/libgit2/libgit2/releases/tag/v1.4.4)** and **[libgit2 v1.3.2](https://github.com/libgit2/libgit2/releases/tag/v1.3.2)**, Jul 12, 2022

   - CVE-2022-29187: this provides compatibility with git's changes to
     address CVE 2022-29187. As a follow up to CVE 2022-24765, now not
     only is the working directory of a non-bare repository examined
     for its ownership, but the .git directory and the .git file (if
     present) are also examined for their ownership.

   - A fix for compatibility with git's (new) behavior for CVE
     2022-24765 allows users on POSIX systems to access a git
     repository that is owned by them when they are running in sudo.

   - A fix for further compatibility with git's (existing) behavior for
     CVE 2022-24765 allows users on Windows to access a git repository
     that is owned by the Administrator when running with escalated
     privileges (using runas Administrator).

   - The bundled zlib is updated to v1.2.12, as prior versions had
     memory corruption bugs. It is not known that there is a security
     vulnerability in libgit2 based on these bugs, but we are updating
     to be cautious.

* **[libgit2 v1.4.3](https://github.com/libgit2/libgit2/releases/tag/v1.4.3)** and **[libgit2 v1.3.1](https://github.com/libgit2/libgit2/releases/tag/v1.3.1)**, Apr 12, 2022

   - CVE 2022-24765: libgit2 is not directly affected by this
     vulnerability, because libgit2 does not directly invoke any
     executable. But we are providing these changes as a security
     release for any users that use libgit2 for repository discovery
     and then also use git on that repository. In this release, we will
     now validate that the user opening the repository is the same user
     that owns the on-disk repository. This is to match git's behavior.
   - Several correctness fixes where invalid input can lead to a crash.
     These may prevent possible denial of service attacks. At this time
     there are not known exploits to these issues.

     - midx: Fix an undefined behavior (left-shift signed overflow)
     - fetch: support OID refspec without dst
     - Fix crash when regenerating a patch with unquoted spaces in
       filename

* **[libgit2 v0.28.4](https://github.com/libgit2/libgit2/releases/tag/v0.28.4)** and **[libgit2 v0.27.10](https://github.com/libgit2/libgit2/releases/tag/v0.27.10)**, Dec 10, 2019

   - CVE-2019-1348: the fast-import stream command "feature
     export-marks=path" allows writing to arbitrary file paths. As
     libgit2 does not offer any interface for fast-import, it is not
     susceptible to this vulnerability.

   - CVE-2019-1349: by using NTFS 8.3 short names, backslashes or
     alternate filesystreams, it is possible to cause submodules to
     be written into pre-existing directories during a recursive
     clone using git. As libgit2 rejects cloning into non-empty
     directories by default, it is not susceptible to this
     vulnerability.

   - CVE-2019-1350: recursive clones may lead to arbitrary remote
     code executing due to improper quoting of command line
     arguments. As libgit2 uses libssh2, which does not require us
     to perform command line parsing, it is not susceptible to this
     vulnerability.

   - CVE-2019-1351: Windows provides the ability to substitute
     drive letters with arbitrary letters, including multi-byte
     Unicode letters. To fix any potential issues arising from
     interpreting such paths as relative paths, we have extended
     detection of DOS drive prefixes to accomodate for such cases.

   - CVE-2019-1352: by using NTFS-style alternative file streams for
     the ".git" directory, it is possible to overwrite parts of the
     repository. While this has been fixed in the past for Windows,
     the same vulnerability may also exist on other systems that
     write to NTFS filesystems. We now reject any paths starting
     with ".git:" on all systems.

   - CVE-2019-1353: by using NTFS-style 8.3 short names, it was
     possible to write to the ".git" directory and thus overwrite
     parts of the repository, leading to possible remote code
     execution. While this problem was already fixed in the past for
     Windows, other systems accessing NTFS filesystems are
     vulnerable to this issue too. We now enable NTFS protecions by
     default on all systems to fix this attack vector.

   - CVE-2019-1354: on Windows, backslashes are not a valid part of
     a filename but are instead interpreted as directory separators.
     As other platforms allowed to use such paths, it was possible
     to write such invalid entries into a Git repository and was
     thus an attack vector to write into the ".git" dierctory. We
     now reject any entries starting with ".git" on all systems.

   - CVE-2019-1387: it is possible to let a submodule's git
     directory point into a sibling's submodule directory, which may
     result in overwriting parts of the Git repository and thus lead
     to arbitrary command execution. As libgit2 doesn't provide any
     way to do submodule clones natively, it is not susceptible to
     this vulnerability. Users of libgit2 that have implemented
     recursive submodule clones manually are encouraged to review
     their implementation for this vulnerability.

* **[libgit2 v0.28.3](https://github.com/libgit2/libgit2/releases/tag/v0.28.3)** and **[libgit2 v0.27.9](https://github.com/libgit2/libgit2/releases/tag/v0.27.9)**, Aug 13, 2019

  - A carefully constructed commit object with a very large number
    of parents may lead to potential out-of-bounds writes or
    potential denial of service.

  - The ProgramData configuration file is always read for compatibility
    with Git for Windows and Portable Git installations. The ProgramData
    location is not necessarily writable only by administrators, so we
    now ensure that the configuration file is owned by the administrator
    or the current user.

* **[libgit2 v0.26.7](https://github.com/libgit2/libgit2/releases/tag/v0.26.7)** and **[libgit2 v0.27.5](https://github.com/libgit2/libgit2/releases/tag/v0.27.5)**, October 5th, 2018  

  - Submodule URLs and paths with a leading "-" are now ignored. This is due to
    the recently discovered CVE-2018-17456, which can lead to arbitrary code
    execution in upstream git. While libgit2 itself is not vulnerable, it can be
    used to inject options in an implementation which performs a recursive clone
    by executing an external command.

  - Submodule URLs and paths with a leading "-" are now ignored. This is due to
    the recently discovered CVE-2018-17456, which can lead to arbitrary code
    execution in upstream git. While libgit2 itself is not vulnerable, it can be
    used to inject options in an implementation which performs a recursive clone
    by executing an external command.

  - When running repack while doing repo writes, `packfile_load__cb()` could see
    some temporary files in the directory that were bigger than the usual, and
    makes memcmp overflow on the p->pack_name string. This issue was reported
    and fixed by bisho.

  - The fix to the unbounded recursion introduced a memory leak in the config
    parser. While this leak was never in a public release, the oss-fuzz project
    reported this as issue 10127. The fix was implemented by Nelson Elhage and
    Patrick Steinhardt

  - When parsing "ok" packets received via the smart protocol, our parsing code
    did not correctly verify the bounds of the packets, which could result in a
    heap-buffer overflow. The issue was reported by the oss-fuzz project, issue
    9749 and fixed by Patrick Steinhardt.

  - The parsing code for the smart protocol has been tightened in general,
    fixing heap-buffer overflows when parsing the packet type as well as for
    "ACK" and "unpack" packets. The issue was discovered and fixed by Patrick
    Steinhardt.

  - Fixed potential integer overflows on platforms with 16 bit integers when
    parsing packets for the smart protocol. The issue was discovered and fixed
    by Patrick Steinhardt.

  - Fixed potential NULL pointer dereference when parsing configuration files
    which have "include.path" or "includeIf..path" statements without a value.

* **[libgit2 v0.26.6](https://github.com/libgit2/libgit2/releases/tag/v0.26.6)** and **[libgit2 v0.27.4](https://github.com/libgit2/libgit2/releases/tag/v0.27.4)**, August 6th, 2018  
This is a security release fixing out-of-bounds reads when processing
smart-protocol "ng" packets. The issue was discovered by the oss-fuzz project,
issue 9406.

* **[libgit2 v0.26.5](https://github.com/libgit2/libgit2/releases/tag/v0.26.5)** and **[libgit2 v0.27.3](https://github.com/libgit2/libgit2/releases/tag/v0.27.3)**, July 9th, 2018  
These releases fix out-of-bounds reads when reading objects from a packfile.
This corresponds to CVE-2018-10887 and CVE-2018-10888, which were both reported
by Riccardo Schirone.<br/><br/>
A specially-crefted delta object in a packfile could trigger an integer overflow
and thus bypass our input validation, potentially leading to objects containing
copies of system memory being written into the object database.

* **[libgit2 v0.26.4](https://github.com/libgit2/libgit2/releases/tag/v0.26.4)**, June 4th, 2018  
Fixes insufficient validation of submodule names (CVE-2018-11235, reported by
Etienne Stalmans) same as v0.27.1.

 * **[libgit2 v0.27.1](https://github.com/libgit2/libgit2/releases/tag/v0.27.1)**, May 29th, 2018  
Ignores submodule configuration entries with names which attempt to perform path
traversal and can be exploited to write to an arbitrary path or for remote code
execution. `libgit2` itself is not vulnerable to RCE but tool implementations
which execute hooks after fetching might be. This is CVE-2018-11235.<br/><br/>
 It is forbidden for a `.gitmodules` file to be a symlink which could cause a Git
implementation to write outside of the repository and and bypass the fsck checks
for CVE-2018-11235.

 * **[libgit2 v0.26.2](https://github.com/libgit2/libgit2/releases/tag/v0.26.2)**, March 8th, 2018  
Fixes memory handling issues when reading crafted repository index files. The
issues allow for possible denial of service due to allocation of large memory
and out-of-bound reads.<br/><br/>
 As the index is never transferred via the network, exploitation requires an
attacker to have access to the local repository.

 * **[libgit2 v0.26.1](https://github.com/libgit2/libgit2/releases/tag/v0.26.1)**, March 7th, 2018  
Updates the bundled zlib to 1.2.11. Users who build the bundled zlib are
vulnerable to security issues in the prior version.<br/><br/>
 This does not affect you if you rely on a system-installed version of zlib. All
users of v0.26.0 who use the bundled zlib should upgrade to this release.

* **[libgit2 v0.24.6](https://github.com/libgit2/libgit2/releases/tag/v0.24.6)** and **[libgit2 v0.25.1](https://github.com/libgit2/libgit2/releases/tag/v0.25.1)**, January 9th, 2017  
Includes two fixes, one performs extra sanitization for some edge cases in
the Git Smart Protocol which can lead to attempting to parse outside of the
buffer.<br><br>
The second fix affects the certificate check callback. It provides a `valid`
parameter to indicate whether the native cryptographic library considered the
certificate to be correct. This parameter is always `1`/`true` before these
releases leading to a possible MITM.<br><br>
This does not affect you if you do not use the custom certificate callback
or if you do not take this value into account. This does affect you if
you use pygit2 or git2go regardless of whether you specify a certificate
check callback.

* **[libgit2 v0.22.1](https://github.com/libgit2/libgit2/releases/tag/v0.22.1)**, January 16, 2015  
Provides additional protections on symbolic links on case-insensitive
filesystems, particularly Mac OS X HFS+.
[Further reading](http://www.edwardthomson.com/blog/another-libgit2-security-update.html).

* **[libgit2 v0.21.3](https://github.com/libgit2/libgit2/releases/tag/v0.21.3)**, December 18, 2015  
Updates protections on the git repository on case-insensitive filesystems,
including Windows NTFS and Mac OS X HFS+: CVE 2014-9390.
[Further reading](https://git-blame.blogspot.co.uk/2014/12/git-1856-195-205-214-and-221-and.html).
