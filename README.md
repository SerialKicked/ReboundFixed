This is a fixed version of the [ReBound: Ecounters](https://www.moddb.com/mods/stalker-anomaly/addons/rebound-encounters1) Addon for Stalker Anomaly. It fixes issues plaguing bother 1.7 (the version this is based on) and more recent versions. This specific 1.7 version was picked due to it having less issues to fix than recent versions.

# ReBound: Encounter 1.7: Fixed.
- Fixed spawn rate so the map doesn't get full of monsters every 4 hours (favor a setting like 4-10 hours between spawns in settings)
- Fixed Predator behavior so not everything becomes a predator after 3-4 spawn (set predators rate to 20%)
- Fixed Predator distance threshold (50 meters -> 500) behavior so they no longer consider player character as their sole enemy until they went through a whole base
- Massive performance improvements in spawning logic
- More stable overall.
- Added specific GAMMA fix to prevent the "i changed map, and everything crash with no log" caused by Rebound:Encounters.

# Details of the encountered issues in the mod and their fixes:
- The spawning logic was incorrect. The base addon would fill from 40% to 60% of whole active map all at the same time. In itself, it's not that big of a problem, but compounded with the other issues, it would lead to a very poor experience. The initial high value was thought out due to a very bad spawning logic (didn't properly track lairs), but it caused more harm than good. This new version, instead, will generate 1+[stalker rank] spawns per loop, and properly assign lairs to said spawns. As such it's recommanded to have a setting like 3-4 hour min time between spawns and a max of about 8.
- The original script would incorrectly assign the "predator" tag to the templates instead of the spawned squads, meaning that after a few spawn rounds, every single mutant group would converge toward the player no matter how far away they are on the map. It would lead to a near uninterrupted (from player POV) flow of monsters. Now the script correcttly assigns the tag to the spawn squads only, keeping the setting meaningful. A 10% to 20% predator setting is recommanded.
- Predators are enemies who are tracking the player from wherever on the map. They're meant to give mutants to engage outside of lairs. The problem with the setting (beyond the issue above), is that they'd ignore EVERYTHING unless they are already right at the player. They would pass through other stalkers (and companions), even in view of the player. The distance has been changed to 500 meters. This means that they'll still traverse the map, but when they get roughly in view distance, they'll react to other stalkers and monsters properly, leading to a less artificial experience.
- The code was poorly optimized, going through that "unholy" loop of listing the 65k possible map object for each attempt in the spawn loop. It was very error prone and very slow. Now everything is cached and optimized, and the 1-2 seconds freeze that would occure each spawn no long happen. Same goes for the code to pick between normal monster and bosses, those were 2 heavy functions and got turned into a 3 liners.

# Details of the GAMMA related issues that this update fixes:
- In "xr_conditions.script", the "check_npc_name()" function is missing a npc:alive() check, meaning that it'll incorrectly process entities that haven't been fully registered by the engine yet. It's normally not a big deal, but given that RE:Bound is spawning stuff in rapid succession, especially on save_load and when entering the map, this function ends up processing partially created units that still haven't been assigned their normal function list. Plus even if it works without the fix if Encounters ain't installed, it's still technically a bug.

# Installation
- Override the files or create a mod overriding the files.

Savegame-compatible with nomal Rebound: Encounter 1.7 (not with more recent versions)
