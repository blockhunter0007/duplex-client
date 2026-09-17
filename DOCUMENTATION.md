Duplex client HTTP API

Base URL: "http://127.0.0.1:65534"

API Version: "1.0.0"

Supported game versions: 1.26.3x-1.26.5x

Overview

The Duplex Client exposes a local HTTP API that allows external applications and custom UIs to communicate with the mod backend.

All endpoints are available through:

http://127.0.0.1:65534

---

API Endpoints

"GET /"

Returns the current API/backend status.

Response

{
  "injected": true,
  "injection_time": 3.304103374481201,
  "api_version": "1.0.0",
  "version": "injected version"
}

---

"GET /eject"

Ejects the current backend.

Response

{
  "success": true
}

---

"GET /reload"

Reloads the backend.

Response

{
  "success": true
}

---

"GET /toggle_module/{module_id}/{toggle}"

Enables or disables a module.

Example

GET /toggle_module/reach/true

This enables the "reach" module.

To disable it:

GET /toggle_module/reach/false

Response

{
  "success": true
}

---

"GET /set_module_value/{module_id}/{value}"

Sets the numeric value of a module.

Example

GET /set_module_value/reach/6.0

This sets the "reach" module's value to "6.0".

Response

{
  "success": true
}

---

"GET /get_module_config/{module_id}"

Returns the current configuration of a specific module.

Example

GET /get_module_config/reach

Response

{
  "config": {
    "toggle": true,
    "value": 6.0
  },
  "success": true
}

---

"GET /get_working"

Returns the modules currently reported as working.

Response

{
  "working_modules": [
    "reach",
    "forcecords",
    "forcedisablecords",
    "hitbox",
    "phasefly"
  ]
}

Example Usage

A UI can use this endpoint to determine which modules are currently available before displaying controls.

---

"GET /get_config"

Returns the current configuration of all modules.

Response

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

Example Usage

A frontend can call "/get_config" when opening or refreshing its UI to synchronize its controls with the current backend state.

---

"GET /get_all_modules"

Returns all modules exposed by the backend and their configuration metadata.

Response

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

---

"GET /get_modules_by_category/{category}"

Returns modules matching the requested category.

Example

GET /get_modules_by_category/combat

Response

{
  "category": "combat",
  "modules": [
    "reach",
    "hitbox"
  ]
}

Another example:

GET /get_modules_by_category/movement

would query the movement modules.

---

Example Frontend Flow

A frontend can use the API in a simple sequence.

1. Check whether the backend is available

GET http://127.0.0.1:65534/

2. Discover available modules

GET http://127.0.0.1:65534/get_all_modules

3. Get the current state

GET http://127.0.0.1:65534/get_config

4. Change a module's state

GET http://127.0.0.1:65534/toggle_module/reach/true

5. Change its value

GET http://127.0.0.1:65534/set_module_value/reach/6.0

6. Verify the change

GET http://127.0.0.1:65534/get_module_config/reach

Result:

{
  "config": {
    "toggle": true,
    "value": 6.0
  },
  "success": true
}

---

Quick Reference

Method| Endpoint| Purpose
|---|---|---|
"GET"| "/"| Check API/backend status
"GET"| "/eject"| Eject backend
"GET"| "/reload"| Reload backend
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

API Version: "1.0.0"
Supported game versions: "1.26.3x-1.26.5x"