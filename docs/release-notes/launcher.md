# luxe launcher release notes

## 0.0.21

- add new project wizard screen with folder choice
- fix download issue on mac/linux
- refactor everything so we can iterate more

## 0.0.18-20

- add download latest to module list
- add run project button on projects
- add "show remote" toggle to modules
- add search to projects and modules
- add some crash resilience 
- show running state for samples

## 0.0.17

- fix crash when leaving tools + settings page too quickly

## 0.0.16

- fix linux: remove Str.lower on paths (obviously) breaking across platforms
- fix default antialiasing being too high (breaks on linux sometimes)

## 0.0.14

- fix some path discrepancies bugs
- fix new projects not being highlighted correctly
- fix some crashes on agent helper when switching away
- fix missing previews when adding a module to a project
- fix missing previews elsewhere

## 0.0.13

- modules can now specify executables to fix cross platform

## 0.0.12

- initial sample viewer for modules
- display project + sample preview images
- fix multiple outlines in a single version

## 0.0.11

- fixes for renaming user

## 0.0.9 - 0.0.10

- agent fixes for sorting versions

## 0.0.6 - 0.0.8

- add initial agent support 
- download/manage tools are now present when viewing a module
- added more logging for certain issues 
- added settings (currently only one dev related setting)

## 0.0.5

- fix text rendering on some GPUs (ronja)
- latest luxe (2020.2.0)

## 0.0.4

- uses new luxe paths
- fix mac launching editor without opening project
- fix mac status for running editors
- fix string wrapping issues
- fixed missing modules not showing in project edit (brody)
- fixed missing modules using any installed dev version
- fix log locations for editor on mac (right click project)
- update editor/engine list on install/remove, so projects see it

## 0.0.3

- add right click luxe version to set bin path
- add settings menu for set bin path
- fix bin path not activating post install
