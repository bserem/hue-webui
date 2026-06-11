# Prompt Log

Chronological log of the prompts that built this project. Each entry is the
user prompt verbatim; the resulting work is in the matching git commits.

Only prompts related to code itself (`index.html`) are mentioned, to reduce noise.

## 1. Initial build

> I want to make a no dependency, single page html HUE lights control.
> It needs to connect to the HUE bridge, load the lights, ordered by rooms, and have an ON/OFF, Brightness and Color/Ambiance selection using HTML5 color picker.
> The brige IP is 192.168.2.2

## 2. Repo + key management + ordering

> Git init and commit
> Add an option to provide the app-key by hand instead of re-registering to the bridge if using a new browser
> Add an option to re-order rooms and lights in a room
> Add an option to view the app-key

## 3. Renaming

> Add an option to rename rooms/lights

## 4. Sensor readings

> Add motion sensors temperature and light readings in the header

## 5. Master switch + header layout

> Add switch for entire bridge next to 'Hue Control' title
> Place sensor readings next to title and the new button

## 6. Kelvin slider

> Display kelvin for white next to ambient lights, slider step is 100K

## 7. Room toggles

> Add toggles next to rooms

## 8. White mode for color lamps + presets

> Color lamps also need white color temperature, for when set to white
> Ambience and Color lamps need buttons for the following presets: READ, RELAX, CONCENTRATE, REST, COOL BRIGHT and BRIGHT. By Order of Kelvin.
> Rooms need the same buttons

## 9. Documentation

> Save prompt history to PROMPT_LOG.md
> Create CLAUDE.md
> Ammend README.md

## 10. Configurable bridge IP

> Make the bridge IP configurable

## 11. Accordion rooms

> Make rooms accordions, remember open/close state

## 12. Zones

> Add support for zones.
> Makes Rooms/Zones orderable.

## 13. Redesign

> /document-skills:frontend-design

Follow-up fix during review:

> the room toggles don't update the light toggles now

## 14. Bright/dark mode

> Make the page have a Bright/Dark UI mode. Control button on top right

## 15. Show/hide rooms and zones

> Add options under settings to show/hide rooms/zones. 2 checkboxes.

## 16. Export/import config

> Add option to export/import config. Should be available on first run too if no bridge is found.
