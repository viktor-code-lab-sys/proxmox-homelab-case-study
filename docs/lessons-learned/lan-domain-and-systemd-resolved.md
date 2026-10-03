# Internal names fail to resolve while dig works

**Symptom.** `git`, `scp` and `getent hosts gitlab.<domain>` failed with "Could not
resolve hostname", while `dig gitlab.<domain> @<dns-server>` answered instantly with the
right address. It started on one VM, then appeared on others.

**Investigation.**

1. `resolvectl status` first showed a public resolver as the current server; pinning
   the internal one helped once, then the problem returned.
2. Flushing caches and restarting `systemd-resolved` did not help on the next VM, where
   the upstream was already correct.
3. The service's own log listed its built-in negative trust anchors, ending in
   `... home internal intranet lan local private test`: names under `.lan` get special
   handling instead of plain forwarding.
4. With a static `/etc/resolv.conf`, `getent` still failed: `nsswitch.conf` had
   `hosts: files myhostname resolve [!UNAVAIL=return] dns`, so lookups went to the
   (now disabled) resolver first.

**Root cause.** Two layers: `systemd-resolved` treats `.lan` specially, and the NSS
`resolve` module answers before plain DNS gets a chance.

**Fix.** In both VM templates: disable and mask `systemd-resolved`, write a static
`resolv.conf`, set `hosts: files myhostname dns`.

**Takeaway.** When `dig` works and applications do not, compare the two paths: `dig`
bypasses NSS, applications do not. And avoid special-use TLDs for internal domains.
