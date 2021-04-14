# DockerWSL2WindowsGuide
Quick procedure to get Docker working on Windows with WSL2. No extra info, just the steps.

1. Check that you have the correct Windows 10 version. You should have 19041 or greater. Open powershell and run

`[System.Environment]::OSVersion.Version`

2. If the build is not sufficiently recent, go to Settings / Update & Security / Windows Update and upgrade to a recent Windows 10 feature release like 20H2.

3. Now that you are on the correct version of Windows, open an admin PowerShell window and run the commands below (steps from https://docs.microsoft.com/en-us/windows/wsl/install-win10).

``` powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
Restart-Computer
```

4. The reboot will take a few minutes while Windows Updates is running.
   
5. Run the commands below in an admin PowerShell window to finish setting up WSL2.

``` powershell
mkdir C:\wsl_temp
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi -OutFile C:\wsl_temp\wsl_update_x64.msi
& C:\wsl_temp\wsl_update_x64.msi
rm C:\wsl_temp
wsl --set-default-version 2
```

6. Run the commands below in an admin PowerShell window to install Docker.

``` powershell
mkdir C:\docker_temp
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest -Uri https://desktop.docker.com/win/stable/Docker%20Desktop%20Installer.exe -OutFile "C:\docker_temp\Docker Desktop Installer.exe"
"C:\docker_temp\Docker Desktop Installer.exe"
```

7. The install will take a few minutes and will log you off after it finishes.

8. Run the command below in a PowerShell window to clean up the Docker installer file that we've downloaded.

``` powershell
rm C:\docker_temp
```

9. It will take a few minutes for Docker to start up for the first time. Once it does, it will give you an option to go through a tutorial. This is a good way to make sure Docker works on your system. Alternatively, you can run the commands below in PowerShell to just test running containers. You'll get a message on your console from the container. The `prune` command will clean up the container.

``` powershell
docker run hello-world
docker container prune
```