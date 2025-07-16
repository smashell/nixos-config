This is my personal NixOS configuration, forked from [dustinlyons](https://github.com/dustinlyons/nixos-config/) and modified for my personal needs. 
# Installing
## MacOS
1. Install Nix
The recommended way to install is using [determinate](https://determinate.systems/).
> Important. The installer will ask if you want to install Determinate Nix. Answer No as it currently conflicts with nix-darwin
2. Initialize a starter template
Choose one of two options:
2.1  Simplified version without secrets management
```bash
mkdir -p nixos-config && cd nixos-config && nix flake --extra-experimental-features 'nix-command flakes' init -t github:smashell/nixos-config#starter
```
2.2 Full version with secrets management
```bash
mkdir -p nixos-config && cd nixos-config && nix flake --extra-experimental-features 'nix-command flakes' init -t github:smashell/nixos-config#starter-with-secrets
```
3. Make apps executable
```bash
find apps/$(uname -m | sed 's/arm64/aarch64/')-darwin -type f \( -name apply -o -name build -o -name build-switch -o -name create-keys -o -name copy-keys -o -name check-keys -o -name rollback \) -exec chmod +x {} \;
```
4. Apply your current user info
```bash
nix run .#apply
```
5. Customizing Nix configuration files
5.1 Decide what packages to install, Review these files:
- modules/darwin/casks.nix
- modules/darwin/packages.nix
- modules/shared/packages.nix
5.2 Review your shell configuration
Add anything from your existing ~/.zshrc, or just review the new configuration.
- modules/darwin/home-manager
- modules/shared/home-manager
6. Optional: Setup secrets
6.1 Create a private Github repo to hold your secrets
In Github, create a private nix-secrets repository with at least one file (like a README). You'll enter this name during installation.
6.2 Install keys
Create new keys:
```bash
nix run .#create-keys
```
7. Build configuration
Ensure the build works before deploying the configuration, run:
```bash
nix run .#build
```
8. Install configuration
Finally, alter your system with this command:
```bash
nix run .#build-switch
```
# Thanks
 - [dustinlyons'](https://github.com/dustinlyons/nixos-config/)
