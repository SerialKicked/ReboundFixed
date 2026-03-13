This is a fixed version of the [ReBound: Ecounters](https://www.moddb.com/mods/stalker-anomaly/addons/rebound-encounters1) Addon for Stalker Anomaly. It fixes issues plaguing both 1.7.8 (the version this is based on) and more recent versions. This specific 1.7.8 version was picked due to it having less issues to fix than recent versions.

## ReBound: Encounter 1.7.8 [Working as Intended Edition]
- Changed spawn rate logic to better scale with progression while preventing the map population from getting out of control
- Changed Predator distance threshold (50 meters -> 500) so they no longer ignore everything until they're already eating the player's face
- Fixed Predator spawn logic so not everything becomes a predator after 3-4 spawn (no more increasingly large death balls)
- More stable experience overall, with no lag spikes.
- Added specific GAMMA fix to prevent the "i changed map, and everything crash with no log" caused by Rebound:Encounters.

## Details of the encountered issues in the mod and their fixes:
- The spawning logic was incorrect. The base addon would fill from 40% to 60% of whole active map all at the same time. In itself, it's not *that* big of a problem, but compounded with the other issues, it would lead to a very poor experience. The initial high value was probably meant to account for a very poor spawning logic (didn't properly track lairs, leading to failed spawns), but it caused more harm than good. This new version, instead, will generate 1+[stalker rank] spawns per loop. Free lairs are tracked before monster spawning, making sure we're not just running code for nothing. As such it's recommended to have a setting like 3-4 hour min time between spawns and a max of about 8. Decrease the values for a more "roguelike" experience.
- Predators are enemies who are tracking the player from wherever on the map. They're meant to give mutants to engage outside of lairs. The problem with the setting (beyond the issues above and below), is that they'd ignore EVERYTHING unless they are already right at the player. They would pass through other stalkers (and companions), even in plain view of the player. The distance has been changed to 500 meters. This means that they'll still traverse the map, but when they get roughly in view distance, they'll react to other stalkers and monsters properly, leading to a less artificial experience.
- The original script would incorrectly assign the "predator" tag to the templates instead of the spawned squads, meaning that after a few spawn rounds, every single mutant group would converge toward the player no matter how far away they are on the map until the game is reset. It would lead to a near uninterrupted (from player POV) flow of monsters. Now the script correcttly assigns the tag to the spawn squads only, keeping the setting meaningful. A 10% to 20% predator setting is recommanded.
- The code was poorly optimized, going through that "unholy" loop of listing the 65k possible map object for each attempt in the spawn loop. It was very error prone and very slow. Now everything is cached and optimized, and the 1-2 seconds freeze that would occure each spawn no long happen. Same goes for the code to pick between normal monster and bosses, those were 2 heavy functions and got turned into a 3 liners. There are more examples, but not worth elaborating on.
- Some logic issues could potentially lead to bad squad spawns as well, not sure they could occur in the wild, but it's fixed.

## Details of the GAMMA related issues that this update fixes:
- In "xr_conditions.script", the "check_npc_name()" function is missing a npc:alive() check, meaning that it'll incorrectly process entities that haven't been fully registered by the engine yet. It's normally not a big deal, but given that RE:Bound is spawning stuff in rapid succession, especially on save_load and when entering the map, this function ends up processing partially created units that still haven't been assigned their normal function list. Worse, it could be called so early in the loading of a map that it wouldn't even leave a trace in the log, making debugging near impossible. Plus, even if it may function without this fix when Encounters ain't installed, it's still technically a pretty bad bug.

## Recommended Settings
- General spawn from 4 to 8 hours for what would have been the intended experience with the base mod. Reduce to 2 to 6 if you want a busy zone.
- 10% to 30% predator spawn chance. Given it's a random chance, not a ratio, you'll still get "omg everyone on the map is chasing me" moments, but they won't be constant, contrary to before fix.
- Disable the whole "spawn vanilla monsters when no fight since X or at least move the threshold super high.
- Disable hayzed? I think it's those zombies that spawn by pack of 20 or something. I didn't edit their absurd spawn numbers yet.

## Installation
- Override the files or create a mod overriding the files.
- If using GAMMA, move the `GAMMA_FIX\xr_conditions.script` file to `gamedata\scripts\xr_conditions.script`

(no i'm not making an installer, if you somehow ended up here, you can manage)

Savegame-compatible with nomal Rebound: Encounter 1.7 (not with more recent versions)
