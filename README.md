# MacDojo

## 🧭 Základy (navigace)

pwd                       # aktuální cesta
ls -lah                   # přehledně, skryté soubory i velikosti
cd ~/Downloads            # do složky
mkdir -p Projekty/{01,02} # víc složek najednou

## 📁 Soubory & složky

cp -a zdroj/ cil/          # kopie se zachováním práv/časů
mv -n soubor cil/          # přesun bez přepsání (-n = no clobber)
rm -rf slozka              # pozor: nevratné
touch README.md            # prázdný soubor
open .                     # otevřít ve Finderu
open -R soubor.txt         # „Reveal in Finder“

## 🧰 Hromadné operace (bez nových utilit)

# 1) Přesun 34_* do složky 34, 35_* do 35, … (v ~/Downloads)
cd ~/Downloads && find . -maxdepth 1 -type f -E -regex '^./[0-9]+_.*' -print0 \
| xargs -0 -I{} sh -c 'f="{}"; b="${f#./}"; d="${b%%_*}"; mkdir -p "$d"; mv -n "$f" "$d/"'

# 2) Vytvoř složky 25..50 (zsh „brace expansion“)
unsetopt IGNORE_BRACES 2>/dev/null; mkdir -p {25..50}

# 3) Hromadná náhrada textu ve všech .txt (in-place záloha .bak)
grep -rl --include='*.txt' 'stare' . | xargs sed -i.bak 's/stare/nove/g'

## 🪟 Finder & GUI z CLI

open -a "TextEdit" README.md   # otevřít v aplikaci
qlmanage -p soubor.pdf >/dev/null 2>&1  # rychlý náhled (Quick Look)
osascript -e 'display notification "Hotovo" with title "Skript"'

## 🔎 Spotlight & metadata

mdfind 'kMDItemFSName = "*faktura*"c' -onlyin ~/Documents  # hledání
mdls soubor.pdf                                            # metadata

## 🌐 Síť & porty

networksetup -listallhardwareports       # mapování rozhraní
ipconfig getifaddr en0                   # IP na Wi-Fi (často en0)
nc -vz example.com 443                   # test spojení na port
curl -I https://example.com              # HTTP hlavičky
lsof -nP -iTCP -sTCP:LISTEN              # kdo naslouchá na portech

## ⚙️ Procesy

top -o cpu                 # „htop“ náhrada
ps aux | grep 'nazev'      # najdi proces
pkill -f 'pattern'         # kill podle vzoru

## 🔋 Napájení & neusínání

caffeinate -i              # neuspávat (dokud běží)
caffeinate -di             # neuspávat ani displej
pmset -g batt              # stav baterie

## 📦 Disk & místo

df -h                      # přehled disků
du -sh *                   # velikosti v aktuální složce
diskutil list              # disky a oddíly

## 🗜️ Komprese & archivy

zip -r archiv.zip slozka/          # ZIP
unzip archiv.zip -d cil/            # rozbalení
tar -czf archiv.tgz slozka/         # tar.gz
tar -xzf archiv.tgz -C cil/         # rozbalení do cil/

## 📋 Schránka & text

pbcopy < soubor.txt         # do schránky
pbpaste > vystup.txt        # ze schránky
say "Build hotov."          # přečíst nahlas

## 📸 Screenshoty & média

screencapture -i ~/Desktop/screen.png   # interaktivní výběr
screencapture -c                        # do schránky
afplay zvuk.mp3                         # přehrát audio

## 🔐 Práva, atributy, karanténa

chmod -R u+rwX,go-rwx slozka/      # rozumná práva pro vlastníka
chown -R $USER:staff slozka/       # změna vlastníka
xattr -l app.app                   # rozšířené atributy
xattr -d com.apple.quarantine soubor.dmg  # odstanit karanténu
chflags hidden soubor && chflags nohidden soubor

## 🚀 Spouštění při přihlášení (LaunchAgents)

# umístění: ~/Library/LaunchAgents/
launchctl load  ~/Library/LaunchAgents/com.me.job.plist
launchctl unload ~/Library/LaunchAgents/com.me.job.plist
launchctl list | grep com.me

## 🔑 SSH klíče

ssh-keygen -t ed25519 -C "email@example.com"
pbcopy < ~/.ssh/id_ed25519.pub   # vlož klíč do GitHubu/Gitu

## 🍺 Homebrew (volitelné)

brew update && brew upgrade
brew install wget jq           # příklady nástrojů
brew list --versions           # co je nainstalováno

## 🐍 Python (co už je v systému)

python3 -m venv .venv && source .venv/bin/activate
python3 -m pip install --upgrade pip
python3 -m http.server 8000    # rychlý lokální server

## 🌀 Zsh triky (rychlejší práce)

echo {01..12}                  # sekvence (může vyžadovat: unsetopt IGNORE_BRACES)
setopt extendedglob            # chytřejší globbing (dočasně pro sezení)
alias ll='ls -lah'             # šikovný alias

## ✅ Bezpečné vzory (na velká množství souborů)

# Nulové oddělovače + -print0/-0 (bezpečné pro mezery v názvech)
find . -type f -print0 | xargs -0 -n1 echo

# „Nanečisto“ nejdřív echo, až pak mv/rm/cp
find . -name '*.log' -print0 | xargs -0 -I{} echo rm -f "{}"





