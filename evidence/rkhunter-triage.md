# rkhunter warning triage

| Warning | Verification | Verdict |
|---|---|---|
| /usr/bin/lwp-request replaced by a script | `dpkg -S` → libwww-perl; `dpkg --verify libwww-perl` clean (matches Ubuntu checksums) | False positive, whitelisted |
| SSH PermitRootLogin not set | `sshd -T` → `permitrootlogin no` (set in sshd_config.d/01-hardening.conf; rkhunter only reads main sshd_config) | False positive, suppressed |
| Hidden file /etc/.resolv.conf.systemd-resolved.bak | File header: managed by systemd-resolved | Expected system file, allowed |
| Hidden file /etc/.updated | File header: created by systemd-update-done | Expected system file, allowed |

Post-whitelist rescan: no warnings. Daily scans enabled (CRON_DAILY_RUN="true").
