# V7/x64 — Unix Version 7 on x86-64

**Unix V7 (Bell Labs, 1979) ported to modern 64-bit hardware with TCP/IP, SSH-2, and AWS EC2 support.**

## Quick Start

```bash
# Decompress images
xz -d v7x64.iso.xz
xz -d v7disk.img.xz

# Run in QEMU
qemu-system-x86_64 -cpu max -m 64M \
  -cdrom v7x64.iso \
  -drive file=v7disk.img,format=raw,if=none,id=hd0 \
  -device virtio-blk-pci,drive=hd0 \
  -device virtio-net-pci,netdev=net0,mac=52:54:00:12:34:56 \
  -netdev user,id=net0,hostfwd=tcp::2222-:22,hostfwd=tcp::2323-:23 \
  -serial stdio -display none

# In another terminal:
ssh root@localhost -p 2222
# or
telnet localhost 2323
```

Password: (empty — just press Enter)

## What Works

- **Shell**: Bourne shell with 169 commands (ls, cat, grep, awk, vi, make, yacc, lex...)
- **Networking**: TCP/IP, DHCP, DNS, ARP, ICMP (ping 8.8.8.8)
- **SSH-2**: curve25519-sha256 KEX, Ed25519 host key, AES-128-CTR
- **Telnet**: with IAC protocol support (Ctrl+C works)
- **Select/PTY**: I/O multiplexing, pseudo-terminals for remote login
- **Preemptive scheduling**: 60Hz APIC timer (V7 original was cooperative)

## System Specs

| Feature | Value |
|---------|-------|
| Max memory | 2GB |
| Max disk | 8TB |
| Max file | 4.2GB |
| Block size | 4096 bytes |
| Syscalls | 49 original + 13 network |
| Commands | 169 |
| Bugs fixed | 69 |

## Files

| File | Size | Description |
|------|------|-------------|
| `v7x64.iso.xz` | ~2MB | GRUB + kernel (boot CD) |
| `v7disk.img.xz` | ~1MB | V7 filesystem (169 commands, man pages, source code) |

## Requirements

- QEMU (`qemu-system-x86_64`)
- `xz` for decompression

## Origin

Based on Robert Nordier's V7/x86 port (1999-2007).
Original V7 by Ken Thompson and Dennis Ritchie, Bell Labs, 1979.
