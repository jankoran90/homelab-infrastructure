# ☁️ Hybrid Cloud Data & Streaming Infrastructure

Tento repozitář obsahuje konfigurační soubory (Docker Compose) a dokumentaci k mému projektu hybridní cloudové infrastruktury. Projekt primárně slouží jako vysoce dostupné řešení pro zpracování, distribuci a plynulý streaming velkoobjemových datových assetů (Big Data / Media files).

## 🏗️ Architektura systému

Systém kombinuje cloudový výpočetní výkon s lokálním archivem a napojením na externí cloudová API úložiště. Vše běží na **Ubuntu Serveru** a je plně kontejnerizováno pomocí **Dockeru**.

* **Server:** Hetzner Cloud VPS (CX23) + připojený Hetzner Volume (Block Storage).
* **Reverse Proxy & Zabezpečení:** Nginx Proxy Manager (NPM) s automatickou obnovou Let's Encrypt SSL certifikátů.
* **Služby:** Modulární mikroslužbová architektura (aplikační kontejnery, řídicí dashboardy).
* **Storage 1 (Cloud Cache):** Rclone s pokročilým VFS cachingem (`--vfs-cache-mode full`). Dynamicky připojuje a zpracovává data z externího Cloud API.
* **Storage 2 (Lokální archiv):** Propojení lokálního datového uzlu s cloudovou VPS přes zabezpečený **SSH Tunel (Remote Port Forwarding)** a následný mount pomocí SSHFS.

## 🚀 Klíčové technické řešení (Highlights)

V tomto projektu jsem úspěšně implementoval několik pokročilých infrastrukturních výzev:

1. **Modulární Docker struktura:** Každá služba má svou vlastní složku a konfiguraci, což zajišťuje bezpečnější upgrady a snadnější izolaci chyb (troubleshooting).
2. **Dynamická propagace mountů:** Využití parametru `rshared` (recursive shared) u Docker svazků. Zajišťuje, že běžící kontejnery okamžitě registrují změny u namountovaných síťových disků bez nutnosti restartu aplikační vrstvy.
3. **Agresivní VFS Caching:** Ochrana proti výpadkům spojení při kontinuálním přenosu velkých souborů (např. 4K video assety) pomocí vytvoření vlastního 40GB diskového bufferu přímo na rychlém NVMe storage.
4. **Monitoring a UX:** Služby a zdraví serveru (vytížení disků, CPU, I/O operace) jsou agregovány a sledovány přes vlastní plně konfigurovatelný YAML dashboard.

---
*💡 Poznámka: Z bezpečnostních důvodů jsou konfigurační soubory v tomto repozitáři očištěny o reálná doménová jména, API klíče a přístupová hesla.*
