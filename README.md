# Feeder

Jednoduchá a rychlá progresivní webová aplikace (PWA) pro správu domácích zásob potravin, sledování pohybů na skladě a generování návrhů jídel pomocí AI automatizace v n8n.

---

## Hlavní funkce

- **Zásoby (Inventory):** Přehled aktuálního stavu zásob na skladě.
- **Katalog (Items):** Seznam všech evidovaných položek a jejich měrných jednotek.
- **Pohyby (Logs):** Historie příjmů, výdajů a inventurních úprav.
- **AI Návrhy jídel:** Integrace s n8n webhookem pro generování tipů na večeři podle aktuálních zásob.
- **Tmavý / Světlý režim:** Přepínání vzhledu aplikace jedním kliknutím.
- **Autentizace:** Bezpečné přihlášení uživatelů přes PocketBase backend.

---

## Technologie

- **Frontend:** Vanilla HTML5, CSS3, JavaScript (ES6+)
- **UI ikony:** Google Material Icons
- **Backend & Databáze:** [PocketBase](https://pocketbase.io/)
- **Automatizace:** [n8n](https://n8n.io/) (webhooky pro zpracování dat a AI návrhy)

---

## Požadavky a Nastavení Backendu

### 1. PocketBase
Aplikace očekává v PocketBase následující kolekce:

1. **`users`**: Standardní auth kolekce.
2. **`main_items`**:
   - `title` (text)
   - `unit` (text, např. `kg`, `ks`, `l`)
   - `type` (select/text, např. `food`)
3. **`main_inventory`**:
   - `item` (relation -> `main_items`)
   - `amount` (number)
4. **`main_inventory_logs`**:
   - `item` (relation -> `main_items`)
   - `amount` (number)
   - `note` (text)
   - `set` (bool - určuje, zda jde o inventurní nastavení stavu)

### 2. Konfigurace v kódu

V souboru `feeder.html` upravte příslušné URL adresy podle vaší infrastruktury:

```javascript
// Adresa vaší PocketBase instance
const pb = new PocketBase('https://pb.example.com');

// Webhook pro zápis pohybů na skladě
fetch('https://n8n.example.com/webhook/LOG_WEBHOOK_ID', { ... });

// Webhook pro návrhy jídel (očekává JSON odpoved ve tvaru { "suggestion": "..." })
fetch('https://n8n.example.com/webhook/MEAL_WEBHOOK_ID', { ... });
```

---

## Spuštění aplikace

Jelikož jde o čistě klientskou aplikaci (Single Page Application bez sestavovacího kroku):

1. Klonujte repozitář:
   ```bash
   git clone https://github.com/svorcdavid/feeder-demo.git
   cd home-inventory
   ```
2. Otevřete soubor `feeder.html` přímo v prohlížeči, nebo jej servírujte pomocí libovolného lokálního serveru (např. VS Code Live Server nebo `python -m http.server 8000`).

---

## Licencování

Tento projekt je poskytován pod licencí **MIT**.