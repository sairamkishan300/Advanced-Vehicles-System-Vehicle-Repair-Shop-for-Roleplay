# Advanced Vehicles System & Vehicle Repair Shop for Roleplay

This repository contains **two standalone but compatible MTA:SA resources**:

- **`Vehicles`** – full vehicle ownership system with garage, impound, car shop, speedometer/mechanics, car panel, and vehicle documents.
- **`vehiclerepair`** – mechanic shop with HTML UI for **repairs, upgrades, handling tuning, storage upgrades, paintjobs and colours**.

They are designed to work together in a RolePlay server, but each resource can also be used independently.

---

## 1. Features

### 1.1 Vehicles Resource (`[vehicles]/Vehicles`)

- **Persistent vehicle ownership** stored in `vehicles.db` (SQLite).
- **Admin vehicle creation** via `/buyVehicle` (and alias `/bv`).
- **Garage system**
  - Create/delete garage markers via ACL-protected commands.
  - Store / retrieve vehicles from garage (HTML UI).
- **Impound system**
  - Admin-controlled impound yards and vehicle impound / release.
- **Car shop / dealership**
  - Configurable car list in `config.lua` (`CarShopData`).
  - Vehicles spawn at configured `vehicleSpawnLocations`.
- **Mechanics / speedometer**
  - Vehicle fuel, speedometer, damage tracking.
  - Extra commands like `/rotate`, `/rot`, `/getfuel`, `/vseat`, `/hl` (see Commands section).
- **Car panel**
  - In‑game panel (HTML UI) for managing and selling vehicles.
- **Vehicle documents panel**
  - HTML panel for viewing vehicle‑related info/documents.
- **Neon integration ready**
  - `haveNeon` and `neon` fields in DB (you can connect them with your own neon resource).
- **Data persistence**
  - On resource start: loads all vehicles from DB and respawns them.
  - On resource stop / player exits vehicle: updates position, fuel, health, damage states, handling, colours, etc.
 <img width="1920" height="1080" alt="document vehicle" src="https://github.com/user-attachments/assets/e7731752-e56d-432f-958c-05071904c240" />

    <img width="1310" height="812" alt="login" src="https://github.com/user-attachments/assets/0a6cc340-380d-4287-b02f-f0604ec8f7f6" />
<img width="532" height="352" alt="manufacture_buy parts" src="https://github.com/user-attachments/assets/f0aa01ea-7636-4813-a2a0-1ceed76770d4" />
<img width="1104" height="720" alt="manufacture_assemble" src="https://github.com/user-attachments/assets/1285b4cb-b7b7-4398-88db-ce60ddb211e3" />

### 1.2 Vehicle Repair Shop (`[vehicles]/vehiclerepair`)

- **Mechanic shop markers**
  - Configure one or more repair shop locations.
  - When a mechanic (ACL group) drives into the marker, a HTML UI panel opens.
- **Repair system**
  - Dynamic repair price based on vehicle health:
    - `REPAIR_BASE_PRICE` and `REPAIR_HEALTH_FACTOR` in `server.lua`.
  - Fixes vehicle and sets health to 1000.
- **Upgrade system**
  - Shows all compatible upgrades with prices (per slot).
  - Purchase and install upgrades directly from the HTML panel.
  - Trial mode (preview upgrades before paying) in the advanced tuning window.
- **Handling packages & custom handling**
  - Predefined packages: `stock`, `sport`, `race`.
  - Custom slider-based handling (max speed, damage multiplier, acceleration multiplier).
- **Storage upgrades**
  - Increase `trunk` and `glovebox` capacity per vehicle model.
  - Integrates with the storage system used by `Vehicles` (base values are taken from/configured like the vehicle system).
- **Colour & paintjob tuning**
  - Change vehicle primary/secondary/tertiary/quaternary colours and headlight colour.
  - Change paintjob (IDs 0–3) via UI and pay once when confirmed.
- **HTML UI + color picker**
  - Clean browser-based repair UI in `ui/index.html`.
  - Integrated color picker (`colorpicker/`) with live preview.
<img width="1052" height="706" alt="customs" src="https://github.com/user-attachments/assets/d001d104-8207-4ddb-8692-6b7361bda085" />
<img width="1298" height="784" alt="carshop" src="https://github.com/user-attachments/assets/9e0ca887-751e-4983-8de6-189a90418342" />
<img width="1038" height="664" alt="impound garage" src="https://github.com/user-attachments/assets/38b6c39e-c754-42aa-a418-970d87b8eb01" />

<img width="1920" height="1080" alt="cartest" src="https://github.com/user-attachments/assets/849dffeb-0baa-455f-99bd-77122b528e06" />
<img width="754" height="402" alt="carpanel" src="https://github.com/user-attachments/assets/e5800bd9-f97a-45bc-844b-ff430d2a0ca1" />

---

## 2. Requirements

- **MTA:SA Server** 1.5.6+ (the Vehicles meta specifies `1.5.6-9.16404` minimum for client).
- **SQLite** support (built‑in in MTA; used for `vehicles.db`).
- Basic knowledge of **ACL groups** and **resource management** in MTA.

---

## 3. Installation

1. **Copy resources** into your server:
   - Place the folders:
     - `[vehicles]/Vehicles`
     - `[vehicles]/vehiclerepair`
   - in your `mods/deathmatch/resources` directory.

2. **Check meta files** (no changes normally required):
   - `Vehicles/meta.xml` – defines all scripts and HTML files for the vehicle system.
   - `vehiclerepair/meta.xml` – defines server/client scripts, UI HTML, and color picker images.

3. **Start resources** from server console or `mtaserver.conf`:

   ```
   start Vehicles
   start vehiclerepair
   ```

4. **Ensure dependencies**
   - If you are using the `/getfuel` command, some fuel‑related code refers to `exports.Reswininv` (an inventory resource). Either:
     - Provide a compatible inventory (or adjust that part of the code), or
     - Disable/comment that part if not using that feature.

---

## 4. Configuration

### 4.1 Vehicles Config (`Vehicles/config.lua`)

Key options:

- **Keybinds**
  - `lockVehicleKEY = "u"` – lock/unlock vehicle key.
  - `openGarageKEY = "e"` – open garage UI key.
  - `storeToGarageKEY = "g"` – store vehicle in garage key.

- **Garage & impound permissions**
  - `PermissionGroup = "Admin"` – ACL group that can create/delete garage & impound markers.

- **Force spawn fines**
  - `ForcespawnAmount = 2000` – fine for force-spawning a vehicle from storage.

- **Garage marker visuals**
  - `markerSize = 15` – world marker radius for garages (used in garage scripts).

- **Vehicle storage capacities**
  - `vehicleStorage` table defines per‑model `trunk` and `glovebox` values.
  - `defaultStorage` is used if the model is not listed.

- **Fuel system**
  - `fuelConsumptionRates` – optional per‑model fuel consumption.
  - `defaultFuelConsumption` – fallback value if model is not listed.

- **Car dealer permission** (future/optional)
  - `dealerPermissionGroup = "Admin"` – ACL group for dealership actions.

- **Car shop UI markers** (`configPontos`)
  - `corMarker` – RGB marker color.
  - `tamanhoMarker` – marker size.
  - `idBlip` – map blip icon ID.
  - `corImagem` – colour of the floating image above the marker.

- **Car shop inventory** (`CarShopData`)
  - List of cars sold in the shop, example entry:

    ```lua
    {
        model = 415,
        displayName = "Mercedes Benz",
        price = 30000,
        brandname = "Benz",
        vehicleType = "Race Car",
        image = "[carimages]/mr_benz.png"
    },
    ```

- **Vehicle spawn positions** (`vehicleSpawnLocations`)
  - List of possible coordinates for spawning purchased vehicles:

    ```lua
    { x = 370, y = -1812, z = 8.3, rx = 0, ry = 0, rz = 90 },
    ```

  - The nearest one to the player is used when buying a vehicle.

> **Tip:** Adjust the **storage**, **fuel**, and **CarShopData** values to match your server’s balance.

### 4.2 Vehicle Repair Config (`vehiclerepair/server.lua`)

- **Mechanic permissions**
  - `mechanicPermissionGroup = "Admin"`
  - Only players in this ACL group can use the mechanic shop UI for repair/advanced tuning.

- **Mechanic shop locations**
  - `mechanicShopPositions` list:

    ```lua
    local mechanicShopPositions = {
        { x = 1512.772, y = -1658.429, z = 13.285 },
        -- add more entries here
    }
    ```

- **Handling packages**

    ```lua
    local handlingPackages = {
        stock = { price = 0, maxVelocity = false, collisionDamageMultiplier = false },
        sport = { price = 2500, maxVelocity = 220, collisionDamageMultiplier = 0.6 },
        race  = { price = 5000, maxVelocity = 260, collisionDamageMultiplier = 0.4 }
    }
    ```

  - `false` values reset that parameter to default handling.

- **Repair price formula**
  - `REPAIR_BASE_PRICE = 1000`
  - `REPAIR_HEALTH_FACTOR = 0.5` per missing health point.

- **Upgrade prices per slot**
  - `upgradePrices` table where key = upgrade slot index.

- **Storage upgrade settings**
  - `STORAGE_MAX_MULTIPLIER = 1.3` → up to +30% storage.
  - `STORAGE_BASE_PRICE = 2000` → cost for a full 30% upgrade.

- **Advanced tuning prices**
  - `HANDLING_CUSTOM_PRICE = 2500` – custom handling sliders.
  - `COLOR_CHANGE_PRICE = 1500` – colour changes.
  - `PAINTJOB_PRICE = 2000` – paintjob changes.

- **Base storage table**
  - `baseStorageByModel` (mirrors `Vehicles` config) defines default `trunk` and `glovebox` values for storage tuning. If not listed, `defaultStorage` is used.

---

## 5. Commands

### 5.1 Admin / Staff Commands – Vehicles Resource

All of the following assume the player is in the required ACL group (usually `Admin`).

- **Create vehicle for player (save to DB)**
  - `/buyVehicle [model] [price] [brandname] [displayName]`
  - Alias: `/bv`
  - Spawns a vehicle at admin’s position, warps the admin into it, saves all data (owner = admin’s account, colours, upgrades, handling, storage, damage states, etc.) to `vehicles.db`.

- **Delete current vehicle from world and DB**
  - `/deleteVehicle`
  - Deletes the vehicle you are currently in and removes it from DB.

- **Get vehicle DB ID**
  - `/GetVehicleId`
  - Alias: `/vid`
  - Shows the unique ID of the vehicle you are currently in.

#### Garage & Impound Management

(From `garage/garage_s.lua` and `garage/impound_s.lua` – these scripts are part of the `Vehicles` resource.)

- **Create garage marker**
  - `/CreateGarage [name]`
  - Creates a garage marker at your position with the given name.

- **Delete nearest garage marker**
  - `/DeleteGarage`
  - Deletes the nearest garage marker to you.

- **Destroy all vehicles (admin cleanup)**
  - `/dav`
  - Alias: `/DestroyAllVehicles`
  - Destroys all vehicles spawned by this system (cooldown is handled inside the script).

- **Create impound marker**
  - `/CreateImpound [name]`

- **Delete nearest impound marker**
  - `/deleteImpound`

- **Impound nearby vehicle**
  - `/impound`
  - Impounds the nearest valid vehicle (with checks & notifications).

#### Car Shop / Misc Commands (Vehicles)

- **Set owner of vehicle by player ID**
  - `/setowner [newOwnerID]`
  - Sets the owner of the vehicle you are sitting in using the target player’s ID.

- **Check your account/name/ID**
  - `/checkmyid`
  - Outputs your account name, ID and nickname.

- **Spawn a shop car at configured location**
  - `/spawncar`
  - Triggers `spawnCarAndTeleport` server event (used by shop logic).

#### Mechanics script (basic utilities in `mechanics/mechanics_s.lua`)

- **Rotate vehicle 180°**
  - `/rotate`
  - Alias: `/rot`

- **Get fuel / gas can (inventory integration)**
  - `/getfuel`
  - Integrates with an inventory export (`exports.Reswininv`). Adjust for your inv system.

- **Switch vehicle seat**
  - `/vseat`
  - Cycles your seat inside the current vehicle.

- **Set headlight colour**
  - `/hl [r] [g] [b]`
  - Sets RGB headlight colour for the current vehicle.

#### Vehicle documents panel

- **Open documents panel**
  - `/vehicle`
  - Toggles the vehicle documents HTML panel for your current vehicle.


### 5.2 Vehicle Repair Shop (vehiclerepair)

The repair shop is mostly **marker based** and uses UI events instead of typed commands.

- Drive a vehicle (as **driver**) into a mechanic shop marker.
- If your account is in `mechanicPermissionGroup` (default: `Admin`):
  - The HTML repair panel opens automatically.
  - From there you can:
    - Repair the vehicle.
    - Install compatible upgrades.
    - Open advanced tuning.
    - Adjust storage capacity.
    - Change handling, colours and paintjob.

**Keybinds:**

- `Escape` – closes tuning/repair windows:
  - If advanced tuning window is open → cancels trial & closes.
  - Else if repair panel is open → closes panel.

---

## 6. Usage Flow

### 6.1 Typical Admin Flow

1. **Create garages & impounds**
   - Use `/CreateGarage` and `/CreateImpound` at desired locations.
2. **Configure shop & vehicles**
   - Edit `Vehicles/config.lua` → adjust `CarShopData`, `vehicleStorage`, `vehicleSpawnLocations`.
3. **Create vehicles for players**
   - Stand where you want the vehicle to spawn.
   - Use `/buyVehicle model price brand displayName`.
   - Give vehicle to the player (set owner) using `/setowner` if needed.
4. **Maintain / clean world**
   - Use `/dav` to destroy all vehicles if the map gets cluttered.
   - Use `/deleteVehicle` to remove individual broken/unused vehicles.

### 6.2 Mechanic / Tuning Flow

1. Add your mechanic accounts to `mechanicPermissionGroup` ACL (default `Admin`, but you can create a dedicated `Mechanic` group).
2. Configure `mechanicShopPositions` in `vehiclerepair/server.lua`.
3. Start `vehiclerepair` resource.
4. Mechanic drives a vehicle into the marker:
   - Repair panel opens.
   - Choose repair or open advanced tuning.
   - Test upgrades/paints/colours in trial mode, then **Apply all** to pay once.

---

## 7. ACL Setup

### 7.1 Vehicles Resource

By default, most admin commands require the **`Admin`** ACL group:

- `PermissionGroup = "Admin"` in `config.lua`.
- Server‑side checks use `isObjectInACLGroup("user." .. getAccountName(getPlayerAccount(player)), aclGetGroup("Admin"))`.

If you want a custom group:

1. Create a group in `acl.xml` (e.g. `VehiclesAdmin`).
2. Add your admin accounts to that group.
3. Change `PermissionGroup` and any hard‑coded `"Admin"` checks to your new group name.

### 7.2 Mechanic Shop

- `mechanicPermissionGroup = "Admin"` in `vehiclerepair/server.lua`.
- For a dedicated mechanic role:
  1. Create ACL group `Mechanic`.
  2. Add your mechanic accounts to it.
  3. Change `mechanicPermissionGroup = "Mechanic"`.

---

## 8. Database

- The `Vehicles` resource uses **SQLite** (`dbConnect("sqlite", "vehicles.db")`).
- Table: `vehicles`
  - Columns include: `id`, `model`, `x`, `y`, `z`, rotations, `owner`, `color`, `paintjob`, `upgrades`, variants, `fuel`, `health`, `headlightColor`, `vehiclestored`, `displayName`, `price`, `brandname`, `numberPlate`, `haveNeon`, `neon`, `handling`, `trunk`, `glovebox`, `interior`, `dimension`, `damage_states`.
- On start and stop the system syncs world vehicle states with DB.

You do **not** need to manually create tables; the script auto-creates them if missing.

---

## 9. Customization Tips

- **Change keybinds** – edit the top of `Vehicles/config.lua`.
- **Balance economy** – adjust prices in `CarShopData`, repair prices, upgrade prices, storage/handling/colour/paintjob costs.
- **Add more mechanic shops** – append to `mechanicShopPositions` list.
- **Integrate with other systems**
  - Inventory: adjust `/getfuel` behavior to your inventory resource.
  - Neon: use `haveNeon` and `neon` DB fields to integrate with your neon resource.
- **UI & branding**
  - Garage / Shop / Documents / Repair UIs use HTML/CSS/JS. You can rebrand, change colours, or translate the text as needed.

---

## 10. Credits

- **Vehicles Resource**
  - Author: `SaiRamKishan` (see `Vehicles/meta.xml`).

You are free to modify configuration, UI and scripts to fit your own server branding and gameplay.
