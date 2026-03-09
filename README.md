# ☁️ Hybrid Cloud Media Server & Infrastructure

Tento repozitář obsahuje konfigurační soubory (Docker Compose) a dokumentaci k mému osobnímu projektu hybridní cloudové infrastruktury. Projekt primárně slouží jako vysoce dostupné mediální centrum a ukázka mých dovedností v oblasti správy Linux serverů a kontejnerizace.

## 🏗️ Architektura systému

Systém kombinuje cloudový výpočetní výkon s lokálním archivem a virtuálním úložištěm. Vše běží na **Ubuntu Serveru** a je plně kontejnerizováno pomocí **Dockeru**.

* **Server:** Hetzner Cloud VPS (CX23) + připojený Hetzner Volume (Block Storage).
* **Reverse Proxy & Zabezpečení:** Nginx Proxy Manager (NPM) s automatickou obnovou Let's Encrypt SSL certifikátů.
* **Služby:** Modulární architektura (Jellyfin, Audiobookshelf, Homepage dashboard atd.).
* **Storage 1 (Cloud Cache):** Rclone s pokročilým VFS cachingem (`--vfs-cache-mode full`). Připojeno k Real-Debrid API.
* **Storage 2 (Lokální archiv):** Propojení lokálního domácího serveru (Dell) s cloudovou VPS přes zabezpečený **SSH Tunel (Remote Port Forwarding)** a následný mount pomocí SSHFS.

## 🚀 Klíčové technické řešení (Highlights)

V tomto projektu jsem řešil a úspěšně implementoval několik pokročilých výzev:

1. **Modulární Docker struktura:** Každá služba má svou vlastní složku a `docker-compose.yml`, což zajišťuje bezpečnější upgrady a snadnější troubleshooting.
2. **Dynamická propagace mountů:** Využití parametru `rshared` (recursive shared) u Docker svazků. Zajišťuje, že kontejnery (např. Jellyfin) okamžitě registrují změny u namountovaných Rclone/SSHFS disků na hostitelském systému bez nutnosti restartu kontejneru.
3. **Agresivní VFS Caching:** Ochrana proti výpadkům streamu při přehrávání 4K HDR obsahu pomocí 40GB Rclone diskového bufferu přímo na rychlém NVMe/Block storage.
4. **Monitoring a UX:** Služby a zdraví serveru (vytížení disků, CPU) jsou sledovány přes plně customizovaný YAML dashboard (Homepage).

---
*💡 Poznámka k bezpečnosti: Všechny repozitáře jsou pro veřejnou prezentaci očištěny o citlivé údaje, hesla, API klíče a doménová jména.*
