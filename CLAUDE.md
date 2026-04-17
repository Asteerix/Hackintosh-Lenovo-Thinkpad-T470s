# Hackintosh-Lenovo-Thinkpad-T470s

Configuration OpenCore EFI pour faire tourner macOS Monterey / Ventura sur un Lenovo ThinkPad T470s (modèles 20HF / 20HG, Intel Core i5-7300U / i7-7600U Kaby Lake, iGPU Intel HD Graphics 620). Repo de fichiers binaires + plist, pas de code applicatif.

## Stack

- **OpenCore** 0.9.x (bootloader)
- **macOS** Monterey (12.x) et Ventura (13.x) — supportés
- **CPU** : Intel Core i5-7300U / i7-7600U (Kaby Lake)
- **GPU** : Intel HD Graphics 620 (iGPU)
- **SMBIOS** recommandé : `MacBookPro14,1` (à régénérer)
- **BIOS** Lenovo : 1.42+

## Commands (manipulation EFI)

```bash
# Pas de build. Workflow standard :
#  1. Préparer un USB d'install macOS (Dortania OpenCore Install Guide)
#  2. Télécharger le dernier release EFI depuis GitHub Releases
#  3. Monter la partition EFI du USB avec MountEFI
#  4. Copier le dossier EFI/ dans la partition EFI
#  5. Régénérer le SMBIOS avec GenSMBIOS (MacBookPro14,1) et patcher config.plist
#  6. Configurer le BIOS (voir Dortania Kaby Lake guide)
#  7. Booter, installer macOS
#  8. Post-install : copier EFI/ vers la partition EFI du disque interne
```

## Project layout

- `EFI/BOOT/` — `BOOTx64.efi` (entry UEFI)
- `EFI/OC/` — OpenCore (config.plist, ACPI, Drivers, Kexts, Resources, Tools)
- `Images/opencore-t470S.png` — capture d'écran du boot picker (utilisée dans README)
- `README.md` — hardware + compatibility matrix + install guide

## Compatibilité

| Feature | Statut |
|---|---|
| Display + brightness | OK |
| Audio (speakers + jack) | OK |
| USB 2.0/3.0 | OK |
| Ethernet | OK |
| Battery + power management | OK |
| Keyboard + TrackPoint | OK |
| Trackpad (gestures) | OK |
| Sleep/Wake | OK |
| iServices (iMessage / FaceTime) | OK avec SMBIOS valide |
| Thunderbolt 3 hotplug | KO |
| Fingerprint reader | KO |
| WWAN (LTE) | KO |
| SD card reader | KO |

## Conventions

- **Toujours régénérer son propre SMBIOS** via GenSMBIOS — ne jamais utiliser les valeurs incluses (blacklist iServices)
- Mettre à jour OpenCore + kexts via OCAuxiliaryTools, jamais à la main sans diff
- Tester chaque évolution sur USB **avant** de toucher l'EFI du disque interne
- Releases versionnées via tags Git (`Releases` GitHub)
- Conventional commits : `type(scope): description`

## Notes

- Repo **éducatif** uniquement. Hackintoshing peut annuler la garantie et endommager le hardware.
- Wi-Fi Intel AC 8265 fonctionne mais limite AirDrop/Handoff → remplacement par Broadcom BCM94360NG recommandé pour pleine fonctionnalité Continuity.
- Si BIOS < 1.42, mettre à jour avant install (compat USB / power management).
- Pas de secret applicatif — mais SMBIOS valide reste sensible (ne pas partager publiquement).
