# custom-udev-rules
Nix Flake to easily add custom udev rules. Adds the `services.udev.customRules` configuration option that takes a list of attributes. See the example below.

# Example flake.nix

```nix
{
  description = "My system configuration";
  
  # [...]
  inputs.nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
  inputs.custom-udev-rules.url = "github:MalteT/custom-udev-rules";
  
  # [...]
  outputs = { nixpkgs, ... }@inputs: {
    nixosConfigurations.my-awesome-system = nixpkgs.lib.nixosSystem {
      system = "x86_64-linux";
      modules = [
        inputs.custom-udev-rules.nixosModule
        
        ({ ... }: {
          services.udev.customRules = [{
            name = "85-yubikey";
            rules = ''
              SUBSYSTEM=="usb", ENV{ID_MODEL_ID}=="0407", ENV{ID_VENDOR_ID}=="1050", TAG+="systemd", SYMLINK+="yubikey"
            '';
          }];
        })
      ];
    };
  };
}
```
