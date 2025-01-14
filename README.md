# sway_workspaces
Restore workspaces in sway to displays and move applications to their workspaces.  

## Setup
1. Setup displays, e.g. with `wdisplay`
1. Download from [Actions](https://github.com/Nama/sway-workspaces/actions) or [Releases](https://github.com/Nama/sway-workspaces/releases)
1. `unzip` and `chmod +x` the file
1. Run after setting up your displays `./sway_workspaces save <profilename>`
1. Repeat for every display setup
1. Run `./sway_workspaces load <profilename>` to restore windows into workspaces
1. Make kanshi run this command for more automation


## Development
### Used objects in tree
* `output` in `workspace` is replaced with the display name
* The `nodes` list is used. All workspaces are in outputs (displays)
  * Windows are in a list in workspaces
* Windows can be nested indefinitely, so `node_getter()` is used
  * This can happen if the user changes the layout of single windows, hence creates a new container
* Workspace to output mapping are saved seperately from the sway tree json
* We get the information about which window is in which workspace from the tree: `i3.get_tree()`, `swaymsg -t get_tree`
* There is no known way (to me) to identify the exact same windows after a reboot (to move multiple windows of the same tool)
  * `window` is only set for Xwayland windows - and is reset after a reboot
  * node IDs reset after a reboot

### Notes
Had to alter the default tree, so the outputs are not the ports (display name instead of DP-1).
Now it doesn't matter if more ports appear after docking.
