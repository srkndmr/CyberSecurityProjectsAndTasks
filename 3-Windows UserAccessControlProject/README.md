
# Windows User Access Control Project

This project demonstrates how to set up and manage user access control on a Windows system. Specifically, the task was to create two user accounts: Alice, an administrator, and Bob, a restricted user with limited access.

## Project Overview

In this project, I set up the following tasks:

1. **Created two user accounts:**
   - **Alice**: Full administrative rights over the system.
   - **Bob**: Limited access restricted to his own folder.

2. **Configured user access to specific folders**:
   - Alice has full access to her folder (`C:\Users\Alice`).
   - Bob has restricted access, and he cannot access Alice's folder.

3. **Disabled inheritance** for Alice’s folder to prevent any unintended access from other users or groups.

4. **Set permissions** for Alice’s folder using PowerShell and the `icacls` command:
   - Alice has **Full Control** over her folder.
   - Bob’s access to Alice’s folder was explicitly removed, ensuring he gets an **Access Denied** error if he tries to access it.

5. **Tested the configuration** by logging into both Alice’s and Bob’s accounts to verify the restrictions.

## Steps Taken

1. **Creating Users:**
   - Created two users via PowerShell: Alice (administrator) and Bob (restricted access).

2. **Configuring Folder Permissions:**
   - Used the `icacls` command to grant Alice full control over her folder (`C:\Users\Alice`).
   - Removed any access for Bob using the `icacls` command.

3. **Disabling Inheritance:**
   - Disabled folder inheritance on Alice’s folder to ensure that no inherited permissions from the parent directory (such as `C:\Users`) are applied.

4. **Testing Access Control:**
   - After configuring the folder permissions, I tested by logging in as Bob and trying to access Alice's folder to ensure that he receives an **Access Denied** message.

## Commands Used

1. **Creating Users (via PowerShell):**
   ```powershell
   New-LocalUser "Alice" -Password (ConvertTo-SecureString "Password123" -AsPlainText -Force) -FullName "Alice" -Description "Administrator Account"
   New-LocalUser "Bob" -Password (ConvertTo-SecureString "Password123" -AsPlainText -Force) -FullName "Bob" -Description "Restricted User Account"
   Add-LocalGroupMember -Group "Administrators" -Member "Alice"
   ```

2. **Configuring Permissions on Alice’s Folder:**
   - Grant full control to Alice:
     ```powershell
     icacls "C:\Users\Alice" /grant "Alice:(OI)(CI)F"
     ```

   - Remove Bob’s access to Alice’s folder:
     ```powershell
     icacls "C:\Users\Alice" /remove "Bob"
     ```

3. **Disable Inheritance on Alice’s Folder:**
   ```powershell
   icacls "C:\Users\Alice" /inheritance:r
   ```

4. **Verify Permissions:**
   ```powershell
   icacls "C:\Users\Alice"
   ```

5. **Test Access (Logged in as Bob):**
   - Attempted to access Alice's folder and received the **Access Denied** message.

## Conclusion

By following these steps, I successfully configured user access control on a Windows machine. Alice has full administrative access, while Bob’s access is restricted to only his own folder. These steps demonstrate a basic but essential method of managing permissions in a Windows environment.
