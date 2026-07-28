# LightRAG Document Management Guide

This guide explains how to manage LightRAG document folders instead of using compressed archives. The new system provides better flexibility and easier document management.

## 🗂️ System Overview

### What Changed
- **OLD**: Single archive files (`lightrag_backup_*.tar.gz`)
- **NEW**: Individual document folders in `lightrag-docs/` directory
- **Benefits**: Better organization, selective uploads, easier backup/restore

### Directory Structure
```
RAG-system/
├── lightrag_manager.sh              # Main management script
├── create_custom_lightrag_folder.sh  # Helper to create new folders
├── lightrag-docs/                   # Local document storage
│   ├── [uuid-1]/                    # Document folder 1
│   │   ├── folder_info.txt          # Metadata about this folder
│   │   └── input/                   # LightRAG input files
│   │       ├── kv_store_doc_status.json
│   │       └── [your-documents]
│   └── [uuid-2]/                    # Document folder 2
│       ├── folder_info.txt
│       └── input/
└── ...
```

## 🚀 Quick Start

### 1. Download Existing Documents
```bash
# Download all LightRAG folders from container
./lightrag_manager.sh download

# Download specific folder
./lightrag_manager.sh download [uuid]

# List what's in the container
./lightrag_manager.sh list-container
```

### 2. Create New Document Folders
```bash
# Create a new folder with your documents
./create_custom_lightrag_folder.sh "my-project" document1.txt document2.pdf

# Create folder with multiple files
./create_custom_lightrag_folder.sh "research-papers" *.pdf *.txt
```

### 3. Upload to Container
```bash
# Upload specific folder
./lightrag_manager.sh upload [uuid]

# Upload all local folders
./lightrag_manager.sh sync
```

## 📋 Complete Command Reference

### LightRAG Manager (`./lightrag_manager.sh`)

| Command | Description | Example |
|---------|-------------|---------|
| `download` | Download all folders from container | `./lightrag_manager.sh download` |
| `download [uuid]` | Download specific folder | `./lightrag_manager.sh download abc-123-def` |
| `upload [uuid]` | Upload specific local folder | `./lightrag_manager.sh upload abc-123-def` |
| `sync` | Sync all local folders to container | `./lightrag_manager.sh sync` |
| `list-container` | List folders in container | `./lightrag_manager.sh list-container` |
| `list-local` | List local folders | `./lightrag_manager.sh list-local` |
| `backup` | Create timestamped backup | `./lightrag_manager.sh backup` |
| `restore [file]` | Restore from backup | `./lightrag_manager.sh restore backup.tar.gz` |
| `clean-container` | Remove all data from container | `./lightrag_manager.sh clean-container` |

### Custom Folder Creator (`./create_custom_lightrag_folder.sh`)

```bash
# Basic usage
./create_custom_lightrag_folder.sh [NAME] [FILES...]

# Examples
./create_custom_lightrag_folder.sh "company-docs" policy.pdf manual.txt
./create_custom_lightrag_folder.sh "research" *.md *.txt
./create_custom_lightrag_folder.sh "project-alpha" /path/to/documents/*
```

## 🔄 Workflow Examples

### Adding New Documents to Your RAG System

1. **Prepare your documents**
   ```bash
   # Create a new folder with your documents
   ./create_custom_lightrag_folder.sh "quarterly-reports" Q1.pdf Q2.pdf Q3.pdf
   ```

2. **Upload to container**
   ```bash
   # Upload the new folder (use the UUID from step 1 output)
   ./lightrag_manager.sh upload [generated-uuid]
   ```

3. **Process in LightRAG**
   - Access the Gradio interface
   - Upload and index your documents through the UI
   - The documents will be processed using LightRAG

### Backing Up Your Work

```bash
# Download all current documents
./lightrag_manager.sh download

# Create a timestamped backup
./lightrag_manager.sh backup

# Result: lightrag-backup-YYYYMMDD_HHMMSS.tar.gz
```

### Restoring from Backup

```bash
# List available backups
./lightrag_manager.sh restore

# Restore specific backup
./lightrag_manager.sh restore lightrag-backup-20240723_143000.tar.gz

# Sync restored files to container
# (script will ask if you want to sync automatically)
```

### Moving Between Environments

```bash
# On source machine: Download and backup
./lightrag_manager.sh download
./lightrag_manager.sh backup

# Transfer backup file to target machine
scp lightrag-backup-*.tar.gz user@target:/path/

# On target machine: Restore and sync
./lightrag_manager.sh restore lightrag-backup-*.tar.gz
```

## 🔧 Integration with Container Startup

The system automatically syncs local folders when starting the container:

```bash
# When you run start_daily.sh, it automatically:
# 1. Checks for local lightrag-docs folder
# 2. Syncs any found folders to the container
# 3. Starts the application

./start_daily.sh
```

## 🎯 Best Practices

### Folder Organization
- Use descriptive names when creating folders: `"legal-contracts"`, `"research-papers"`, `"user-manuals"`
- Keep the `folder_info.txt` files for reference
- Organize related documents in the same folder

### Backup Strategy
- Run `./lightrag_manager.sh backup` before major changes
- Keep multiple backup files for different time periods
- Store backups in a separate location for safety

### Workflow Tips
- Always download existing folders before making local changes
- Use `list-local` and `list-container` to verify sync status
- Test uploads with single folders before bulk operations

## 🐛 Troubleshooting

### Container Not Running
```bash
Error: Container 'kotaemon-instance' is not running

# Solution: Start the container first
./start_daily.sh
```

### Permission Issues
```bash
# If you get permission errors, check container status
podman ps -a

# Restart if needed
podman restart kotaemon-instance
```

### Folder Not Found
```bash
# Check if folder exists locally
./lightrag_manager.sh list-local

# Check if folder exists in container
./lightrag_manager.sh list-container

# Re-download if needed
./lightrag_manager.sh download
```

### UUID Confusion
- Each folder has a UUID-style name (e.g., `abc123-def456-ghi789`)
- Check `folder_info.txt` in each folder to see the original name
- Use `list-local` to see all folders with their sizes and file counts

## 🔄 Migration from Old Archive System

If you have old archive files, you can still use them:

1. **Extract old archives manually**
   ```bash
   tar -xzf lightrag_backup_20240723_120415.tar.gz
   ```

2. **Organize into new structure**
   ```bash
   # Move extracted files into lightrag-docs/ structure
   # Or use create_custom_lightrag_folder.sh to recreate properly
   ```

3. **Sync to container**
   ```bash
   ./lightrag_manager.sh sync
   ```

## ✅ Quick Health Check

Run these commands to verify everything is working:

```bash
# 1. Check container status
podman ps | grep kotaemon-instance

# 2. List container folders
./lightrag_manager.sh list-container

# 3. List local folders
./lightrag_manager.sh list-local

# 4. Test downloading
./lightrag_manager.sh download

# 5. Verify sync capability
./lightrag_manager.sh sync
```

## 📞 Support

If you encounter issues:
1. Check the troubleshooting section above
2. Verify container is running: `podman ps`
3. Check script permissions: `ls -la *.sh`
4. Ensure Python is available for UUID generation: `python3 --version`

---

*This document management system provides much more flexibility than the previous archive-based approach while maintaining all the functionality you need for LightRAG document handling.* 