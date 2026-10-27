# 🏠 Domestic Containment Facility

This repository contains the configuration, automation rules, and strict
operational constraints governing Home Assistant on the local precinct network.

The objective of this deployment is not to create a "smart home." A house is
not smart; it is an uncooperative aggregate of brick, wire, and pipe that is
perpetually looking for an excuse to fail. The objective here is
**containment**: keeping appliances strictly local, quiet, and unaware of any
external internet connection.

---

## 📜 Standing Orders (The Non-Negotiables)

1. **The Wall Switch Rule**  
    If a light switch, thermostat, or deadbolt requires an internet connection,
    a cloud broker, or an operational server to perform its basic physical
    duty, it is contraband. Everything must function mechanically when the
    server is unplugged.

2. **The Quarantine Doctrine**  
    All IoT hardware is untrusted, hostile, and quarantined on an isolated VLAN
    with zero outbound WAN routing. If a lightbulb attempts to establish a DNS
    lookup or ping an external server in AWS or Shenzhen, the firewall quietly
    drops the packet into the void.

3. **Local Protocols Only**  
    Zigbee, Z-Wave, local MQTT, and hardwired relays over USB/Ethernet.
    Proprietary cloud bridges and Wi-Fi IoT garbage requiring vendor accounts
    are banned from the premises under penalty of percussive decommissioning.

4. **YAML-First Discipline**  
    UI-driven dashboard configuration invites silent drift. Structural state,
    packages, and critical integrations are declared in version-controlled text
    files so breakages can be traced, blamed, and reverted.

---

## 🗂️ Facility Blueprint

```text
ha-config/                   # my current HA config - ⚠️ Not yet populated⚠️
├── configuration.yaml       # Root declarations; keep minimal - ⚠️ Not yet populated⚠️
├── automations.yaml         # Routine procedures (monitored) - ⚠️ Not yet populated⚠️
├── scripts.yaml             # Manual intervention sequences - ⚠️ Not yet populated⚠️
├── scenes.yaml              # Static states (rarely tolerated) - ⚠️ Not yet populated⚠️
├── packages/                # Modular domain configs (lights, climate, power) - ⚠️ Not yet populated⚠️
│   ├── core_power.yaml
│   ├── perimeter.yaml
│   └── environmental.yaml
├── custom_templates/        # Jinja templates built with stubbornness - ⚠️ Not yet populated⚠️
├── .storage/                # Gitignored runtime state (Home Assistant's mess)
├── secrets.yaml.example     # Sanitized template for operational keys - ⚠️ Not yet populated⚠️
├── .gitignore               # Strict exclusion zone
├── README.md                # well....duh!
└── blueprints               # some blueprints that were deemed "required"
    └── random_schedule_for_Aquara-Pet-Feeder-C1
        └── random_schedule_for_Aquara-Pet-Feeder-C1.yaml

```

---

## 🛑 Secrets & Containment Setup

Under no circumstances is `secrets.yaml` or any authentication token to be
committed to version control. The repository relies on `secrets.yaml.example`
for structural tracking.

### 1. Provisioning Local Secrets

```bash
cp secrets.yaml.example secrets.yaml
chmod 600 secrets.yaml

```

Populate `secrets.yaml` with your local network values, database connection
URIs, and internal tokens.

### 2. Validating the Perimeter Locally

Never restart the production daemon on blind faith. Run the configuration check
before issuing restart signals:

```bash
# If running containerized
docker exec -it homeassistant python -m homeassistant --config /config --script check_config

```

If it throws a red error, fix the YAML indentation. Computers do not forgive
sloppy spacing, and neither do I.

---

## 🔨 Percussive Routines (Automations)

Automations in this environment exist only to resolve operational friction, not
to entertain guests.

* **Deadman Monitors:** Ping sweeps to verify quarantined sensors haven't
  quietly dropped off the mesh without logging a fault.
* **Aggressive Mesh Reset:** Automatic power cycling via smart-plug relay on
  coordinator USB dongles if ZHA/Zigbee2MQTT reports consecutive frame drops.
* **Thermal Throttling:** Immediate shutdown of non-essential loads if rack
  cabinet exhaust exceeds tolerable thresholds.

---

## 📋 Emergency Procedure

If the entire automation stack hangs, falls into a race condition, or attempts
to act clever:

1. Sever power to the host machine.
2. Rely on mechanical wall switches.
3. Apply standard percussive maintenance.
4. Review logs only when calm.

