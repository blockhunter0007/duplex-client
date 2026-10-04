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

The "version" /is the game version,the software is currently injected in. this value might alsow change due to injecting into different game versions, furthermore the currently supported versions might change without notice,the app will inject even though the version might not be supported indicated by the "sp_message" key in the json.

The "supported" key gives you information about what game version the injector used as base.

The "sp_message" key gives information about the compatibility between the client and the game, it can be "ver_family" if for example 1.26.52 isn't supported but 1.26.51 is so it's just falls back to the latest 5x version, it can be "fully" if the version is specifically supported and "latest" if the version has no supported version family and no supported version, it just falls back to the latest supported.

---

"GET /inject"

Injects the whole client into the game.

WARNING: this API endpoint takes a lot of time to respond, keep a very high timeout like 999 seconds

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

"GET /get_modules"

Returns all modules exposed by the backend and their configuration metadata.

Response

```json
{
  "all_modules": {
    "reach": {
      "injected": true,
      "category": "combat",
      "default_value": 3.0,
      "min_value": 3.0,
      "max_value": 6.0
    },
    "forcecords": {
      "injected": true,
      "category": "visual"
    },
    "forcedisablecords": {
      "injected": true,
      "category": "visual"
    },
    "hitbox": {
      "injected": true,
      "category": "combat",
      "default_value": 0.6,
      "min_value": 0.6,
      "max_value": 10.0
    },
    "phasefly": {
      "injected": true,
      "category": "movement"
    },
    "speed": {
      "injected": true,
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

Example Frontend Flow

A frontend can be initialised in the following way:

1. Check whether the backend is available

GET http://127.0.0.1:65534/

2. If the backend is not injected, inject it

GET http://127.0.1:65534/inject

3. Retrieve the modules, theyre configuration metadata, and the injection status

GET http://127.0.1:65534/get_modules

4. Retrieve the current configuration of all modules

GET http://127.0.1:65534/get_config

5. Draw the UI and and bind events

6. (Optional) Update the configuration of a module

GET http://127.0.1:65534/toggle_module/reach/true

GET http://127.0.1:65534/set_module_value/reach/6.0

---

Quick Reference

Method| Endpoint| Purpose
|---|---|---|
"GET"| "/"| Check API/backend status
"GET"| "/inject"| Inject backend
"GET"| "/eject"| Eject backend
"GET"| "/reload"| Ejects and reinjects backend
"GET"| "/toggle_module/{module_id}/{toggle}"| Enable/disable a module
"GET"| "/set_module_value/{module_id}/{value}"| Set a module value
"GET"| "/get_config"| Get current configuration of all modules
"GET"| "/get_modules"| Get all modules, their configuration metadata and theyre injection status

---

Base URL

http://127.0.0.1:65534

API Version: 1.0.0
Currently supported game versions: 1.26.3x-1.26.5x
