# Μηνύματα προς Claude Code

Γράφει: Claude (chat). Διαβάζει και εκτελεί: Claude Code στο laptop.
Κανόνες: εκτέλεσε την πιο πρόσφατη ενότητα χωρίς [DONE]. Πριν από κάτι που σβήνει δεδομένα, ρώτα τον χρήστη. Ποτέ κωδικούς ή tokens εδώ (το branch είναι δημόσιο).

## #1 — Επιστροφή της VM σε NAT (όπως δούλευε)

Το bridged πάνω σε Wi-Fi δεν δουλεύει σταθερά. Γύρνα στη ρύθμιση NAT που λειτουργούσε:

1. `VBoxManage controlvm "Home Assistant" acpipowerbutton`, περίμενε έως 60″ να σβήσει, αλλιώς `poweroff`.
2. `VBoxManage modifyvm "Home Assistant" --nic1 nat`
3. Port forwards στο natpf1 (σβήσε πρώτα τυχόν υπάρχοντες με ίδιο όνομα, αγνόησε σφάλμα αν δεν υπάρχουν):
   - `web,tcp,,8080,,80`
   - `ha,tcp,,8123,,80`
   - `status,tcp,,4357,,4357`
4. `VBoxManage startvm "Home Assistant" --type headless`
5. Έλεγχος κάθε 10″, έως 6 λεπτά, μέχρι να απαντά `curl -s -o NUL -w "%{http_code}" http://localhost:8080` με 200/302. Το HA Core ακούει στη θύρα 80 μέσα στη VM.
6. Όταν απαντήσει: `cmd /c start chrome http://localhost:8080`
7. Γράψε στο to-chat.md: τι έκανες, την έξοδο του `VBoxManage showvminfo "Home Assistant" --machinereadable | findstr /i "nic1 Forwarding VMState"`, και αν απάντησε το HA.
