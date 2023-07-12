#include <stdio.h>
#include <Windows.h>
#include <sddl.h>

// Function to create a system restore point
BOOL CreateSystemRestorePoint()
{
    HANDLE hSRP;
    DWORD dwError;

    // Create a description for the restore point
    WCHAR description[MAX_PATH];
    wcscpy_s(description, MAX_PATH, L"Registry Cleaner Restore Point");

    // Create the restore point
    SRPINFO srpInfo;
    memset(&srpInfo, 0, sizeof(SRPINFO));
    srpInfo.dwEventType = BEGIN_SYSTEM_CHANGE;
    srpInfo.dwRestorePtType = APPLICATION_INSTALL;
    srpInfo.llSequenceNumber = 0;
    srpInfo.szDescription = description;

    if (!SRSetRestorePoint(&srpInfo, &hSRP))
    {
        dwError = GetLastError();
        wprintf(L"Failed to create system restore point. Error: %lu\n", dwError);
        return FALSE;
    }

    wprintf(L"System restore point created successfully.\n");
    return TRUE;
}

// Function to create a backup of the registry
BOOL CreateRegistryBackup()
{
    WCHAR backupFilePath[MAX_PATH];
    if (!GetTempPath(MAX_PATH, backupFilePath))
    {
        wprintf(L"Failed to get temporary path for backup file.\n");
        return FALSE;
    }

    wcscat_s(backupFilePath, MAX_PATH, L"RegistryBackup.reg");

    // Export the entire registry to a backup file
    if (RegSaveKey(HKEY_LOCAL_MACHINE, NULL, NULL) != ERROR_SUCCESS)
    {
        wprintf(L"Failed to create registry backup.\n");
        return FALSE;
    }

    wprintf(L"Registry backup created successfully at: %s\n", backupFilePath);
    return TRUE;
}

void scanRegistry()
{
    // Create a system restore point
    if (!CreateSystemRestorePoint())
    {
        // Handle the failure if necessary
        return;
    }

    // Create a backup of the registry
    if (!CreateRegistryBackup())
    {
        // Handle the failure if necessary
        return;
    }

    // Continue with the registry scanning and cleaning process
    HKEY hKey;
    if (RegOpenKeyEx(HKEY_CURRENT_USER, "Software", 0, KEY_READ, &hKey) == ERROR_SUCCESS)
    {
        printf("Scanning registry...\n");

        // Rest of the code for scanning the registry and removing obsolete entries

        RegCloseKey(hKey);
    }

    // Restore the registry from the backup if needed
    // ...

    // Remove the temporary backup file
    // ...
}

int main()
{
    scanRegistry();

    return 0;
}
