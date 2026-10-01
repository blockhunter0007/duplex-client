Duplex client HTTP API

Base URL: http://127.0.0.1:65534

API Version: 1.0.0

Currently supported game versions: 1.26.3x-1.26.5x

Overview

The Duplex Client exposes a local HTTP API that allows external applications and custom UIs to communicate with the mod backend.

All endpoints are available through:

http://127.0.0.1:65534

IJN: Injection needed

---

API Endpoints

"GET /"

Returns the current API/backend status.

Response

```json
{
  "injected": true,
  "injection_time": 3.304103374481201,
  "api_version": "1.0.0",
  "version": "1.26.52",
  "supported": "1.26.51",
  "sp_message": "ver_family"
}
```

if "injected" is false you have to use the API method /inject first before the client works.

The "injection_time" represents how long the client took to inject in seconds

The "api_version" may change when the backend updates,so don't rely on this documentation as this only updates when major changes to the user facing side are made.

The "version" /is the game version,the software is currently injected in. this value might alsow change due to injecting into different game versions, furthermore the currently supported versions might change without notice,the app will inject even though the version might not be supported indicated by the "sp_message" key in the json. This can lead to crashes bans ect.

The "supported" key gives you information about what game version the injector used as base.

The "sp_message" key gives information about the compatibility between the client and the game, it can be "ver_family" if for example 1.26.52 isn't supported but 1.26.51 is so it's just falls back to the latest 5x version, it can be "fully" if the version is specifically supported and "latest" if the version has no supported version family and no supported version, it just falls back to the latest supported.

---

"GET /inject"

Injects the whole client into the game.

Response

```json
{
  "success": true
}
```

---

"GET /eject"

Ejects the current backend.

Response

```json
{
  "success": true
}
```

IJN

---

"GET /reload"

Reloads the backend, it's equivalent to /eject + /inject.

this method shouldn't be called usually

Response

```json
{
  "success": true
}
```

IJN

---

"GET /toggle_module/{module_id}/{toggle}"

Enables or disables a module.

Example

GET /toggle_module/reach/true

This enables the "reach" module.

To disable it:

GET /toggle_module/reach/false

Response

```json
{
  "success": true
}
```

IJN

---

"GET /set_module_value/{module_id}/{value}"

Sets the numeric value of a module.

Example

GET /set_module_value/reach/6.0

This sets the "reach" module's value to "6.0".

Response

```json
{
  "success": true
}
```

IJN

---

"GET /get_module_config/{module_id}"

Returns the current configuration of a specific module.

Example

GET /get_module_config/reach

Response

```json
{
  "config": {
    "toggle": true,
    "value": 6.0
  },
  "success": true
}
```

IJN

---

"GET /get_working"

Returns the modules currently reported as working.

Response

```json
{
  "working_modules": [
    "reach",
    "forcecords",
    "forcedisablecords",
    "hitbox",
    "phasefly"
  ]
}
```

Example Usage

A UI can use this endpoint to determine which modules are currently available before displaying controls.

IJN

---

"GET /get_config"

Returns the current configuration of all modules.

Response

```json
{
  "reach": {
    "toggle": true,
    "value": 6.0
  },
  "forcecords": {
    "toggle": false
  },
  "forcedisablecords": {
    "toggle": false
  },
  "hitbox": {
    "toggle": false,
    "value": 0.6
  },
  "phasefly": {
    "toggle": false
  },
  "speed": {
    "toggle": false,
    "value": 0.1000000015
  }
}
```

Example Usage

A frontend can call "/get_config" when opening or refreshing its UI to synchronize its controls with the current backend state.

IJN

---

"GET /get_all_modules"

Returns all modules exposed by the backend and their configuration metadata.

Response

```json
{
  "all_modules": {
    "reach": {
      "category": "combat",
      "default_value": 3.0,
      "min_value": 3.0,
      "max_value": 6.0
    },
    "forcecords": {
      "category": "visual"
    },
    "forcedisablecords": {
      "category": "visual"
    },
    "hitbox": {
      "category": "combat",
      "default_value": 0.6,
      "min_value": 0.6,
      "max_value": 10.0
    },
    "phasefly": {
      "category": "movement"
    },
    "speed": {
      "category": "movement",
      "default_value": 0.1000000015,
      "min_value": 0.1000000015,
      "max_value": 5.0
    }
  }
}
```

Example Usage

A UI can use this endpoint to build its controls dynamically.

For example, the response indicates that "reach" supports a numeric value with a range of "3.0" to "6.0".

A frontend could therefore create:

Reach
[ ON / OFF ]

Value
[---------●--]
3.0        6.0

For a module such as "phasefly", which has no value metadata, the UI only needs to provide a toggle.

IJN

---

"GET /get_modules_by_category/{category}"

Returns modules matching the requested category.

Example

GET /get_modules_by_category/combat

Response

```json
{
  "category": "combat",
  "modules": [
    "reach",
    "hitbox"
  ]
}
```

Another example:

GET /get_modules_by_category/movement

would query the movement modules.

IJN

---

Example Frontend Flow

A frontend can use the API in a simple sequence.

1. Check whether the backend is available

GET http://127.0.0.1:65534/

if not injected call GET http://127.0.0.1:65534/inject

2. Discover available modules together with the config

GET http://127.0.0.1:65534/get_all_modules

for the configuration and

GET GET http://127.0.0.1:65534/get_working


for the currently injected modules

3. Get the current state

GET http://127.0.0.1:65534/get_config

4. Change a module's state

GET http://127.0.0.1:65534/toggle_module/reach/true

5. Change its value

GET http://127.0.0.1:65534/set_module_value/reach/6.0

6. Verify the change

GET http://127.0.0.1:65534/get_module_config/reach

Result:

```json
{
  "config": {
    "toggle": true,
    "value": 6.0
  },
  "success": true
}
```

IJN

---

Quick Reference

Method| Endpoint| Purpose
|---|---|---|
"GET"| "/"| Check API/backend status
"GET"| "/eject"| Eject backend
"GET"| "/reload"| Ejects and reinjects backend
"GET"| "/toggle_module/{module_id}/{toggle}"| Enable/disable a module
"GET"| "/set_module_value/{module_id}/{value}"| Set a module value
"GET"| "/get_module_config/{module_id}"| Get a module's configuration
"GET"| "/get_working"| Get currently working modules
"GET"| "/get_config"| Get current configuration
"GET"| "/get_all_modules"| Get module information
"GET"| "/get_modules_by_category/{category}"| Get modules by category

---

Base URL

http://127.0.0.1:65534

API Version: 1.0.0
Currently supported game versions: 1.26.3x-1.26.5x