# Storage Arrangement Verification

## Owner
**wongivan852**

## Last Verified
2025-12-05

---

## Hardware Setup

### Primary Device: MacBook M4
- **Purpose**: Development workstation
- **Use Cases**:
  - Software development
  - Code compilation
  - Running development servers
  - Testing and debugging
  - Daily productivity tasks

### External Storage: 2TB SSD
- **Purpose**: Backup storage
- **Use Cases**:
  - Time Machine backups
  - Project archives
  - Code repository backups
  - Document backups
  - Media storage overflow

---

## Recommended Backup Strategy

| Item | Location | Backup Frequency |
|------|----------|------------------|
| Active projects | M4 internal SSD | Continuous (Git) |
| Project archives | 2TB SSD | Weekly |
| System backup | 2TB SSD | Daily (Time Machine) |
| Important documents | 2TB SSD | Weekly |

---

## Storage Health Checklist

- [ ] M4 internal storage has sufficient free space (>20% recommended)
- [ ] 2TB SSD is connected and mounting properly
- [ ] Time Machine backups are running successfully
- [ ] Important files are synced to backup drive
- [ ] Git repositories are pushed to remote

---

## Verification Status

| Component | Status | Notes |
|-----------|--------|-------|
| MacBook M4 | Active | Primary development machine |
| 2TB SSD | Active | Backup storage device |
| Arrangement | Verified | Development on M4, Backup on SSD |

---

## Redundancy Policy

**Principle**: Each file should exist in ONE primary location only (plus automated backups).

| Keep on M4 Only | Keep on 2TB SSD Only |
|-----------------|----------------------|
| Active Git repos | Archived projects |
| node_modules, build artifacts | Large media files |
| IDE configs, caches | VM/disk images |
| Work-in-progress files | Old installers (.dmg) |

**Do NOT duplicate**:
- Git repositories (use remote as backup, not SSD copies)
- Development dependencies (regenerable from package files)
- Build outputs (regenerable from source)

See: [REDUNDANCY_REMOVAL_GUIDE.md](./REDUNDANCY_REMOVAL_GUIDE.md) for detailed cleanup steps.

---

## Summary

This setup follows a sensible arrangement:
- **M4 MacBook** handles all development work, leveraging its powerful Apple Silicon for fast compilation and development tasks
- **2TB SSD** serves as a dedicated backup drive, ensuring data safety and providing archive storage

This separation keeps the development environment clean while maintaining reliable backups on the external drive.
