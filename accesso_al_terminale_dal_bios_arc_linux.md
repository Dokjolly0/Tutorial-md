
# Ripristino di Arch Linux Tramite Live USB e Accesso alla Console

Questa guida spiega come accedere alla console di Arch Linux e riparare un sistema non avviabile utilizzando un Live USB. Gli utenti possono seguire questi passaggi per risolvere problemi legati a configurazioni errate, partendo dall'accesso al BIOS fino alla modifica dei file di configurazione.

---

## Prerequisiti

1. **Live USB di Arch Linux**: Un dispositivo USB avviabile contenente Arch Linux. Può essere creato utilizzando un'immagine ISO di Arch e uno strumento come Rufus (per Windows) o `dd` (su Linux).
2. **Accesso al BIOS/UEFI**: per configurare l'ordine di avvio.

---

## Procedura Passo-Passo

### Passo 1: Accedere al BIOS/UEFI e Configurare l'Ordine di Avvio

1. **Spegnere il Computer** e riaccenderlo.
2. Durante l'avvio, premere il tasto per entrare nel BIOS/UEFI (di solito `F2`, `Del`, `Esc` o `F10`, a seconda del produttore).
3. Nel BIOS, andare alla sezione **Boot** e impostare il **Live USB** come primo dispositivo di avvio.
4. Salvare le modifiche e uscire dal BIOS.

### Passo 2: Avviare dal Live USB di Arch Linux

1. Dopo aver impostato il BIOS per avviare dal Live USB, si vedrà un menu con le seguenti opzioni:
   - Arch Linux install medium
   - Arch Linux install medium (con supporto vocale)
   - Memtest86+
   - Reboot into firmware interface
2. Selezionare **"Arch Linux install medium"** e premere `Enter` per avviare il live environment.

### Passo 3: Aprire un Terminale e Montare la Partizione di Arch Linux

1. Una volta nel live environment, aprire il terminale (dovrebbe essere accessibile automaticamente).
2. Eseguire il comando seguente per identificare le partizioni disponibili:
   ```bash
   lsblk
   ```
3. Identificare la partizione principale di Arch Linux (di solito `/dev/sda1` o `/dev/nvme0n1p1`).
4. Montare la partizione principale con:
   ```bash
   mount /dev/sda1 /mnt
   ```
5. Se si ha una partizione separata per `/boot`, montarla con:
   ```bash
   mount /dev/sda2 /mnt/boot
   ```

### Passo 4: Accedere al Sistema Installato con Chroot

1. Eseguire il comando seguente per "chrootare" nella partizione montata:
   ```bash
   arch-chroot /mnt
   ```

### Passo 5: Modificare i File di Configurazione

1. Ora si ha accesso completo al sistema installato. Modificare il file di configurazione che causa problemi. Ad esempio:
   ```bash
   nano /etc/X11/xorg.conf.d/10-nvidia-drm-outputclass.conf
   ```
2. Commentare o eliminare le linee che causano problemi. Ad esempio:
   ```plaintext
   # Section "OutputClass"
   #     Identifier "intel"
   #     MatchDriver "i915"
   #     Driver "modesetting"
   # EndSection
   
   # Section "OutputClass"
   #     Identifier "nvidia"
   #     MatchDriver "nvidia-drm"
   #     Driver "nvidia"
   #     Option "AllowEmptyInitialConfiguration"
   #     Option "PrimaryGPU" "yes"
   # EndSection
   ```
3. Salvare il file premendo `Ctrl + O`, poi `Enter`, e uscire con `Ctrl + X`.

### Passo 6: Ripristinare il Boot Loader GRUB (Se Necessario)

1. Se il problema persiste e GRUB non si avvia correttamente, eseguire:
   ```bash
   grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ArchLinux
   ```
2. Generare il file di configurazione di GRUB:
   ```bash
   grub-mkconfig -o /boot/grub/grub.cfg
   ```

### Passo 7: Uscire e Riavviare

1. Uscire dal chroot con:
   ```bash
   exit
   ```
2. Smontare le partizioni:
   ```bash
   umount -R /mnt
   ```
3. Riavviare il computer:
   ```bash
   reboot
   ```

---

## Conclusione

Dopo il riavvio, il sistema dovrebbe avviarsi normalmente. Se il problema persiste, potrebbe essere necessario verificare le impostazioni hardware o consultare la documentazione ufficiale di Arch Linux.

---
