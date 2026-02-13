- wsl --install kali-linux
- sudo apt update && sudo apt full-upgrade -y
- se der erro abra o sudo nano /etc/apt/sources.list e apague tudo e cole deb [arch=amd64] http://http.kali.org/kali kali-rolling main non-free non-free-firmware contrib
- se der outro erro, agora de python, use o: </br>
---
- sudo rm -rf /var/lib/apt/lists/*
- sudo apt update
- sudo apt full-upgrade -y
---

- depois sudo apt install -y kali-linux-large
- sudo apt install -y kali-win-kex
- kex --win -s


---
DICA:
- apt update --fix-missing AJUDA PROS ERROS

---

melhorar shell:

- sudo apt update
- sudo apt install zsh zsh-autosuggestions zsh-syntax-highlighting -y
- chsh -s $(which zsh)
- pronto, agora com autocomplete melhor, sem som chato e com cores nos comandos.
