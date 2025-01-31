# luxe release notes

## 2024.2.3

This build fixes issues from last build + more big beta todos.

- Color; add Color.hex_string 
- Frame; mark; add a history so it doesn't crash after time
- Text; another color fix
- Mesh; new mesh importer pipeline merged in (to document)
- Render; fix issue with shadows in 3d render module
- Text Style; fix asset types not finding fonts

## 2024.2.2

More work on stabilizing new modifiers and world tick!
The changes to world tick may take time to finalize but 
for most content works as is and shouldn't be visible.

- Render; fix some text color bugs 
- UI; more tweaks on UI layout/auto size interactions
- Outlines; Fix copying .gitignore and .vscode as intended
- Frame; add initial Frame.mark helper for seeing frame timelines
- World; changes in how world tick world
- World; modifiers now use init instead of new() removing need for super()
- Editor; Editor api for accessing editor instance in modifiers
- Text; add initial shadow/outline effects (Ronja)
- Agent; fix some minor issues and auto restart agent as needed
- Assets; fix off by one error by moving generated assets into project
- Scene 2.0; fix scene scripts running in editor
- Scene 2.0; fix scene scripts not running tick
- Scene 2.0; scene scripts have parent class now (no required functions)

## 2024.2.1

Lots of stability and bug fixes on the new modifier workflows, 
scene/prototypes and editor this time around. Not necessarily
fully complete yet, but much closer to parity and much more reliable.

- Scene 2.0; add scene scripts
- Scene 2.0; fix wrong modifier APIs used
- Scene 2.0; add unload for scene/prototype (Entity.destroy(root))
- Text; fix jumpy text
- Wren; allow { on new line in more places
- World; revise how attach/detach and modifier events work
- Assets; fix font face gen not being found on first compile
- Assets; fix crashes when certain apis are used certain ways
- Toggle; add get() api
- Tiles; fix crash in certain cases
- Blocks; several major bugs found + fixed 
- Blocks; fix arrays being incorrect for lx inputs in new data
- Blocks; add destroy
- Engine; clean up wip memory from in dev code
- Engine; fix a couple memory corruption issues

## 2023.11.2

- Outline; fix pixel art project dependency version
- Fix uninitialized crash in new var fields (thanks msminotaur!)

## 2023.11.1

- Outline; add pixel art outline with auto scaling and all that
- Wren; fix a few issues with new fields in completion
- Wren; fix explicit fields not initializing parent class fields
- Basis; add pixelated AA shader + basis for scaling pixel art
- UI; add UIColor control + ColorPicker control

## 2023.11.0

Whew. Important to note that this build was 1.5 ish years of constant use and change,
so the list of changes and fixes is incomplete at best. There's also a lot of 
foundation changes that shift the engine to it's more final form for open beta.

There's a huge list, not all listed here.

Some of these things are still in flight and will land in the next few versions,
but we're switching back to the rapid iterative builds where we have frequent builds instead.

### Asset db
There's a new wip asset database which tracks missing assets or assets moving around, 
and will be able to answer questions about references and lots more. 

Currently the new asset system co-exists with the old one. Some assets are handled
by the old system and some handled by the new. This is all pretty transparent but,
there are things that are less efficient or things that are doing extra work cos of it.
Those will go away as we finish this up, but shouldn't affect your day to day use much.

### New project format
The project has changed shape a bit, there's now a `luxe.project/` folder which 
contains things like the version.lx, modules.lx, ignore and so on. This folder
will be where all project configuration lives and keeps the root clean for your content.

This also makes those things more machine editable, for example you can write to version.build.lx
and it'll automatically be available as a setting `project.build` to display in game.
We use this to write git version hashes into that.

### Rendering and Shaders
The shaders have evolved a lot, make sure if you had project local shaders you look at the 
module shaders.emsl for the new Draw, View, Frame inputs. These change the `input.view.mvp` 
and friends to `input.draw.mvp` and so on. 

Shaders also now send their data as buffers (more modern) which allows a lot of stuff
like compute and other things to use the data more effectively. This is largely transparent for you.

### UI Layout
Controls in the UI now have flex layout built in. You opt in with `UI.set_layout_mode`,
and use `Control.set_behave` and `Control.set_contain` to control the layout.

### Agent + Code completion
The agent has seen many improvements! Code completion, references and jump to
definition should work better now. There are more places with annotations and 
inferred types and more to come.

### Wren
We've been improving the baseline wren as well. There's a new GC that is aware
it's running in a game and does work on a frame level, allowing things to spread 
out and not spike as much. Still wip, but significantly smoother in a bigger game 
like Mossfield Origins.

#### Explicit fields
We've also now got explicit field declarations! 
They go first in a class, must be initialized.

This reduces boilerplate a lot for one thing. 
Before you had to write this for every field:

```js
field { _field }
field=(v) { _field=v }
// ... initialize
_field = 3
```

But now, you can just do this:

```js
var field = 3
```

This generates the get/set for you, but you still have the `_field` though.
It will only generate it if one doesn't exist, so you can override the behavior
and still treat them as regular methods, and existing code is compatible.

```js
class Example {
  var number = 2
  construct new() {
    number = number + 2
    Log.print(_number) //4
  }
}
```

Fields like this can be initialized with any expression since the initializer 
is executed when an instance is constructed. So you can do things like

```js
var entity = Some.code(here)
```

### Random sampling of changes

- Topograph sort service API
- Frame; add Frame.skip(N)
- Render; expose write mask to pass layer
- Input; add display name for keys
- Draw; add plane3D, bounds3D and aabb3D for geo
- Selection; add selection service api
- UI; add wip world inspector control (view scene in game)
- Add fuzzy search for strings
- UI; add filtered list api
- IO; add wip Ptr and Uint64 type
- Str; add format api
- Str; is_alphanumeric
- LX; deterministic key serialization
- Sprite; add create where it'll infer the size from the image
- Wren; var field
- Mac; Universal Intel/Apple Silicon binary
- Mesh; auto instancing, with location and tag options
- UI; wip drag and drop
- UI; send destroy events
- Fix clipboard corruption on some platforms
- Render; vsync setting can be changed dynamically
- UI; make commits more efficient
- Text; add wrap, styles, markup, shaping and more
- Fonts; new font compiler + latest MTSDF 
- Jobs; add new task system (internal atm)
- Render; add writable buffers for compute
- Render; expose compute properly
- Wren; new wip frame based GC with better dials
- Sprite; add set geometry
- UI; add set_system_cursor to control (wip)
- Input; add system + custom mouse cursors on desktop
- Atlas; add rect exists 
- Render; clean up mat4 vertex inputs 
- UI; nicer debug visualization and control over it
- Video; wip simple video player (mpg1)
- Entity; add on create/destroy listeners
- UI; add any_focused query
- UI; Label; add auto size width/height/both 
- Draw; expose mat basis on create
- Camera; add zoom 2d
- Geometry; add get_aabb
- Entity; destroy changed a bit, no defer needed, safe in loops
- Text; add loc to text objects
- Loc; add PO/POT parsing and handling
- Loc; add initial Localization apis
- UI; add localization to label
- UI; fixed control destroy should take children with
- UI; control enter/exit consistency
- UI; fix events being missed sometimes
- Transform; euler<>quat cache for more stable to and from
- UI; move layout to built in
- Draw; add line3d, camera, frustum, plus, plus3d, ring3d
- Camera; add get_frustum
- Render; remove Float3 types from inputs
- Render; Input buffers now use buffer objects 
- Wren; add Log.print(...) instead of System.print
- Wren; add Sys.stackCaller() for debugging
- Wren; renamed System -> Sys
- add IO.date_and_time(from: Num)
- add IO.date_and_time(from: Num, format: String)
- Input; fix graph node events
- Sprite; new sprite incoming
- Sort render geo by basis as well
- Crash; Initial crash handling (windows only atm, wip)
- Plot; add plot service for storing numbers over time
- Toggle; add Toggle service API (multi boolean for e.g pause)
- SVG; add experimental svg -> geo + svg -> image apis
- Triangulate; add initial triangulate helper apis
- Noise; add Noise API with various flavours of noise
- Wren; add orderedKeys to map
- Audio; add playing? query
- Audio; add pause and pause_of api
- UI; Panel; border is now interior to the control
  - search for set border and half the size if breaking
- Transform; fix get_angle2D_world returning local angle
- Mesh; clean up some memory on destroy
- Render; fix headless crashes
- Render; perf; use frame allocator for queue data
- LX; parse errors and surface them (via Result)
- Wren; add Result type to core
- Wren; add Option type to core
- UI; Control; add get pos/size
- UI; Control; add get UI from control api
- LX: use canonical Wren NaN to avoid bugs
- Transform: fix issue with unlink previous parent

## 2022.0.5

This build is largely bug fixes and foundations that aren't visible just yet.

- Engine; fix a bunch of memory leaks and issues 
- Zip; fix unzipping files on linux/mac
- Outlines; new format for outlines which will show up in new build
- Assets; add get_script which can return script code in release builds
- UI; add UIImageFlags for pixelated images or images with mips
- UI; add Control docs (ronja)
- UI; add UITabs control with tabs
- UI; fix Control.destroy_children not cleaning up parent properly (ronja)
- Loc; initial Localization api; Not in use yet
- Physics; add Body3D.get_aabb
- Blocks; BLOCKS BLOCKS BLOCKS
- UI; add UIBlock control for displaying blocks
- World; fix Layer destroy 
- Mesh; move all lods when transform changes as intended
- Render; fix MRT clear color issues
- Render; support PBO style uploads for images
- Render; use scratch memory for submissions instead of allocating


## 2022.0.4

### Sprite Import Autofill  
When importing a sprite, the material and image name will now default to the file name of the imported sprite. This speeds up workflow (thanks miniglitch for the suggestion).

### Asset ID completion
When an API signals to completion that it accepts an asset type, completion can now show the list of assets of that type. All thanks to Ronja.

### Windows unicode path fixes 
If your user name on Windows included a non ascii character it could confuse the path APIs of the engine, causing trouble when trying to install luxe. This should be fixed where any user name or project folder name can work as intended, even if named with emoji etc. Thanks to Cepukka and Nomad_Hermit for debugging!

--- 

- Runtime; fix more bugs in schedules 
- Render; use Uint16 for index buffer replace/update as intended
- UI; fix bugs in destroying ui 
- Mesh; fix first item ignoring transforms in editor
- Values; fix memory issue causing weird bugs
- Transform; add `get_world_matrix(entitym into_matrix)`
- Tiles; add clearer errors for set_visual 
- Draw; path3d + path report errors for invalid point counts
- Sprite; add clearer errors for contains 
- Docs; add pqueue and AnimEvent docs
- Fix \" causing code coloring issues
- UI; tweak contrast of toggle control
- UI; tweak contrast of toggle control
- Input; add `mouse_state_wheel()` query, returns a float2
- Math; add angle2d for signed 2d angles between directions
- Completion; add "asset_type" annotations 
- luxeignore; ignore log files so they aren't loaded as assets


## 2022.0.3

- Save; add initial save system
- IO; image save now has an extra hdr bool variant
- Web; don't do background sleep, it can lock up the browser
- Web; fix IDBFS mounting and access for save data
- Web; template includes fixes for deploying direct to e.g itch.io
- Web; emsdk 3.1.1

## 2022.0.2

- Render; don't skip rendering geometry in multiple layers if found in the first
- Draw; add Draw.ngon/ngon_solid for drawing triangles, hexagons, etc
- Draw; fix a few ring + circle bugs when non-uniform radius
- Transform; add get_scale_world and component getters
- UI; Control; add can_see/can_see_area/can_see_point
- Render; fix bug in layer passes that could cause crashes
- Math; add documentation (ronja)

## 2022.0.1

- Wren; fix some script compiler errors when tabs are present, and other minor bugs
- Mesh; fix some build errors creating new errors
- Mesh; fix texture parsing creating errors 
- UI; add missing UIImage.get_color
- UI; add missing UIButton.get_render_text
- UI; fix weird clipping issue (caused by compiler inlining)
- UI; fix recursive events bug causing sliders to explode
- IO; fix processes sharing stdio handles causing a crash 

## 2021.0.10

#### wren changes

The way errors are handled and printed by the engine/language now allow vscode to _correctly_
jump to the right line when alt clicked in the terminal. Additionally, only the actual error
is promoted to a "problem" in the Problem tab, rather than every line of the callstack. 

#### completion stuff

Ronja has been hard at work making the experience in vscode better for completion.
They're rewritten it to be able to handle inheritance, not randomly silently fail over time,
and a whole lot more. 

There's at least a dozen fixes, make sure you grab the latest agent version when it's available.

#### render changes

A lot of work has been going into improving meshes workflow, like automatic instancing and various features.
These are most useful in 3D but it's worth noting that if you're using 3D and run into issues please report them.
There are parts still undocumented too, since they're new/potentially volatile. If you wanna use them, you know where to ask!

- Project; add `postdeploy()` hook in project.luxe (for web build templates)
- Input; add `set_mouse_pos(x: Num, y: Num)`
- Input; add `set_mouse_visible(state: Bool)`
- Draw; add initial `path3D(context, points, style)` for 3D lines
- Draw; add alpha to `PathStyle`
- Color; fix bug in hsv conversion (ronja)
- Color; add `hex_color(color: Color, include_alpha: Bool)` to get hex int from color (ronja)
- Color; add `Color.pink` for the luxe pink color
- Math; add `Math.approx` for comparing float numbers
- Build; fix asset extensions causing issues with non-luxe assets with the same extension
- Build; Mesh; fix compiler error when trying to report a mesh error
- Build; Mesh; fix material imports from assets (for later importing)
- World; Mesh; add initial instanced flag for automatic mesh instancing
- World; optimize transform change event sets from allocating every time
- Render; optimize image input setting in backend
- Render; optimize render passes a bunch
- Render; add "replace tags" to material basis for direct pipeline control 
- Render; add `engine.render.metrics.gpu` setting, exposing the opengl GPU timer
- Runtime; fix crash when things queue something during shutdown
- Runtime; emit `engine.ready` for scripts opting into ready from elsewhere
- Runtime; add `debug` section in frame after `visual`, before `end`
- Runtime; logs are rotated for log.txt, log.1.txt etc instead of overwriting each time
- Runtime; fix tick functions being rearranged
- Runtime; exposed logs to user facing code for display
- Release; disable windows console window in release builds
- IO; regex now allows 32 groups/sub matches (was 16)
- IO; fix missing image_failure_reason() endpoint
- IO; work on UV backed process API, not finished
- UI; add `UI.set_layout_mode(mode: UILayoutMode)` to opt into flex layout
- UI; add wip `luxe: ui/field/number` for consistent number fields with precision radial
- UI; add wip `luxe: ui/field/vector` for consistent float2/3/4 fields using number fields
- UI; add wip `luxe: ui/field/choice` a filtered dropdown list (wip name)
- UI; add wip `luxe: ui/field/path` a path selector field
- UI; display control + id + type below mouse in debug vis
- UI; add `UI.mouse_to_canvas(ui: Entity, x: Num, y: Num)` for Input -> UI conversion
- UI; add `UI.set_debug_mode(mode: UIDebugMode)` for individual debug vis
- UI; Panel; add missing `UIPanel.get_color(panel: Control)` API
- UI; Slider; now emits change/commit events for drag vs release 
- UI; add missing `UI.has(entity: Entity)` API
- UI; optimize UI drawing creating a new material each time, reuses them
- UI; make layout part of the UI system itself, and allow partial layouts
- UI; layout now contains UIContain.vfit and UIContain.hfit to explicitly ensure size to children
- UI; fix children_at_point behaviour for controls outside of parent
- UI; fix marked control not getting reset when leaving canvas (nihilocrat)
- UI; fix text change events sending previous text as well
- UI; fix text double click time counting when unfocused
- UI; fix invalid ui crashing the events processing (e.g destroy a ui from an event)
- UI; redo checkbox visual behaviour
- UI; add quad_detailed to ui draw
- Lists; remove_where now returns the value like the rest of wren (non breaking change)
- API; remove old `luxe: array` - replace with `luxe: containers`
- API; remove old `luxe: geometry`, no longer used (moved to old lines sample)
- Wren; make error messages line up when using tabs (ronja)
- Wren; fix blank wren files erroring as not found
- Wren; fix missing debug info for class locations (ronja)
- Wren; work on exposing the debugger, use `--wren-debugger` to enable
- Wren; work on vm level profiling built in (not exposed yet)
- Wren; add line + module to runtime errors
- Wren; change how stacks + errors are reported for vscode
- internal; fix imgui implementation (not exposed yet)
- internal; significant work on the new blocks (not user facing yet)

## 2021.0.9

- render; allow setting depth_write/depth_compare for a pass layer
- render; fix rendering to array textures
- world; ui; error on cancelling events without a ui attached

## 2021.0.8

- materials; add error when specifying a missing input to a basis for debugging
- render; add `Render.valid_text(text)`
- render; cache render passes by id to avoid create materials infinitely
- astar; add a second infinite loop guard for rare edge case (couldn't find why)
- window; don't sleep at all if background time is set to 0
- ui; control; add get/set_enabled
- ui; disable "last line doesn't justify" behaviour of layout
- web; fix emscripten preload cache being aggressive
- draw; draw text returns render text object as intended
- draw; fix circles going one step too many
- IO; add `IO.date_and_time()` for printing current time
- fix crash when removing a tick from inside a tick
- fix assert on Entity.set_name when name isn't a string
- potential fix for unicode names being weird on windows
- io; return "web" for IO.os() on web target
- tiles; error when trying to create a tile without a Tiles modifier
- world; prototype now errors when asking on non prototypes

## 2021.0.7

- Fix regression from imports added in 2021.0.6, paths should work as intended
- UI; double click setting added `engine.runtime.input.rapid_press_interval`
- UI; double click time default increased to 0.45s (was 0.2s)
- Anim; Sprite Value Track now exposes uv.horizontal/uv.vertical
- Anim; Sprite Value Track now exposes uv.left/uv.right/uv.top/uv.bottom

## 2021.0.6

- mac; fixes for build deployment target (now 10.13)
- Str.path normalizes drive letters for windows paths
- outline; fix tutorial background sleep, add logo for clarity
- UI; control; move tab concerns to control level so it works everywhere

## 2021.0.5

- latest SDL - 2.0.14
- latest emsdk - 2.0.25
- Draw - fix rect_detailed for y+ up and clamped radius
- Draw - add rect detailed example/test to sample
- LX - fix bug serializing vectors of bool

## 2021.0.4

- Fix Assets class being special, so now it gets proper completion
- Docs + types for code completion for several classes
  - Assets, Anim, Assert, Strings, Audio, Color
  - modules: containers, draw, editor, events, game, id, sat2d
- UI - add commit event to text fields
- UI - text field now cancels input on escape, commits on enter, and can tab forward
- UI - recursively handle UI events properly, so no events are lost
- Sprite - in editor, update size on changing material automatically
- Assets - import for sprite shows warnings for existing content not errors
- Transform - add `set_snap(x, y)`

## 2021.0.3

Minor breaking change: If you had a constructor that returns any value (including `this`), that's invalid.
Often you can just convert to `static new() {}` or similar and have an internal `construct` method instead.

- `Str.fixed` now uses a reliable format where 0.1 remains 0.1 instead of 0.1000001231
- add `Str.vec` which formats a vec using Str.fixed
- `LX` now stringifies numbers using `Str.fixed` with 6 digits precision (:todo: configurable)
- Wren - clean up attributes spam
- Wren - latest version of 0.4.0 https://github.com/wren-lang/wren/releases/tag/0.4.0
- Wren - fix crashy behaviour in much bigger projects due to unitialized value
- Wren - fix newlines being different based on file newlines within strings
- Anim - Sprite frames track resets material on reset
- Anim - Sprite frames track now sets the material if it's not a match, as intended
- Anim - Sprite frames track allow inverted ranges for start/end index
- Anim - Sprite values track now allows animating sprite properties as a curve
- Anim - Transform - relative animations now work as intended
- Anim - add `set_interval_time`, sets the time in 0...1 range for a live anim
- Anim - add `track_get_range`/`track_set_range`, sets the range for a track in a live anim
- Anim - `play`/`play_only` now error properly if you pass a null asset id
- Anim - fix crash when animation modifier accessed an old entity
- World - when requesting a system, return a new one if not present (instead of error)
- Transform - fix some ways to set a position not respecting snap
- Transform - fix unlink without a parent asserting, error properly instead
- Transform - fix linking to self being an infinite loop + crash, error properly instead
- Transform - add experimental `get_pos_world_unsnapped`
- Runtime - add `engine.runtime.quit` setting, if false, quit will do nothing and fire an event
- Render - add error for missing material basis on `Material.create`
- UI - add `UI.make` experimentally (it's like spawn but returns a properly sized container)
- Sprite - initial annotations for completion/docs
- Math - add `dir2D(pos, target)`

## 2021.0.2

- Windows - fix resize/moving a window from blocking the game
- Wren - add annotations (available at runtime as SomeClass.meta)
- Wren - add raw string literal """like this""", can cross lines and include anything
- Wren - add list.remove(item)
- Wren - add experimental way to call a function by name
- Transform - fix bugs due to use of euler in animation track
- Transform - add global default snap setting e.g pixels `engine.transform.snap = [1,1,0]`
- Transform - experiment with applying transforms on spawning prototypes more
- World - clean up some memory properly
- IO - fix creating paths across drive boundaries, fixes launcher
- LX - add raw string support to lx files
- LX - escape some json content correctly when saving json
- Mesh - fix a bug where changing the mesh asset could crash if done a certain way
- Runtime - queued ticks will process recursively queued ticks correctly now
- String - add indent(string) and indent_strip(string) for convenience
- Outline - add vscode default tasks to empty outline
- Tiles - add Tile.get/Tile.set for storing values on a tile
- Color - add hex(value, alpha)
- Result - add Result type experimentally
- World - modifiers have find_entity(relative_entity, uuid) for finding entities
- Camera - if no default camera exists when one is created, set it as default
- UI - fix obscure timing issue with dead controls being marked
- UI - add `Control.destroy_children(control)`
- Wren - fix infinite loop in some parsing cases
- Wren - error on missing imports up front
- Wren - warn on single quote strings
- Wren - warn when imports mismatch case
- Math - vector angle between `angle(v1, v2, up)`
- fix some dev mode issues
- fix crash when asset reloading fails to compile
- add debug file to find issues in lx parse till there's real errors

## 2021.0.1

- UI - fix nasty text focus bug (eduardo, bach)
- UI - when setting text, error properly on not-a-string
- Render - add atlas API and assets for atlas support
- Sprite - add atlas support via Sprite.create(entity, atlas, atlas_image_id)
- Sprite - add origin setting (0.5, 0.5 center default. 0,0 is bottom left)
- Anim - Sprite track now has proper frame based information instead of keys
- Bytes - subscript [] returns uint8 (instead of int8) as intended
- Render - add window_hide(state)
- Render - add `engine.runtime.window.allow_screensaver` to not block
- Render - add VertexInputRate for per instance vs per vert
- Render - add get/set_instance_count for geometry
- Shaders - add pixel perfect aspect upscaling shader for outlines
- Shaders - add input.view.time to shaders
- World - add proper errors on invalid material for Sprite
- Samples - add previews and sample.lx for new launcher
- Deploy - add experimental custom icon support + a default icon
- Compiler - print success status
- Docs - work on tutorial

## 2021.0.0

- Fix major font rendering bug.... 
- Fix filewatch again
- Scripts - add an error for missing imports
- Game - add `Frame.schedule` for calling functions in future, absolute time
- World - add `World.schedule` for calling functions in future, world relative time
- World - add Camera.cull and some tools for frustum culling when rendering
- World - add World.render with a second camera to cull with (can be same as render cam)
- World - experimental render view descriptors
- Settings - add `has` query
- UI - add `UIList.can_scroll`
- UI - add `UIWindow.get_collapsed`
- UI - fix some event ids being stomped
- UI - fix events being sent to parents even if they didn't want them
- UI - visual polish for several things
- Math - add `dist2D` for non vector
- Anim - transform tracks now have an absolute flag
- Anim - fix reverse rate not working as intended. reverse now works again
- Material - add depth bias setting to basis
- Expand color class with some constants, hsv functions and lerp
- Render - explore plural tags for render layers
- Regex - fix bugs and update lib for utf8 support + for substitutions 

## 2020.3.5

#### stability fixes

There were a few vague crashes that happened occasionally when running a (bigger) project, and re-running normally worked.
This was introduced not long ago by mistake when I was testing the new native lx parser. 

All instances of weird behaviour have been found and fixed, so things are back to being stable + reliable again.
I've also optimized a couple places for script compilation to be much more snappy when iterating.

#### lots of physics work

For 7DFPS I was making a physics game, that necessitated a bunch of work on physics.
Instead of doing the minimum, I took the opportunity to implement the intended filtering + collision model,
add collision callbacks, physics materials, and body settings assets.

This filtering model is based on "ignore/overlap/collide" being the 3 settings for a body. 
When two bodies overlap, the "least" type is used. e.g ignore vs collide = ignore. overlap vs collide = overlap.
This means any body can be collidable (solid) or overlapping (triggers, etc).

The callbacks send events for overlap `begin`, `active`, and `end`. 
A collide event is also sent if the result is `collide`.

These are only applied to 3D at the moment, but the same code + model applies to 2D and will make it's way there soon.

#### lots of transform work

Similiarly, I needed some special transformation stuff, so instead of just adding the one I needed I added a
lot of the fundamentals for dealing with transforms. This is still based on the idea that transforms should be
possible to use without resorting to math constructs like matrix or quaternions. Lot of progress here!

There's lots to see otherwise: 

- Window - Fix fullscreen to be borderless windowed, add alt+enter handling
- Scripts - optimize recompilation a lot for faster iteration
- Wren - 0.4.0 - changes here - https://github.com/wren-lang/wren/releases/tag/0.4.0-pre
- Wren - fix corruption in GC due to an unmarked value I added
- Wren - fix potential access of uninitializd memory
- Wren - fix corruption in GC due LX parsing - https://github.com/wren-lang/wren/issues/869
- Draw - add `Draw.cross(context, x, y, z, radius, angle, style)`
- Draw - add simple ring variant `Draw.ring(context, ox, oy, oz, radius, smoothness, style)`
- Math - Add `Math.length(vec)`, `Math.length_sq(vec)`, `Math.length_sq2D(vec)`
- Math - Add `Math.angle(vec, other_vec)`
- String - fix `Str.fixed(string, precision)` affecting the number
- Assets - Meshes now auto triangulate, so no need to triangulate inputs
- Assets - Mesh compiler prints more useful nested info for choosing a node
- Physics 3D - fix bugs when editing physics bodies in editor (tilman)
- Physics 3D - add body config asset
- Physics 3D - add collider materials asset
- Physics 3D - fix mesh collider when meshes were indexed
- Physics 3D - fix mesh internal seam collisions for smooth collisions
- Physics 3D - add mesh MeshColliderType for controlling behaviour
- Physics 3D - add callbacks for overlap + collide events
- Physics 3D - add initial wip filtering for collide/overlap/ignore
- Physics - Fix debug vis in editor
- UI - add `UIText.select_all(control)`
- Transform - fix look_at when linked to another transform (tilman)
- Transform - fix bugs in world rotation setting (tilman)
- Transform - add `rotate_around(entity, x, y, z, axis_x, axis_y, axis_z, degrees)`
- Transform - add `rotate_around_world(entity, x, y, z, axis_x, axis_y, axis_z, degrees)`
- Transform - add `world_point_to_local(entity, x, y, z)`
- Transform - add `local_point_to_world(entity, x, y, z)`
- Transform - add `local_dir_to_world(entity, x, y, z)`
- Transform - add `world_dir_to_local(entity, x, y, z)`
- Transform - add `local_vector_to_world(entity, x, y, z)`
- Transform - add `world_vector_to_local(entity, x, y, z)`
- Transform - add `set_rotation_slerp(entity, from, to, t)`
- Transform - add `set_rotation_slerp_world(entity, from, to, t)`
- Transform - add `set_rotation_slerp_angle_axis(entity, axis, from, to, t)`
- Transform - add `set_rotation_slerp_angle_axis_world(entity, axis, from, to, t)`
- Transform - add `rotate_angle_axis_slerp(entity, axis, angle_amount)`
- Transform - add `rotate_angle_axis_slerp_world(entity, axis, angle_amount)`

## 2020.3.4

- Runtime - allow background_sleep to affect headless apps
- World - add Scene.entity_forget for moving entities around manually
- UI - don't abort on > 1000 events (can happen when file dialog is up)
- Compiler - add transient concept for compiling transient data in editor

## 2020.3.3

- Wren - add continue keyword to use in loops
- Wren - skip type-style annotations in several places
- Wren - method arity checks are now reported as an error where possible
- Wren - method max parameters is now a compile time check
- Wren - improvements to the script check parsing + graceful failure
- Math - `random_point_in_unit_circle` streamlined by ronja
- Input - fix `Input.set_mouse_capture` to work as intended across platforms (tilman)
- Input - add `Input.mouse_x_rel()`/`Input.mouse_y_rel()` for capture use
- LX - add a stringify to json flag for strict json output
- UI - fix crash when control is destroyed while another is captured (ronja)
- UI - fix events not being cancelled for listeners (ronja)
- UI - fix a rogue wrenalyzer complaint about UIWindowChange (ronja)
- Assets - auto generated images have a `generate_mipmaps_at_load` added
- Camera - set2D/set3D for less confusion (set2D uses x/y/w/h and converts internally)
- Render - Resource IDs like images are passed to debug tools (e.g renderdoc)
- Render - Fix material override bug being wrong on web
- Render - Refactor FBO handling to be more strictly conformant (webgl2)
- Render - fix some issues running headless builds
- IO - fix http server issue on some platforms (tilman)

## 2020.3.2

- IO - add logging for failure to create directories
- Anim - fix validation of animations so script errors happen (eva)
- UI - fix destroyed controls not updating marked state (ronja)
- UI - fix relative mouse offsets being wrong (ronja)
- UI - fix crash when marked update finds destroyed controls
- UI - custom events now have proper cancellable IDs and can be cancelled
- UI - fix bug where captured controls lose events due to new marked fix
- UI - fix scroll areas not updating when resized, they do now
- Render - fix bug in opengl backend causing many sprites to fail (jonathan)
- Transform - add change callbacks `listen(entity, fn)` / `listen_all(world, fn)` 
- World - add `World.valid(world)`

## 2020.3.1

- Wren - Allow newline before a dot so you can do fluent syntax
- Wren - List now has `indexOf(item)` and `swap(index, other)`
- Wren - Num now has `var max = 3.max(7)`, `.min(other)` and `num.clamp(0, 1)`
- Astar - add 2D pathfinding API 
- Anim - fix crashing when the entity an autoplay was queued for is gone
- Settings - fix string settings being stored as bool (??)
- luxe: pqueue - add MinPQ and MaxPQ priority queues
- Lists - `binary_search_first`, `remove_where`, `index_of_where` args
- Render - add vsync flag (`engine.render.vsync` now works)
- Render - pass layer can now use a basis
- Render - pass layer can now access `.dest` directly
- Render - add `Render.submit_fn`
- Render - backend fixes for some incorrect strings (shader errors)
- Render - fix important viewport bug 
- Render - can now correctly render to cubemaps
- Render - can now correctly render to mip levels of images
- Render - add missing `Image.destroy` and `Image.valid`
- Render - add `view_inverse`, `proj_inverse`, and more to `View` inputs
- Runtime - filewatch fixes + also now respects dupes, ignores for N frames
- Runtime - fix a bunch of plugin bugs while testing a rust plugin
- Input - fix `set_mouse_capture(state)` capturing cursor like FPS games need
- World - add optional `into` arg to `World.world_point_to_view`
- World - add `World.set_default(world)`/`World.get_default()`
- World - add `Modifiers.has_system_in_world(modifier_id, world)`
- World - wip: `Entity.valid` checked for modifier tick before sending
- World - Transform now has `sync_world(world)` and `sync(entity)`
- UI - Fix custom events firing incorrectly (ronja)
- UI - Fix Text taking focus when things are above it (like editor scene buttons)
- UI - Fix text rendering (button/window/text/label) being delayed by a rendered frame
- UI - querying label metrics is now accurate at all times (brody)
- UI - Fix layout bugs (update recommendation from layout dev)
- UI - Fix enter/exit only happening on mouse move, and exit other bugs (ronja)
- docs - fix how to for `random` (jonathan)
- docs - document `Assets` API
- docs - document `luxe: containers`/`Lists`/`MapOrdered` API
- docs - document `luxe: draw`/`LineCap`/`LineJoin`/`PathStyle`/ API
- docs - document `luxe: editor`/`Editor` API
- docs - document `luxe: events`/`Events` API
- docs - document `luxe: game`/`Frame`/`Ready` API
- docs - document `luxe: id`/`ID` API

## 2020.3.0

Time for some clean up, yay project changes!

#### new game class

In it's simplest form, a game class
before 2020.3.0 looked like this:
```js
import "luxe: game" for Game
class game is Game {
  construct ready() {
    System.print("ready!")
  }
}
```

This has changed to the following, as a more final form. 
Notes: the `super` is required, the base class is now `Ready`, 
       and the actual class is called `Game`.
```js
import "luxe: game" for Ready
class Game is Ready {
  construct ready() {
    super("ready!") //or just super() is fine
  }
}
```

#### New project class
Similarly, the project class has been finalized too.
Before 2020.3.0:
```js
import "luxe: project" for Project
class project is Project {
  construct new(target) {}
}
```

And after: The base class is `Entry`, the actual class is `Project`, 
and the constructor is called `entry` too.
```js
import "luxe: project" for Entry
class Project is Entry {
  construct entry(target) {}
}
```

#### Access your Game class
You can now reach the live instance of your game class anywhere.

Just use `import "luxe: game" for Game`, this variable contains your game instance.
For example, `Game.app` would be accessible from other scripts now.

#### default input maps
Previously input maps weren't loaded by the editor and the template didn't have one.
This is better now, if you set `engine.input.entry` in your entry settings file,
then the editor + the game will automatically load this input config for you. 

This means for example, previewing a scene in editor would work with the arcade module again,
since it used input maps for input but there wasn't any loaded before.

#### New main loop
**Renaming**: Note that `World.tick_world` was renamed to `World.tick`, 
and `World.tick_systems` was removed. We just use `World.tick` now.

The main loop is getting closer to done too, so you now have access
to it from anywhere and can schedule callbacks within it. 
You can access it via `luxe: game` for `Frame` atm. 
The system is a work in progress, but useful so far.

The loop works based on sections, and at the moment those are predefined by luxe.
They are `begin, init, sim, visual, end` in that order. Sim being simulation, as in the update function.

You can use `Frame.on`, `Frame.before` and `Frame.after` to place callbacks in or around
a section. These ones are recurring callbacks and happen every tick, they return a handle 
that can be used with `Frame.off` to remove them.

The other ones are once off callbacks.
`Frame.queue` will run the callback at the end of the section that's running.
`Frame.next` will run the callback at the start of a frame (before any sections)
and `Frame.end` will runt the callback right at the end of the current frame (after all sections).

As an example, empty project outline and samples now use it to update and render worlds.
**Note that you don't need to change anything** in your project, it should still work as before even if you don't use this new form.
```js
//update our worlds
  Frame.on(Frame.sim) {|delta|
    World.tick(_world, delta)
    World.tick(_ui_world, delta)
  }
//render our worlds
  Frame.on(Frame.visual) {|delta|
    World.render(_world, _camera, "game", {"clear_color":_color})
    World.render(_ui_world, _ui_camera, "ui")
  }
```

#### Animation events
Animation tracks now have events, default ones, custom ones and a tick event.

The default events are `start` and `complete` which fire when an animation
begins playing and when it ends (via stop or otherwise). Note that these are 
for the entire animation, not a particular track.

For tracks, if you set `tick_event = true` in your anim asset for that track, 
it will fire an event every time the animation is updated. This means you can
now use an animation track to drive other things directly. Example below.

The track also can have a list of custom events, which behave like keyframes.
In this example, the animation is 0...1 on the timeline, but in the middle and quarter
of the way through, a custom event will get fired each time that time is crossed.
This let's you respond to keyframes directly, such as playing sounds or effects at key points.
_(note that an audio track is still coming, but this means you can use them already)._

```js
events = [
  { time=0.25 event="quarter" }
  { time=0.5 event="middle" }
]
```

**Listening to the events**   
The event handler is set on an animation instance, one that is returned from `Anim.play`.
In the example below, the animation is fading in a mesh, but I'm using the tick event to also
scale the mesh based on the values in the animation curve. 
You can see it in action here: https://i.imgur.com/uYHNbOS.mp4
```js
var anim = Anim.play(entity, "anims/example")
Anim.on_event(entity, anim) {|entity, anim, time, value, track_type, track, event|
  //event will be `tick`, `start`, `complete` or a custom value.
  if(Strings.get(event) == "tick" && Entity.valid(entity)) {
    var scale = 9.5 + (0.5 * value)
    Transform.set_scale(entity, scale, scale, scale)
  }
}
```

#### UI Layout API

There's a new modifier for doing dynamic layout with UI.
It's documented in the [layout tutorial here](../learn/ui/basic-layout.md).

#### Material changes
The format of the material basis has changed a bit. 
Some of the changes are a warning and some are an error.
You can look at the default assets to see more examples, but here's the changes:

Write mask used to be a string, and is now a map, like `write_mask = { red=true green=true blue=true alpha=true }`. Any values left out will mean false, so `write_mask = { red=true }` will write only to the red channel. 

`samplers` have been removed, images are now a regular input and not in a separate section. This affects the basis file (`.material_basis.lx`), the material instances (`.material.lx`) and the `Material.set_image`/`Material.get_image` API. 

Images are accessed by name (not index like before), like other material inputs, so the API is now `Material.set_input(material, "sprite.image", image)` and `var image = Material.get_input_image(material, "sprite.image")`.

Before (this is from `sprite.material_basis.lx`):

```js
samplers = {
  sprite.image = {
    index = 0
    type = "image2D"
    sampler_state = "linear_repeat"
  }
}
inputs = {
  sprite.uv = {
    type = "float4"
    value = [0 0 1 1]
  }
  ...
```

After:

```js
inputs = {
  sprite.image = {
    type = "image"
    value = {
      type = "image2D"
      sampler_state = "linear_repeat"
    }
  }
  sprite.uv = {
    type = "float4"
    value = [0 0 1 1]
  }
  ...
```

That's the basis, this is the material instances (this is from `logo.material.lx`):
```js
material = {
  basis = "luxe: material_basis/sprite"
  samplers = { 0 = "luxe: image/logo" }
  inputs = {
    sprite.color = [1 1 1 1]
    sprite.uv = [0 0 1 1]
  }
}
```

After:

```js
material = {
  basis = "luxe: material_basis/sprite"
  inputs = {
    sprite.image = "luxe: image/logo"
    sprite.color = [1 1 1 1]
    sprite.uv = [0 0 1 1]
  }
}
```

- docs - add wren intro
- docs - expand rendering concepts page
- docs - add UI basics + custom controls tutorial
- docs - document `UIEvent` 
- input - add `all` node for convenience
- project - fix .luxeignore folders, `path/` now ignores path
- project - project class refactored and cleaned up
- project - added injected settings to entry settings 
- project - magic settings: `project.name`, `project.version`, `project.package`
- project - magic setting `project.build_type`, one of `dev` or `release`
- outline - add default input map so e.g `arcade` just works
- samples - clear out old samples that were wrong, update ones that stay
- API - block in `Set` concept which will get used soon in more places
- game - add `engine.input.entry`, entry input asset to load
- game - main loop now running as sections with ranges + priorities
- game - main loop now accessible via `Frame` in `luxe: game`
- game - add `Frame.next`, `Frame.end` and `Frame.queue`
- game - game class refactored and cleaned up
- game - `luxe: game` for `Game`,`Renderer` for access from anywhere
- build - cache binary hash of binaries, so new builds trigger rebuilds
- world - renamed `World.tick_world` -> `World.tick`
- world - remove `World.tick_systems`, just `World.tick` is enough
- world - `Scene` scripts use `new(world)` + don't take an arg from `Scene.load`
- world - add `World.clear(world)` which empties the world
- world - fix bug in `Entity.create` causing an internal assert (Whuop)
- tiles - add `get_color` and `set_color` per tile, in data + editor as well
- render - fix missing arg in Image.redefine (Ronja)
- render - material now has stencil refs (basis->material->geo)
- render - text now uses an array of images for fonts
- render - fixed bug with text bounds + vertical align being inverted
- render - mesh data in assets can specify stencil refs
- ui - add UIWindowChange for window change events 
- ui - add layout helper (code only atm)
- ui - removed 'root' concepts in rendering, fixing several bugs + more efficient
- ui - `Control.set_events` is now plural, returns ID for `Control.unset_events`
- ui - fix `enter` and `exit` meaning something different, now works as intended
- ui - fix scroll handles stealing focus even when not visible (Ronja)
- ui - add `UI.set_material_basis(ui, solid, text)`
- strings - removed old `Glob` API (use `IO.wildcard_match(needle, haystack)`)
- material - shader library can now be specified per stage
- material - remove old samplers concepts + API (`Material.get_image/set_image`)
- material - write mask now a map `write_mask = { red=true green=true blue=true alpha=true }`
- anim - tracks now have custom events `events=[{ time=0.5 event="event" }, ...]`
- anim - anim now fires `start` and `complete` events on begin/end
- anim - use `Anim.on_event` + `|entity, anim, time, value, track_type, track, event|`
- log - fix bug in log error checking that caused an SDL_RW error on log.txt
- wren - add `Num.tau`, on instances: `num.max(val)`, `num.min(val)`, `num.clamp(min,max)`. 

## 2020.2.0 

Because of breaking changes, it's time for 2020.2.x
Note that there may be a few more changes related to the same work in 2020.2.x, 
it will always be clear what is changed.

#### UI Controls  
All UI controls (except Control itself, for now) now have 
the UI prefix for consistency. That means `Button` becomes 
`UIButton`, `Panel` => `UIPanel` etc. 

#### Image API
Previously the Image API was minimal and tucked under `Render.image_*`.
`luxe: render` now has a proper `Image` class with a bunch of queries,
a destroy function which was missing, and a `redefine` function, for
resizing or recreating the same handle under a new ImageDesc.

#### Transform API
Some of the 2D focused APIs, namely `Transform.set_depth` changed to 
`set_depth2D`, as well as `get_depth2D`. `set_angle2D` now has `get_angle2D`,
and in both cases a `_world` variant for world vs local space exists too.

- paths - fix for windows mklink needing root first (bin link)
- Added 2D depth + angle get/set for world + local
- modifiers tick in order of their priority (tilman)
- material inputs fix potential bug for defaults
- UI controls renamed
- UI canvas now requires a camera (technically was optional before)
- UI - text was flickering weirdly on some GPUs (ronja)
- UI - renamed entity arg to ui_entity for clarity

## 2020.1.2

- wren - fix some stack issues causing false script errors
- modules - fix some crashes in weird old code
- filewatch - fix some crashes due to pulling the rug on uv
- filewatch - paths normalized for consistency
- filewatch - fix bugs in add/remove e.g (bool same = strcmp(a,b))
- docs - document assert usage
- docs - update some sprite usage docs for clarity (eduardo)
- Lists.add_unique now returns true/false (true if added)
- ui - label now has color_hover options
- lx - add apply

## 2020.1.1

- block in Render.submit_now()
- Str.path uses native path normalization for consistency
- new luxe root folder support
- fix newlines in descriptions for modifiers
- `luxe: events` now uses proper hash combining
- webgl1 support fixes, including new shader support

## 2020.1.0

Another big release, lots of changes not listed likely...

#### new versioning
The new 2020.x.x format is in use. 
luxe 2020.1.x is compatible with editor 2020.1.x, 
and will generally be stable or minor changes.
This means any 2020.1.x update is safe, 
where 2020.2.x might change bigger things.

#### new project.modules.lx
This file tracks dependencies and metadata, including
the version of luxe your project is using.

#### latest wren
Previously luxe was a few years behind wren for reasons.
It's now using the latest version.

#### new prototypes
The new runtime based prototypes are online and 
mostly working, variants too. They're editable in the editor
and can be spawned and created from there. Modifiers can be 
attached to instances once spawned, and overrides applied.
There's also a newer api for dealing with the inner elements.
Prototypes are now also an entity that is returned.

#### Y+ is now up in 2D
Matches with 3D, fixes math to be consistent with trig functions,
makes animation curves not upside down, and much more.
Note that UI is still y+ down from top left origin.

#### updated samples
Many samples were migrated for Y+ up and moved to system tests.
This means they're a bit easier to read and understand, 
have a readme and more. The sample template is now 
also part of the API meaning samples share the same code.

#### `_luxe.data` is gone, `.luxe` is used
Delete `_luxe.data` from any project, it will error on random issues.


#### changes
- audio - latest soloud
- script - fix clang windows + computed gotos (faster)
- script - fix memory leak on certain (failed) file reads
- script - fix leaks from scripts loaded to wren
- assets - fix size limit of 2gb on parcels
- lx - API usage now tracks source names for better errors
- lx - fix keys with dots in working with get/set/remove
- lx - now using native LX parser, much faster overall
- io - add time_since_epoch_utc
- io - add hash file
- events - add clear_all for tag
- spaces - in user names shouldn't break anymore
- shaders - add ref keyword
- text - fix many alignment and layout bugs
- anim - fix memory leaks on certain tracks
- anim - interpolation is now per key, fallback to track
- anim - layers -> tracks
- anim - fix autoplay happening too early, fixes crashes
- anim - fix major bug on anim stop, affecting other anims
- tags - fix bug when removing tag that doesn't exist crashes
- draw - added rect detailed
- draw - circle conventions made consistent
- draw - fix circles having off by one error
- draw - ui - text now uses unique material, so no glitching
- transform - fix bug where linked entities stayed on destroy
- transform - add get_link/get_linked
- transform - add get/set world component wise
- transform - add set_euler_*_world
- transform - add get_euler_*_world
- transform - links in data are now uuid, not name
- transform - initial snap setting (for pixel snap, set to 1)
- UI - added draw rect detailed
- UI - add render mode settings (in world vs to image)
- UI - now works as intended in world space automatically
- UI - control/scroll/list: add get index for control
- UI - fix bug when control is invalid breaks further events
- UI - add window text/title size, redefine set_collapsed
- UI - fix dead entities existing in the UI <-> entity map
- UI - fix entities not destroying UI on destroy
- UI - fix render data cleanup on destroy
- bytes - add BytesWriter
- outlines - empty - add cameras to defaults
- deploy - fix luxe path when version is dev
- project - use **project.modules.lx** for dependencies
- project - now uses native wildcard, speeds up a lot
- `luxe: array` - renamed to `luxe: containers`
- Lists - add prepend/append
- added `luxe: test`, initial unit testing basis
- modules - version solver integrated (not active yet)
- text - Text.get_extents(entity) (no args)
- text - add get/set material
- modifiers - fix bug where path doesn't exist on create (tilman)
- modifiers - fix cached build state being wrong, now reliable
- modifiers - blocks are now refactored for the new workflow
- string - add quick wrap() helper
- string - add path_is_absolute
- input - add Input.deadzone(x, y, zone) for convenience
- scenes - fix multiple modifiers of same type in many layers
- scenes - fix compiling scenes from modules
- shape2d - add destroy
- shape2d - add get_bounds
- phys3d - add capsule shape
- phys3d - add cast_shape
- phys3d - add set_allow_rotation
- phys3d - add query_sphere and query_box
- phys3d - fix major bug in debug mode vs release (scaling)
- tiles - add get_all
- tiles - add get all with visual
- tiles - fix bugs with pairing functions on web target
- tiles - fix bug with tile not changing
- values - add has_key
- values - fix editor changing values
- values - add clear
- math - add random_point_in_unit_circle
- math - add smoothstep, smootherstep, weighted_avg
- math - add map_linear, nearest_power_of_two, within_range
- math - add wrap_radians, wrap_angle, lerp_angle, angle_delta
- render - add trilinear sampler states
- render - add material input blocks
- render - refactor material inputs for consistency and usage
- render - render pass layers now use material inputs (breaking)
- render - layers + images now have a display_id (renderdoc)
- image - add generate_mips flag in image.lx
- image - load before basis since they can be used as defaults now
- settings - load settings before game.wren
- build - fix script compiler error about a newline after a dot

## 1.0.0-dev.86

- tiles - fixed crash on querying empty tilemap for tags
- tiles - fixed crash in removing tiles in editor
- script - allow release builds access to modules opt in

## 1.0.0-dev.85

- io - added file watch events on desktop
- log - try more resilient log writes over cwd
- log - add log level control from entry settings
- settings - add Settings.apply
- render - blocked in image redefinition for later
- world - add `Scene.entity_list`
- world - add `Prototype.entity_list`
- modifiers - require 'class' field in modifier.lx
- modifiers - prevent using keywords in fields (tilman)
- modifiers - fix bugs in modifiers with booleans
- moduler - disable old auto download properly

## 1.0.0-dev.84

- docs - new docs for `Sprite`, `Anim` and more.
- settings - add background_sleep setting to window
- debug - fixed wren debugger on linux
- sat2d - fix returns no overlap on missed circle collision
- render - fix crash accessing out of bounds images
- tiles - add Tile api for set/get tile size
- tiles - fixed internal data desync as depth/coord change
- wren - fixed += etc not being allowed by script compiler

## 1.0.0-dev.83

- web - web deploy should work again
- world - Scene, Layer, Prototype now exist (not on World.*)
- anim - renamed animation layers -> tracks
- text - fixed vertical align with bounds > 1 line
- render - added sort_by_z, sort_by_z_reverse, none
- build - fixed bug causing run to fail on prior errors
- build - fixed jump to error for project.luxe files
- docs - added link to wren in workflow
- docs - added a few more tutorials
- docs - cleaned up some details
- Sprite - added `Sprite.contains(entity, x, y)`
- ui - scroll handles block mouse events leaking
- engine - added event stream for e.g resized

## 1.0.0-dev.82

- anim - system refactored, data changed. see samples!
- anim - return instance handles, api uses them now
- anim - repeats renamed to play_count
- anim - loop=true/false added to data files
- anim - added anim modifier, with auto play list
- anim - fixed transform anims in edit mode
- anim - add reset on stop, so state can be reverted
- anim - add y range (so y can be relative or absolute)
- anim - add play_only for solo playing
- tiles - fix bugs in painting tiles
- camera - add get/set default for a world
- camera - get_fov_vertical, get_near, get_far, get_aspect
- camera - get_projection
- entity - Entity.world -> Entity.get_world
- world - systems have a priorty to sort by (temporary)
- transform - add get_rotation_matrix
- transform - add look at, set angle axis, rotate angle axis
- transform - scene load now tries linking objects on load
- mesh - add control over enabled and active mesh levels
- mesh - add level/element count getters
- ui - select all with cmd-a/ctrl-a in text fields
- ui - add component-wise size getters for controls
- ui - fix controls crossing UI canvas owners
- ui - add engine.ui.debug_vis setting
- ui - add input graph node setting to canvas
- ui - add window close/collapse functions
- ui - add canvas enumeration
- sat2d - fix shape sweeps, poly vs poly edge case
- sat2d - return intersected, normals for sweeps
- physics - don't simulate in edit mode
- physics2d - now using bullet only, liquidfun is gone
- physics2d - refactor APIs a lot. Body2D. see samples
- physics3d - blocking in 3d apis
- bytes - add clear, write_uuid
- math - add distance for vectors, lerp
- string - fix for fixed function broke on non decimals
- assets - string data loaded in release builds
- assets - wip packable modules
- io - added zip_decompress for byte decompression
- settings - fixed bool type (fullscreen flag now works)
- window - x/y positions can be set from settings
- window - engine.runtime.window.display for which monitor
- render - better error messages in several places
- render - fix array images internally (data driven atm)
- render - added array types to material inputs/shaders
- render - send camera pos and world matrix to shaders
- render - add image generate mipmaps helper
- render - add tick to renderer script
- render - add R and RG image types
- wren - experimental debugger support via vscode
- wren - added Num log2 and exp
- wren - fixed script compiler missing new fields
- wren - optimized script compiling, more reliable now
- wren - added `+= -= /= *= &= |= >>= <<= ^= %=`

## 1.0.0-dev.81

- web builds are back (`luxe deploy --target web`)
- image - add api for getting bytes from an image
- camera - add apis for get/set matrices
- camera - add look at, clip to world and world to clip
- tiles - apis for grabbing tiles by tag and depth
- fonts - fix accessing invalid pages sometimes
- bytes - fix `pos=` assertions
- fix CI for linux builds + include linux build

## 1.0.0-dev.80

- important world bug fixes
- added settings.lx loading 

## 1.0.0-dev.79

- update samples and readmes for samples

## 1.0.0-dev.78

- fixed custom modifiers
- lots of other stuff