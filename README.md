# Mod Menu Integration (v2)

This repository is setup to make Mod Menu integration as easy as possible. Before I walk you through my process, here're a couple important things to remember:
- In order to keep references intact, ensure this repository is cloned into a file named `ModMenuExample`.
- Utilize the `move` function to move all files in this repository into your project at once. This will maintain references and make the process much smoother!

# Step-By-Step

## Step One: Clone The Repository
- This repository is setup like a mod, so clone it into `ShooterGame/Mods/ModMenuExample`.
- With the kit open all files should be at the main-level, and not nested within any files.
- Outside of the kit, files should be located within `ShooterGame/Mods/ModMenuExample/Content`  
![ShooterGameEditor_NgfcZie0nb](https://github.com/user-attachments/assets/f9928bff-69e9-42b0-90e7-5ddaa7b816d0)

## Step Two: Move The Files
- Select all of the files within the `ModMenuExample` directory
- Drag them into your mod, selecting the `move` option to move all files (this is **very important** because it **maintains references**, meaning it should almost be plug-and-play!)  
![LzrQcb4hxn](https://github.com/user-attachments/assets/ba23aadd-5169-4477-9d8b-d9ea7fe3eb4e)

## Step Three: Rename The Files
- To ensure you don't accidentally reference another mod's copy of the communication files, I recommend renaming the files!
- Optionally, you should move them into subfolders at this point to keep consistency with your project's organization.  
![ShooterGameEditor_4ZgEYOw0UQ](https://github.com/user-attachments/assets/0f2bad48-d8f3-4536-917f-d2dd7c3796f8)

## Step Four: Add The CCA To Your MDA
- Open your mod's ModDataAsset and search for the "ExtraWorldSingleSingletonActorClasses" array
- Add a new element and set it to your newly renamed CCA
- Save your ModDataAsset, and reset the PGD cache if necessary (this can be accomplished by restarting the kit, or opening a level not linked to your MDA/PGD and running PIE, then switching back)  
![ShooterGameEditor_NiMdoTXlAz](https://github.com/user-attachments/assets/5c6afee7-a008-412e-be8d-f9f5c9d10f37)

## Step Five: Setup The CCA
- Open the newly moved CCA file, and navigate to the defaults tab.
- Sort by modified properties only; you should see Advanced > Tags.
- Change the tag at index 0 to `CCA_YOURMODNAME`. Copy this value with `CTRL+C`, since we'll be using it again in a moment.
   - ![ShooterGameEditor_1qGAN3c1gQ](https://github.com/user-attachments/assets/c1feab1e-b46f-4e78-954f-9d362017648c)
- Remove your `Modified Properties Only` filter, and configure the `IniSection` variable. Set this to the ini header that should be used when Mod Menu is not installed.
- Save and compile the CCA.  

![ShooterGameEditor_pS1mmQ5kuL](https://github.com/user-attachments/assets/365c8b60-d8b2-48c1-b9b1-93a811023c91)

## Step Six: Setup The Function Library
- Open the `MMCommFunctions` library.
- Open the `GetCCA()` function, and change the `CCA_Example` name literal to the value of your CCA's tag.
- Then, rename the function category name to incorporate your mod's name. This will make it much easier to ensure you're using the correct function!
   - ![ShooterGameEditor_LP0OP2Aqy0](https://github.com/user-attachments/assets/c825b120-e04b-4e13-a42f-c370a25c5497)
- Save and compile, then close the library.  
![ShooterGameEditor_qOQjSATYXh](https://github.com/user-attachments/assets/592809af-84b2-4594-a8e8-78b4673d97c5)


# Settings

## Creating

Your mod's settings are stored in the `settings` array within the CCA. You can add new settings by adding to this array. Here are descriptions for each setting parameter:

 - **Category:** (Name) the category your setting belongs to. Standard: ``ModName/Category``. NOTE: The category is used to sort by-mod, so this standard is critical!
   - Spaces ARE allowed in both the mod & category name.
   - Sub-Sub Categories are not currently supported.
- **UUID:** (Name) the Unique ID of your setting.
   - Standard: ``ModPrefix_SettingName``
- **Type:** (Enum) the TYPE of setting that your setting is.
- **Name:** (String) the name of the setting, displayed to the player.
- **Description:** (String) the custom description of your setting
   - Index 0 == Setting Description
   - Index 1 == Custom Tooltip (optional)
- **Value:** (String) the DEFAULT value of your setting. Will NOT be overridden on preexisting saves if changed later.
- **MinMax:** (String[]) the minimum and maximum allowed values that can be input.
   - Required for numerical types (`Float`, `Int`): Index 0 == min, Index 1 == max
   - Vector format is: `x,y,z`: Index 0 == min, Index 1 == max
   - Classes utilizes this for: Index 0 == Parent Classpath, Index 1 == Num Allowed Selections

## Loading

In order to load settings, you need to use the `GetSettingValue()` function. This function only works server-side, as Mod Menu does not handle replication. Simply pass in the name of your setting, and then cast the `string` output into the correct type. From here, you can use the value as you wish; I recommend storing it within a variable for later use.
- If storing settings as variables, you will need to manually handle updating them if/when mod menu dispatches an update notification. The CCA template automatically handles receiving this in the `LoadSettings()` function; using this you can inform whatever actors of yours need to be notified of an update and have themn refetch.  
Here's an example of how I load settings in one of my mods:
![image](https://github.com/user-attachments/assets/208d06c5-c867-4ee6-a163-17dae1834f1c)

