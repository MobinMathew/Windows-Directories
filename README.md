# Windows-Directories

1. Credential and access logs:
   C:\Windows\System32\config --> inside that
   <img width="1000" height="665" alt="image" src="https://github.com/user-attachments/assets/f44c5be0-a3dd-48c2-a7fc-c3cfdaf7d51b" />
• SAM (Security Accounts Manager)
	• Use: Stores local user account credentials, group memberships, and password hashes (NTLM).
	• Forensic Relevance: Used to recover local usernames, account creation dates, last login timestamps, password hint data, and failed login counts.

• SECURITY
	• Use: Manages system-wide security policies, user rights assignments, and active audit settings.
	• Forensic Relevance: Reveals the machine's local audit configurations, showing what types of events (e.g., logon successes/failures) the operating system was tracking.

• SOFTWARE
	• Use: Contains configuration data for the operating system environment and all installed third-party applications.
	• Forensic Relevance: Acts as an information goldmine. It tracks installed software, autostart programs (Run keys), operating system installation dates, registered owners, and recent file pathways (MRU lists).

• SYSTEM
	• Use: Holds the hardware configuration, device drivers, and system services organized into data structures called "Control Sets."
	• Forensic Relevance: Documents every USB storage device ever connected to the machine (via USBSTOR). It also lists installed Windows services, system startup configurations, timezone settings, and the computer name.

• COMPONENTS
	• Use: Manages configuration details regarding Windows updates, component packages, and system feature installations.
	• Forensic Relevance: Helps verify system patch levels and identifies whether specific operating system features or updates were deliberately modified or rolled back.

• DEFAULT
	• Use: Serves as the configuration template for new user accounts created on the local machine.
	• Forensic Relevance: Indicates default environmental variables or registry settings enforced on new profiles before customized user configurations take over.

• ELAM (Early Launch Anti-Malware)
	• Use: Governs configuration details for drivers initialized during early boot cycles before standard third-party antimalware components load.
	• Forensic Relevance: Helps verify if boot-level security software was operational or if a bootkit attempted to intercept security controls early in the startup sequence.


# Additional System Files & Templates

• BBI / BBD-Template
	• Use: Serves as basic structural frameworks or transactional backup references utilized during operating system deployment and updates.
	• Forensic Relevance: Generally lower priority, but useful for analyzing system update histories and structural integrity checks.

• DRIVERS
	• Use: Contains localized configurations or caching frameworks related to system hardware components and active device drivers.
	• Forensic Relevance: Validates driver signatures and helps track when specific hardware driver updates occurred.

• userdiff
	• Use: An older legacy file used during upgrades to bridge configurations between older OS profile variants and newer registry structures.
	• Forensic Relevance: Rarely contains modern active user telemetry, but remains useful for checking historical system upgrade paths.

• VSMIDK
	• Use: A specialized virtual machine or hypervisor integration layer file associated with Microsoft's virtualized security environments (such as Virtualization-Based Security or Credential Guard).
	• Forensic Relevance: Confirms whether advanced virtualization-based security protections are enabled to isolate high-value system credentials.

# Folders in the Directory

• RegBack
	• Use: Historically contained automatic backups of the core registry hives. Note: Modern Windows 10 and 11 installations leave this folder empty by default to save disk space unless manually re-enabled via the registry.
	• Forensic Relevance: If populated, it provides an un-tampered timeline snapshot of the registry hives from a previous date, allowing investigators to compare configurations before and after a security incident.
	
• systemprofile
	• Use: The dedicated profile directory for the built-in Local System account (NT AUTHORITY\SYSTEM).
	• Forensic Relevance: Contains its own NTUSER.DAT hive. Analyzing this folder reveals network activity, script execution history, and application behaviors running under elevated system privileges.

• TxR
	• Use: Houses the Kernel Transaction Manager (KTM) log files.
	• Forensic Relevance: Tracks transaction logs for registry changes. If an attacker modifies or deletes a registry key right before an investigation, transactional data remnants may still exist within these logs.

• bbimigrate / BFS
	• Use: Temporary migration or system-level staging folders created during feature updates, specialized build rollouts, or baseline configuration shifts.
	• Forensic Relevance: Documents system migration timestamps and confirms file states during major operating system updates.

# System and Event Logs:
C:\Windows\System32\winevt

Folders and Their Uses

• Logs
	• Use: This folder contains the actual Windows Event Log files stored in the modern binary XML format (.evtx).
	• Content: It houses hundreds of separate log files tracking different areas of the operating system. The most famous core logs include Application.evtx, Security.evtx, and System.evtx, alongside hundreds of application- and service-specific logs (e.g., PowerShell, TaskScheduler, RemoteDesktopServices).
	
• TraceFormat
	• Use: This folder is used by the operating system to store formatting instructions, schemas, or definition files for Event Tracing for Windows (ETW) logs and software traces.
	• Content: It helps tools parse raw tracing data into human-readable text by providing the structural context of the logged events.

Digital Forensics Relevance

From a digital forensics and incident response (DFIR) perspective, the Logs folder is arguably the single most important timeline resource on a Windows system. It tracks what happened, when it happened, and who did it.

• Security.evtx (Authentication & Account Activity)
	• Tracks successful and failed login attempts (Event IDs 4624 and 4625).
	• Documents logon types (e.g., Type 2 for physical keyboard, Type 10 for Remote Desktop/RDP, Type 3 for network shares).
	• Records user account creation, deletion, privilege modifications, and explicit credential usage.

• System.evtx (System Integrity & Hardware)
	• Logs system startup, shutdown, and unexpected restarts (crashes/blue screens).
	• Documents the installation of new Windows services (Event ID 7045), a primary mechanism used by malware to establish persistence.
	• Tracks time changes, clear-log commands, and hardware driver alerts.

• Application.evtx (Software Behavior)
	• Records application crashes, errors, installer activity, and updates.
	• Can reveal unauthorized or malicious software executing and failing on the host system.

• Operational & Application-Specific Logs (Deep Diagnostics)
	• Microsoft-Windows-PowerShell/Operational.evtx: Capitalizes script block logging to reveal the exact command-line code executed by an attacker or script.
	• Microsoft-Windows-TerminalServices-LocalSessionManager/Operational.evtx: Pinpoints exact timelines and originating IP addresses for remote desktop (RDP) sessions.
	• Microsoft-Windows-TaskScheduler/Operational.evtx: Tracks the creation and execution of scheduled tasks used by threats for automated execution.
	
