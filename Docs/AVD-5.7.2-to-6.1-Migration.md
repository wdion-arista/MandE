# AVD Migration: 5.7.2 to 6.1

**Date:** 2026-07-23
**Lab:** Media & Entertainment (MandE) - EASTCOAST_FABRIC

---

## Overview

This document covers all changes made to migrate the MandE lab from Arista AVD version 5.7.2 to 6.1. AVD 6.0 introduced breaking schema changes across `eos_designs` and `eos_cli_config_gen`. This migration resolved 83 validation errors and eliminated all warnings.

---

## 1. Removed Data Models

### `avd_data_validation_mode` (removed in 6.0)

- **File:** `global_vars/global_dc_vars.yml`
- **Action:** Removed `avd_data_validation_mode: error`
- **Reason:** This setting no longer exists in AVD 6.x.

### `avd_eos_designs_debug` (renamed in 6.0)

- **File:** `common/playbooks/build.yml`
- **Action:** Renamed to `eos_designs_keep_tmp_files: true`

### `design.type` (removed in 6.0)

- **File:** `group_vars/EASTCOAST_FABRIC.yml`
- **Action:** Removed the `design: type: l3ls-evpn` block
- **Reason:** The `design.type` concept was removed. AVD 6.x infers the design type from the node type keys.
- **Reference:** https://avd.arista.com/6.x/docs/porting-guides/6.x.x.html#removal-of-designtype

---

## 2. Renamed Data Models

### `local_users` -> `aaa_settings.local_users`

- **File:** `global_vars/global_dc_vars.yml`
- **Action:** Wrapped the existing `local_users` list under `aaa_settings:`
- **Before:**
  ```yaml
  local_users:
    - name: cvpadmin
      ...
  ```
- **After:**
  ```yaml
  aaa_settings:
    local_users:
      - name: cvpadmin
        ...
  ```

### `underlay_multicast` -> `underlay_multicast_pim_sm`

- **Files:** `group_vars/EASTCOAST_FABRIC.yml`, `group_vars/PURPLE_LEAFS.yml`
- **Action:** Renamed key
- **Before:** `underlay_multicast: true`
- **After:** `underlay_multicast_pim_sm: true`

### `static_routes` key renames (inside `structured_config`)

- **Files:** `group_vars/BLUE_SPINES.yml`, `group_vars/RED_SPINES.yml`
- **Action:** Within `structured_config.static_routes[]`:
  - `destination_address_prefix` -> `prefix`
  - `gateway` -> `next_hop`
- **Before:**
  ```yaml
  static_routes:
    - destination_address_prefix: 0.0.0.0/0
      gateway: 10.1.99.1
  ```
- **After:**
  ```yaml
  static_routes:
    - prefix: 0.0.0.0/0
      next_hop: 10.1.99.1
  ```

### `dhcp_servers[].subnets` -> `dhcp_servers[].ipv4_subnets`

- **Files:** `group_vars/PURPLE_LEAFS.yml` (3 sites), `group_vars/BLUE_LEAFS.yml` (SITE5)
- **Action:** Renamed `subnets` to `ipv4_subnets` inside each `dhcp_servers` block

### `flow_tracking_settings.trackers[].exporters[].collector` -> `collectors`

- **Files:** `group_vars/MONITOR.yml`, `group_vars/MONITOR_720XP.yml`, `group_vars/MONITOR_7280SR.yml`
- **Action:** Changed from a single object to a list
- **Before:**
  ```yaml
  exporters:
    - name: IPFIX_MandE
      collector:
        host: 127.0.0.1
  ```
- **After:**
  ```yaml
  exporters:
    - name: IPFIX_MandE
      collectors:
        - host: 127.0.0.1
  ```

---

## 3. Removed Keys

### `router_bgp.peer_groups[].type` (invalid in 6.x)

- **Files:** `group_vars/PURPLE_LEAFS.yml`, `group_vars/BLUE_LEAFS.yml`, `group_vars/RED_LEAFS.yml`
- **Action:** Removed `type: evpn` from `structured_config.router_bgp.peer_groups[]`
- **Impact:** The `type` field previously auto-configured `ebgp_multihop`, `maximum_routes`, and address-family activation. These must now be set explicitly:
  ```yaml
  peer_groups:
    - name: EVPN-OVERLAY-PEERS
      ebgp_multihop: 3
      maximum_routes: 0
  address_family_evpn:
    peer_groups:
      - name: EVPN-OVERLAY-PEERS
        activate: true
  address_family_ipv4:
    peer_groups:
      - name: EVPN-OVERLAY-PEERS
        activate: false
  ```

---

## 4. EOS Config Gen Keys Moved to `custom_structured_configuration_`

AVD 6.x no longer passes EOS config gen keys through `eos_designs` to `eos_cli_config_gen`. Top-level keys that are not recognized as `eos_designs` inputs are silently ignored with a warning. The fix is to prefix them with `custom_structured_configuration_`.

### In `group_vars/EASTCOAST_FABRIC.yml`

The following keys were converted from top-level to `custom_structured_configuration_<key>:` form:

| Old Key | New Key |
|---------|---------|
| `management_console:` | `custom_structured_configuration_management_console:` |
| `management_ssh:` | `custom_structured_configuration_management_ssh:` |
| `dns_domain:` | `custom_structured_configuration_dns_domain:` |
| `clock:` | `custom_structured_configuration_clock:` |
| `ipv6_unicast_routing:` | `custom_structured_configuration_ipv6_unicast_routing:` |
| `interface_defaults:` | `custom_structured_configuration_interface_defaults:` |
| `errdisable:` | `custom_structured_configuration_errdisable:` |
| `radius_server:` | `custom_structured_configuration_radius_server:` |
| `aaa_server_groups:` | `custom_structured_configuration_aaa_server_groups:` |
| `aaa_accounting:` | `custom_structured_configuration_aaa_accounting:` |
| `aaa_authentication:` | `custom_structured_configuration_aaa_authentication:` |
| `dot1x:` | `custom_structured_configuration_dot1x:` |
| `static_routes:` | `custom_structured_configuration_static_routes:` |
| `ip_dhcp_snooping:` | `custom_structured_configuration_ip_dhcp_snooping:` |

### In `global_vars/global_dc_vars.yml`

| Old Key | New Key |
|---------|---------|
| `management_api_http:` | `custom_structured_configuration_management_api_http:` |
| `banners:` | `custom_structured_configuration_banners:` |
| `aliases:` | `custom_structured_configuration_aliases:` |
| `aaa_authorization:` | `custom_structured_configuration_aaa_authorization:` |
| `ip_dhcp_snooping:` | `custom_structured_configuration_ip_dhcp_snooping:` |

### In `group_vars/PROD/PROD.yml`

| Old Key | New Key |
|---------|---------|
| `static_routes:` | `custom_structured_configuration_static_routes:` |

### In `group_vars/NETWORK_SERVICES.yml`

| Old Key | New Key |
|---------|---------|
| `mpls:` | `custom_structured_configuration_mpls:` |

### Important: Use Underscore Prefix, Not Dict Form

AVD uses `custom_structured_configuration_prefix` (default: `["custom_structured_configuration_"]`) to match variable names by prefix. Each key must be its own top-level variable:

```yaml
# CORRECT - underscore prefix form
custom_structured_configuration_dns_domain: lab.example.com
custom_structured_configuration_clock:
  timezone: Canada/Eastern

# WRONG - dict form (not processed by AVD)
custom_structured_configuration:
  dns_domain: lab.example.com
  clock:
    timezone: Canada/Eastern
```

---

## 5. Schema Changes in `eos_cli_config_gen`

### `errdisable.recovery.causes` items are now dicts

- **Before (5.7.2):** List of strings with a shared `interval`
  ```yaml
  errdisable:
    recovery:
      causes:
        - bpduguard
        - link-flap
      interval: 30
  ```
- **After (6.1):** List of dicts with per-cause `interval`
  ```yaml
  errdisable:
    recovery:
      causes:
        - name: bpduguard
          interval: 30
        - name: link-flap
          interval: 30
  ```
- **Config output change:** `errdisable recovery cause bpduguard interval 30` (per-cause) instead of a separate `errdisable recovery interval 30` line. Functionally equivalent.

### `radius_server.hosts` renamed to `radius_server.servers`

- **Before:** `radius_server.hosts[].host`
- **After:** `radius_server.servers[].host`

### `aaa_accounting.dot1x.default` uses `methods` list

- **Before (5.7.2):**
  ```yaml
  aaa_accounting:
    dot1x:
      default:
        type: "start-stop"
        group: agni-server-group
  ```
- **After (6.1):**
  ```yaml
  aaa_accounting:
    dot1x:
      default:
        type: "start-stop"
        methods:
          - method: group
            group: agni-server-group
  ```

---

## 6. AVD 6.x Rendering Changes (Expected Diffs)

These differences appear in the generated configs and are inherent to AVD 6.x. They are functionally equivalent and cannot be avoided without modifying AVD itself.

| Difference | AVD 5.7.2 | AVD 6.1 | Impact |
|-----------|-----------|---------|--------|
| BGP maximum-paths | `maximum-paths 4 ecmp 4` | `maximum-paths 4` | None (EOS defaults ecmp to match max-paths) |
| errdisable interval | Global `errdisable recovery interval 30` | Per-cause `errdisable recovery cause X interval 30` | Functionally identical |
| Trailing whitespace | `alias ... tail ` (trailing space) | `alias ... tail` | Cosmetic only |

---

## 7. `evpn_gateway` Behavior Change

- **File:** `group_vars/BLUE_LEAFS.yml` (SITE5_BLUE_LEAFS)
- **Change:** In AVD 6.x, `evpn_gateway` requires explicit `remote_peers` to generate the `EVPN-OVERLAY-CORE` peer group. Without `remote_peers`, the peer group is no longer created.
- **Impact for this lab:** The backup config had an `EVPN-OVERLAY-CORE` peer group with zero neighbors (unused). The new config omits it entirely. No functional impact.
- **To restore:** Add `remote_peers` with hostnames of the remote EVPN GW peers if inter-domain peering is needed.

---

## 8. ContainerLab Changes

### Management Interface: `Management0` -> `Management1`

AVD 6.x in digital twin mode forces `Management1` for all vEOS/cEOS nodes when `digital_twin.environment == "act"`. This is hardcoded in the AVD `mgmt_interface` resolution and cannot be overridden via `mgmt_interface`, `platform_settings`, or `custom_platform_settings`.

**Fix:** Instead of fighting AVD, accept `Management1` and use a cEOS interface mapping file to map containerlab's `eth0` to `Management1`.

- **File created:** `clab/interface_mapping.json` — auto-generated by topgen with all Ethernet interfaces used in the topology
- **Topology update:** `arista_ceos` kind now includes `enforce-startup-config: true` and `binds` for the mapping file

### CLAB-specific EOS Config Gen Keys

- **File:** `group_vars/CLAB/CLAB.yml`
- Keys moved to `custom_structured_configuration_` prefix:

| Old Key | New Key |
|---------|---------|
| `aaa_root:` | `custom_structured_configuration_aaa_root:` |
| `daemon_terminattr:` | `custom_structured_configuration_daemon_terminattr:` |
| `ntp:` | `custom_structured_configuration_ntp:` |
| `ip_name_servers:` | `custom_structured_configuration_ip_name_server:` |

### NTP Schema Change

- `ntp.servers[].vrf` moved to `ntp.vrf` (top-level, no longer per-server)
- **Before:**
  ```yaml
  ntp:
    servers:
      - name: 216.232.132.95
        vrf: default
  ```
- **After:**
  ```yaml
  ntp:
    vrf: default
    servers:
      - name: 216.232.132.95
  ```

### `ip_name_servers` -> `ip_name_server` (Restructured)

The key was renamed (plural to singular) and restructured from a flat list to a VRF-based hierarchy:

- **Before:**
  ```yaml
  ip_name_servers:
    - ip_address: 8.8.8.8
      vrf: default
    - ip_address: 8.8.8.8
      vrf: Production
  ```
- **After:**
  ```yaml
  ip_name_server:
    vrfs:
      - name: default
        servers:
          - ip_address: 8.8.8.8
      - name: Production
        servers:
          - ip_address: 8.8.8.8
  ```

---

## 9. ACT Topology Changes

### `act_topgen` Role Updated for AVD 6.x

AVD 6.x moved `peer`, `peer_interface`, and `peer_type` from top-level keys on `ethernet_interfaces` entries into a `metadata` sub-key. This broke the topology link generation (both `act-logic.j2` and `clab-logic.j2`), producing `links: []`.

- **Files:** `common/playbooks/act_topgen/act-topgen/templates/clab-logic.j2`, `act-logic.j2`
- **Fix:** Added backward-compatible metadata extraction at the start of each interface loop:
  ```jinja2
  set _peer = ifdata.get("metadata", {}).get("peer", ifdata.get("peer"))
  set _peer_interface = ifdata.get("metadata", {}).get("peer_interface", ifdata.get("peer_interface"))
  set _peer_type = ifdata.get("metadata", {}).get("peer_type", ifdata.get("peer_type", ""))
  ```

### ACT `device_model` Mapping

AVD 6.x structured configs use short platform family names (e.g., `7280SR3`) in `metadata.platform`, but ACT requires full model numbers (e.g., `7280SR3-48YC8`).

- **File:** `common/playbooks/act_topgen/act-topgen/defaults/main.yml`
- **Added:** `act_device_model_map` variable to map AVD platform names to ACT-compatible models:
  ```yaml
  act_device_model_map:
    "7280SR3": "7280SR3-48YC8"
    "7280R3": "7280SR3-48YC8"
    "7020TD": "7020TR-48"
    "720XP": "720XP-48ZC2"
  ```
- **Template:** `act-logic.j2` now applies the mapping: `act_device_model_map[_platform] | default(_platform)`

### ACT `generic` Nodes Cannot Have `ports`

ACT rejects `ports` on `generic` node type (used for servers). The template now only adds `ports` for `veos` nodes.

- **File:** `act-logic.j2`
- **Error:** `Node 'site1-server1' of type 'generic' contains field(s) not allowed for this node type: 'ports'`
- **Fix:** Conditional `ports` assignment based on node type

### ACT Management IPs

Changed `act_mgmt_ip` from `192.168.1.x` to `192.168.0.x` in inventory to match CVP's `192.168.0.5/24` network and avoid onboarding warnings.

- **File:** `sites/eastcoast/inventory.yml`

### CVP Node Disabled

Added `act_add_cvp: false` to `group_vars/EASTCOAST_FABRIC.yml` to prevent deploying a CVP VM in ACT. This setting must be in EASTCOAST_FABRIC (not ACT group_vars) because the topgen playbook runs against the PROD inventory.

### Interface Mapping Auto-Generation

The topgen role now auto-generates `interface_mapping.json` dynamically from the structured configs, mapping all Ethernet interfaces used in the topology plus `eth0` -> `Management1`.

- **New file:** `common/playbooks/act_topgen/act-topgen/templates/clab-interface-mapping.j2`
- **New task:** Generates `{{ clab_output_folder }}/interface_mapping.json`
- **New defaults:** `clab_interface_mapping_file`, `clab_mgmt_interface_name`

---

## Files Modified

### Lab Group Vars & Global Vars

| File | Changes |
|------|---------|
| `global_vars/global_dc_vars.yml` | Removed `avd_data_validation_mode`, `local_users` -> `aaa_settings.local_users`, moved 5 keys to `custom_structured_configuration_` |
| `common/playbooks/build.yml` | `avd_eos_designs_debug` -> `eos_designs_keep_tmp_files` |
| `group_vars/EASTCOAST_FABRIC.yml` | Removed `design.type`, renamed `underlay_multicast`, moved 14 keys to `custom_structured_configuration_`, schema fixes for errdisable/radius_server/aaa_accounting, added `act_add_cvp: false` |
| `group_vars/PROD/PROD.yml` | `static_routes` -> `custom_structured_configuration_static_routes`, key renames |
| `group_vars/BLUE_SPINES.yml` | `destination_address_prefix` -> `prefix`, `gateway` -> `next_hop` |
| `group_vars/RED_SPINES.yml` | Same as BLUE_SPINES |
| `group_vars/PURPLE_LEAFS.yml` | `underlay_multicast` rename, `subnets` -> `ipv4_subnets`, removed `peer_groups.type`, added explicit BGP settings |
| `group_vars/BLUE_LEAFS.yml` | `subnets` -> `ipv4_subnets`, removed `peer_groups.type`, added explicit BGP settings |
| `group_vars/RED_LEAFS.yml` | Removed `peer_groups.type`, added explicit BGP settings |
| `group_vars/MONITOR.yml` | `collector` -> `collectors` |
| `group_vars/MONITOR_720XP.yml` | `collector` -> `collectors` |
| `group_vars/MONITOR_7280SR.yml` | `collector` -> `collectors` |
| `group_vars/NETWORK_SERVICES.yml` | `mpls` -> `custom_structured_configuration_mpls` |
| `group_vars/CLAB/CLAB.yml` | Moved `aaa_root`, `daemon_terminattr`, `ntp`, `ip_name_servers` to `custom_structured_configuration_`; NTP vrf restructured; `ip_name_servers` -> `ip_name_server` with VRF hierarchy |
| `group_vars/ACT/ACT.yml` | Added `digital_twin` block |
| `sites/eastcoast/inventory.yml` | Changed `act_mgmt_ip` from `192.168.1.x` to `192.168.0.x` |

### act_topgen Role

| File | Changes |
|------|---------|
| `act-topgen/templates/clab-logic.j2` | AVD 6.x metadata extraction for peer fields, interface collection for mapping, `enforce-startup-config` and `binds` on arista_ceos kind |
| `act-topgen/templates/act-logic.j2` | AVD 6.x metadata extraction for peer fields, `act_device_model_map` lookup, conditional `ports` only for veos nodes |
| `act-topgen/templates/clab-interface-mapping.j2` | New template — auto-generates interface mapping JSON from structured configs |
| `act-topgen/defaults/main.yml` | Added `clab_interface_mapping_file`, `clab_mgmt_interface_name`, `act_device_model_map` |
| `act-topgen/tasks/main.yml` | Added task to generate `interface_mapping.json` |
| `act-topgen/README.md` | Updated for dual ACT/CLAB coverage, AVD 6.x compatibility notes |
| `README.md` (top-level) | Full rewrite with variable reference, `update_clab_act_inventory` script docs |

---

## References

- [AVD 6.x Porting Guide](https://avd.arista.com/6.x/docs/porting-guides/6.x.x.html)
- [custom_structured_configuration usage](https://avd.arista.com/6.x/docs/porting-guides/6.x.x.html#using-eos-config-eos_cli_config_gen-data-models-when-running-eos_designs)
- [design.type removal](https://avd.arista.com/6.x/docs/porting-guides/6.x.x.html#removal-of-designtype)
