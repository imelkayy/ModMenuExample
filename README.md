# Mod Menu Communcation

This repository serves as both an example on how to implement Mod Menu communication into your own mod, as well as a template for easy integration.

## Registering Your Mod

Retrieve a reference to Mod Menu's CCA. The example files included provide a function utilizing Wildcard's `GetActorWithTag()` system, which acts as a more-efficient CustomActorList. Mod Menu's tag for this system is `CCA_ModMenu`. Create a new JSON object and add a `StringField` with the key `CCATag`; set this field to the Tag on your CCA (Located: `Actor/Advanced/Tags`). Using the reference to Mod Menu's CCA, use the `SendModData()` interface function from the `ModCommuncation_Interface` with the Key `AddMod` and this newly created JSON object.
![image](https://github.com/imelkayy/ModMenuExample/assets/63310939/0ab6c266-ee33-4470-b486-e32c641a50a1)



## Creating Your Settings

Copy/move the `RuntimeSetting_Example` Struct and `SettingType_Example` Enums into your mod. If you make a copy, ensure you change the `SettingType_Example` enum to your newly copied one.

In a `CCA`/`Singleton`, create an Array variable using this Struct. Here you can setup each setting by creating a new element in the array. `UUIDs` MUST be unique; they are used to store and retrieve your settings. To prevent collisions, I recommend using a unique suffix or prefix for your mod.

 * **Category**: ModName/SubCategory  |  Leaving just ModName will place your setting directly under the mod divider in the settings UI. Adding /`SubCategoryName` will add the setting into a subcategory under your mod category.
 * **Description**: Index 0 - Description displayed by Mod Menu  |  Index 1 - Custom tooltip override


## Packaging Your Settings

Setting Structs MUST match the struct in this repo. 
To package your settings, implement the key `MMSettings` in the `RequestModData` function from the `ModCommunication_Interace`. Create a new JSON Object and a `Keys` local String array variable. `ForEach` element in your `Settings` array, add the `UUID` to the `Keys` array and `SetStructField` on the JSON object with the `UUID` as the key and Settings element as the value.

<img width="1000" alt="parsecd_71iZOg4lTf" src="https://github.com/imelkayy/ModMenuExample/assets/63310939/9065b447-e624-41d6-9627-7f1c58f727b0">


## Retrieving Your Settings

Located within the `MMCommFunctions` library is `GetSettingValue()`. This function implements Mod Menu's retrieval API, and includes a fall-back to `CCA_Example` if Mod Menu isn't installed.
<img width="1000" alt="parsecd_xPsB0oksTP" src="https://github.com/imelkayy/ModMenuExample/assets/63310939/64d760aa-540c-41ff-a6ea-ca9bf8343095">

The GetSetting() interface function runs the following on `CCA_Example`. It provides functionality for ini to be used when Mod Menu isn't present, as well as defaulting to your `Settings` array when an ini isn't loaded.
<img width="1000" alt="parsecd_AKuWdxRkR1" src="https://github.com/imelkayy/ModMenuExample/assets/63310939/a786f85f-fec8-4403-9d56-ceec12a6c707">

