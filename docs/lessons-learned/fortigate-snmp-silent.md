# FortiGate accepts SNMP requests but never answers (open)

**Status:** open, deferred.

**Symptom.** `snmpget -v2c` from the cluster to the firewall times out for every OID,
including the Fortinet-specific ones.

**Investigation.**

1. Community hosts extended to the lab subnet; `allowaccess snmp` added to the VLAN
   interface; query statuses set explicitly.
2. `diagnose sniffer` on the interface: requests arrive.
3. `diagnose debug application snmpd -1`: the daemon matches the community and the host
   entry and processes `get-next`.
4. `diagnose sniffer` for packets **from** port 161: nothing is ever sent.

**Root cause.** Unknown. Configuration, routing and access are verified; the daemon
receives and parses the request but produces no reply.

**Next steps.** Test from the management VLAN, test SNMPv3, review release notes for the
installed FortiOS build, then open a vendor ticket.

**Takeaway.** Follow the packet in both directions; "no response" splits into "never
arrived" and "never sent", and the fix is different for each.
