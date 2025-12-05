# Redundancy Removal Guide

## Purpose
Remove duplicate files between M4 MacBook (development) and 2TB SSD (backup) to optimize storage usage.

---

## Principle: Single Source of Truth

| Data Type | Primary Location | Backup Location | Action |
|-----------|------------------|-----------------|--------|
| Active projects | M4 | Git remote | Remove from 2TB SSD |
| Archived projects | 2TB SSD | Git remote | Remove from M4 |
| System backups | 2TB SSD | - | Keep only on SSD |
| Downloads/temp files | M4 | - | Don't backup |
| Media libraries | 2TB SSD | - | Remove from M4 |
| Documents (active) | M4 | 2TB SSD (via Time Machine) | Single copy + backup |

---

## Step 1: Identify Duplicate Files

Run these commands on your M4 to find duplicates:

```bash
# Mount your 2TB SSD first, then:
# Replace /Volumes/2TB_SSD with your actual SSD mount point

# Find duplicate files by name
find ~ -type f -name "*.zip" 2>/dev/null | sort > ~/m4_archives.txt
find /Volumes/2TB_SSD -type f -name "*.zip" 2>/dev/null | sort > ~/ssd_archives.txt
comm -12 <(basename -a $(cat ~/m4_archives.txt) | sort) <(basename -a $(cat ~/ssd_archives.txt) | sort)

# Find large duplicate files (>100MB)
find ~ -type f -size +100M -exec basename {} \; 2>/dev/null | sort -u > ~/m4_large.txt
find /Volumes/2TB_SSD -type f -size +100M -exec basename {} \; 2>/dev/null | sort -u > ~/ssd_large.txt
comm -12 ~/m4_large.txt ~/ssd_large.txt
```

---

## Step 2: Common Redundancies to Remove

### From M4 (keep on 2TB SSD only):
- [ ] Old project archives (.zip, .tar.gz)
- [ ] VM images and disk images
- [ ] Large media files (videos, raw photos)
- [ ] Old application installers (.dmg, .pkg)
- [ ] Completed project backups

### From 2TB SSD (keep on M4 only):
- [ ] Active Git repositories (use remote instead)
- [ ] Node modules, vendor folders, build artifacts
- [ ] Cache files and temporary data
- [ ] IDE settings and configurations

---

## Step 3: Cleanup Commands

### On M4 - Remove items that belong on backup only:

```bash
# Remove old archives (after confirming they're on SSD)
# BE CAREFUL - verify before running!
# rm ~/Downloads/*.dmg
# rm ~/Downloads/*.pkg

# Clean development caches
rm -rf ~/Library/Caches/Homebrew/*
rm -rf ~/.npm/_cacache
rm -rf ~/Library/Developer/Xcode/DerivedData/*

# Find and review large files
du -sh ~/* 2>/dev/null | sort -hr | head -20
```

### On 2TB SSD - Remove items that belong on M4 only:

```bash
# Remove redundant Git repos (keep them on M4 + remote)
# find /Volumes/2TB_SSD -name ".git" -type d

# Remove node_modules from backups
# find /Volumes/2TB_SSD -name "node_modules" -type d -prune

# Remove build artifacts
# find /Volumes/2TB_SSD -name "dist" -type d
# find /Volumes/2TB_SSD -name "build" -type d
# find /Volumes/2TB_SSD -name "__pycache__" -type d
```

---

## Step 4: Time Machine Exclusions

Prevent future redundancy by excluding development artifacts from Time Machine:

```bash
# Add exclusions to Time Machine
sudo tmutil addexclusion ~/node_modules
sudo tmutil addexclusion ~/.npm
sudo tmutil addexclusion ~/.cargo
sudo tmutil addexclusion ~/Library/Caches
sudo tmutil addexclusion ~/Library/Developer/Xcode/DerivedData
sudo tmutil addexclusion ~/.docker

# Verify exclusions
tmutil isexcluded ~/node_modules
```

---

## Step 5: Recommended Tools

| Tool | Purpose | Install |
|------|---------|---------|
| **dupeGuru** | Find duplicate files visually | `brew install dupeguru` |
| **ncdu** | Interactive disk usage viewer | `brew install ncdu` |
| **rdfind** | Find duplicate files by content | `brew install rdfind` |

### Using rdfind to find true duplicates:

```bash
# Find duplicates between M4 home and SSD
rdfind -dryrun true ~ /Volumes/2TB_SSD

# Generate report only (no deletion)
rdfind -outputname duplicates.txt -dryrun true ~ /Volumes/2TB_SSD
```

---

## Final Checklist

After cleanup:

- [ ] M4 contains only active development files
- [ ] 2TB SSD contains only backups and archives
- [ ] No duplicate large files exist on both drives
- [ ] Time Machine exclusions are configured
- [ ] Git repos exist on M4 + remote (not duplicated on SSD)
- [ ] Freed up space verified on both drives

---

## Space Verification

```bash
# Check M4 disk usage
df -h /

# Check 2TB SSD usage
df -h /Volumes/2TB_SSD

# Compare before/after
```
