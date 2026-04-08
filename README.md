# austinbeickert.github.io

Hello, this is my collection of coding projects:

Quick Navigation:
1. Data Cleanup in SQL https://github.com/austinbeickert/mycodebase?tab=readme-ov-file#data-cleanup
2. Dark Souls: The Musical. A Python Mod: https://github.com/austinbeickert/mycodebase?tab=readme-ov-file#dark-souls-the-musical
3. Target API and Data Scrape Skeleton: https://github.com/austinbeickert/mycodebase/blob/main/README.md#the-red-aisle
   
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##### data-cleanup
This is a basic data cleanup example done in SQL. These cleaned tables can now be easily analyzed in tools like Tableau.

It includes:
- creating backup raw-data tables
- converting dates
- trimming whitespace and special characters
- converting values into readable, analysis-friendly formats

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

##### Dark Souls: The Musical
Dark Souls' soundtrack is unique in that music is really only present during boss fights. Because of this, I often have my own music going in the background during my playthroughs. I already had a list of thematically appropriate songs I was listening to but I wanted to create a tool that could detect what stage of the game I was in and automatically play the right one. 

This mod is written in Python and utilizes the Spotify API to control music playback. For flexibility, ease of use, and 'Jolly Cooperation' (distribution), the mod pulls playlist links from a .txt file within the mod folder which can be edited to include any 12 playlists of your choosing. It is comprised of 4 modules whose functions are outlined below.

##### -main.py
main.py is the lead controller module. It loads the current playlist and checks for new boss kills every 5 seconds by reading your PC’s memory during gameplay. I used a free application called Cheat Engine to narrow down the memory values I needed to track boss. Killing a boss in Dark Souls has a cascading impact to dialogue options, NPC story progression and location, and even physical parts of the map. This means a “boss death flag” is already a value used by the game. I used this memory address to determine the playlist to queue next. 

Below is an example of the checked memory value for the first boss. When the bit changes from a 0 (False) to a 1 (True), it means the boss is flaged as DEAD.
"Asylum Demon": {"base_offset": 0x01C7C5F0,"offsets": [0x20, 0x0, 0x1],"bit": 7}

##### -boss_flags.py
This is the bookkeeping module. It tracks boss flags and deaths for every required boss in the game and the returned values are fed to the main controller.

Inside this module, there is one main function named get_all_boss_statuses() which is called by the main controller every 5 seconds. It checks each boss flag at its assigned memory segment and returns a status for each boss. This status list is saved as 'previous_status' and then compared to a new 'current_status' list every 5 seconds as the player runs through the game. When a new boss dies, the current_status now contains a new DEAD / 1 value, triggering a playlist change because it no longer matches the previous status list.

##### -playlist_loader.py & jukebox_playlists.txt
This module first vets the local jukebox_playlists.txt file to confirm there are enough playlists and then loads the first one. It contains logic to return the current playlist, skip to the next playlist, and pause playback completely. It’s important to note that this module only controls which playlist is selected - it does not trigger playback. Playback is handled by spotify_controller.py. 

##### -spotify_controller.py
This controller works in tandem with playlist_loader.py and the Spotify API to pause or resume playback. It does not control which songs or playlists are selected - only whether playback is active. As previously mentioned - playlist selection is handled by playlist_loader.py.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------   

#The Red Aisle
This is the beginning of a data scraping program using Target’s API, called “The Red Aisle.” Currently, it returns product information for a specific item from a specific ZIP code. This can be expanded with filters, automation, and a GUI to generate alerts for new deals.
