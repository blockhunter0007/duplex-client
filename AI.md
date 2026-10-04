# AI.md — Duplex Client GUI

Task

Generate a modern, polished GUI for the Duplex Client using the API documentation in "DOCUMENTATION.md".

The GUI should be designed as a local backend dashboard with a clean interface, clear status indicators and dynamically generated module information.

1. Before starting

- Read "DOCUMENTATION.md" completely.
- Understand the available endpoints, response formats and module metadata.
- Do not invent API endpoints, response fields or backend capabilities.
- If an implementation detail is missing, handle it gracefully instead of assuming behavior.

2. Design

Use the following visual style:

[STYLE HERE]

The interface should have:

- A consistent design system.
- Clear typography and visual hierarchy.
- Responsive layouts.
- Consistent spacing, borders and component sizes.
- Accessible contrast and readable status indicators.
- Loading, empty and error states.

3. Backend

Use "DOCUMENTATION.md" as the source of truth for the API.

Keep the timeout high (atleas t for the inject method because it might take longer)

4. Dashboard

Display:

- Backend connection status.
- Injection status.
- API version.
- Current game version.
- Supported base version.
- Compatibility message. (warn if not fully supported
- Injection duration, when available.

Use clear labels and distinguish connection errors from compatibility information.

5. Module explorer

Build the module list dynamically.

Do not assume that every module has a numeric value. Modules without value metadata should be displayed without a numeric control.

Provide category filtering and a module details view.

6. Synchronization

When the GUI opens:

1. Request the backend status. if its not injected inject it.
2. Load module metadata.
3. Load the working module list.
4. Load the current configuration.
5. Build the ui.

Do not display example values as if they were live backend data.

7. Error handling

Handle:

- Backend unavailable.
- Invalid JSON responses.
- Missing fields.
- Unknown modules.
- Failed requests.
- Inconsistent module metadata.
- Backend disconnection during refresh.

Show understandable error messages and provide a way to retry read operations.

8. Implementation quality

- Separate API communication from UI components.
- Use reusable components.
- Keep backend data and display state synchronized.
- Avoid unnecessary repeated requests.
- Do not hardcode the module list.
- Do not silently replace failed API responses with sample data.
- Keep the interface maintainable and easy to extend.

9. On Close

 - Ask the user if they want to keep the backend injected or eject it

If the documentation does not specify a behavior, do not invent it.
