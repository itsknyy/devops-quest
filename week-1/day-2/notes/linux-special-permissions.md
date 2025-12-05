# Special Permissions in Linux

## **SUID (Set User ID)**

- **Purpose**: SUID allows a file to run with the permissions of the file's owner (usually root), not the user who runs it.
- **How it works**: When SUID is set, the file runs as the file's owner, even if another user runs it.
- **Example**: A common example is the `/usr/bin/passwd` command. It allows normal users to change their passwords but runs with `root` permissions to update system files.

  **Setting SUID**:
  To set the SUID bit on a file:
  ```bash
  chmod u+s filename

---

## **SGID (Set Group ID)**

- **Purpose**: The SGID bit makes a file run with the group permissions of the file’s group owner not the user’s group.
- **Example**: If SGID is set on a directory, files created inside that directory will inherit the directory's group instead of the user's group.

  **Setting SGID**:
  To set the SGID bit on a file or directory:
  ```bash
  chmod g+s filename_or_directory

---

## **Sticky Bit**

- **Purpose**: The Sticky Bit is used on directories to make sure only the owner of a file can delete or rename it, even if others have write permissions to the directory.
- **How it works**: When the sticky bit is set on a directory, only the file owner can delete or modify their own files, even if other users have permission to write to the directory.

  **Setting the Sticky Bit**:
  To set the sticky bit on a directory:
  ```bash
  chmod +t directory_name

---

