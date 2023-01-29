# Update WSL 1 to WSL 2

To see whether your Linux distribution is set to WSL 1 or WSL 2, use the command in PowerShell:
```
wsl -l -v
```
## Update the Linux OS that use WSL 1 to 2
`wsl --set-version <distro name> 2`
```
wsl --set-version Ubuntu 2
```

![image](https://user-images.githubusercontent.com/9446035/215339406-18d8e1e9-dfee-4af7-8a5b-6567110a041e.png)
