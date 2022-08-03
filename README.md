![Turret That Dino](https://github.com/Kozenomenon/TurretThatDino_Source/blob/main/Mods/TurretThatDino/Icon/TTD_Repo_Icon.png?raw=true)

# Turret That Dino
 Simple mod for Ark: Surival Evolved that makes turrets shoot Cnidaria & Noglins. Has config to include modded dinos & some custom admin commands.
 
 [Steam Workshop Link](https://steamcommunity.com/sharedfiles/filedetails/?id=2591969003)

## Setup
 _I recommend you add this to your Ark Dev Kit while it is closed._
 
 You will notice that this repo has some folders in it. 
 
 They must be copied to the precise locations specified! 
 - `ModBase` must go to your kit's `Content` folder. 
   - e.g. `ARKDevKit\Projects\ShooterGame\Content\ModBase` 
 - `Mods\TurretThatDino` must go to your kit's `Content\Mods` folder. 
   - e.g. `ARKDevKit\Projects\ShooterGame\Content\Mods\TurretThatDino` 
 - `media` doesn't go to your kit, is just the font I used on workshop description. 
 
 _Note: Yes, ModBase lives outside Mods folder._ 
 
## Dev Notes 
 - The entire mod is basically a singleton actor: `/Mods/TurretThatDino/Data/TTD_CCA` 
 - Its a copy of `Template_CCA_Save` from my other open-source [ArkTemplates](https://github.com/Kozenomenon/ArkTemplates) 
 - INI Loaded at: `ConstructionScript` -> `LoadSettings` 
 - Dino Classes loaded at: `BeginPlay` -> `DelayedInit` -> `LoadDinoClasses` 
 - Edits Dino CDOs at: `BeginPlay` -> `DelayedInit` -> `EditDinoCDOs` 
 - Edits Existing Dinos at: `BeginPlay` -> `DelayedInit` -> `EditExistingDinos` 
 - For PIE & Live SinglePlayer, `EndPlay` reverts edits to dino CDOs 
 - Log output works in PIE and Live game. 
   - In live, needs `DebugMode=True` & launch args ` -log -servergamelog` 
   - In PIE, open the `Output` located on the kit's `Window` menu 
 - In PIE, some settings have different defaults. 
   - `DebugMode=True` 
   - `InfoLog=True` 
   - `AutoDiscoverVariants=True` 
 
## INI Settings
```ini
[TurretThatDino]
DebugMode=False
InfoLog=False
AddDinoPaths=
OverrideDinoPaths=
AutoDiscoverVariants=False
```
**DebugMode** <br> 
Default is False. Set to True for debug logging in live game. This will be more verbose than the `InfoLog` provides and is not intended for general use _(which is why this setting is not listed on the workshop's info)._ As with any live game logging, the host process needs to be launched with the args ` -log -servergamelog` or there will be no logs! <br>
**InfoLog** <br>
Default is False. Set to True if you would like to see some logging output in the server game log as to the functioning of the mod. It will output what dino classes are being edited for turret targeting. This occurs during game/server start. All InfoLog lines will start with [TurretThatDino] _(so you can search for them easily)_. <br>
**AddDinoPaths** <br>
Default is blank. This setting is for adding more dinos to be affected by this mod. Dinos listed here will be additive to the default (Noglin and Cnidaria + vanilla variants). 
Expected value is a comma-delimited list of dino spawn paths. Not class names- you need to put the entire spawn path for this to work. <br>
For example, Cnidaria's spawn path is: <br>
`/Game/PrimalEarth/Dinos/Cnidaria/Cnidaria_Character_BP.Cnidaria_Character_BP` <br>
**OverrideDinoPaths** <br>
Default is blank. This setting is for replacing the list of dinos to be affected by this mod. Use this setting if you don't want it to affect Cnidaria and/or Noglins (or a variant), and want to explicitly define the list yourself. 
Expected value is a comma-delimited list of dino spawn paths. Not class names- you need to put the entire spawn path for this to work. <br>
For example, Noglin's spawn path is: <br>
`/Game/Genesis2/Dinos/BrainSlug/BrainSlug_Character_BP.BrainSlug_Character_BP` <br>
**AutoDiscoverVariants** <br>
Default is False. Setting to True will cause a longer server start up time! Mod will search for modded variants for Cnidaria and Noglins. Will only find modded dinos if they are setup as child class of the vanilla. If they are done as copies then you will need to add their paths using one of the two settings above. <br> 

## Admin Commands
Admin commands can be entered via console or RCON. <br> 
If using RCON omit the 'admincheat' prefix. RCON outputs to logs. 

**Dinos** <br> 
`admincheat scriptcommand ttd dinos` <br> 
This will print out what dinos are affected by the mod. By default specifies class names. To output full paths include the argument 'path': <br> 
`admincheat scriptcommand ttd dinos paths` 

**FindVariants** <br> 
`admincheat scriptcommand ttd findvariants` <br> 
This will force the mod to attempt discovery of modded variants. By default scans dinos in the world. To instead do a class scan (which takes longer, will hang server), include the argument 'classes': <br> 
`admincheat scriptcommand ttd findvariants classes`