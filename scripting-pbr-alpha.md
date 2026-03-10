# Scripting PBR Alpha

# Introduction

Blinn-Phong transparency and PBR Alpha have some nuanced differences that need to be understood when handling PBR transparency for showing and hiding parts of the mesh. These differences need to be accounted for in order to ensure the intended outcome.\
This guide is aimed at scripters, but others might find it interesting as well.


# Alpha Modes

Let’s start with Alpha modes:

| **BP Mode**                 | **PBR Mode**                    | **Details**                                                         |
| --------------------------- | ------------------------------- | ------------------------------------------------------------------- |
| PRIM\_ALPHA\_MODE\_NONE     | PRIM\_GLTF\_ALPHA\_MODE\_OPAQUE | No Blending                                                         |
| PRIM\_ALPHA\_MODE\_BLEND    | PRIM\_GLTF\_ALPHA\_MODE\_BLEND  | Alpha Blending                                                      |
| PRIM\_ALPHA\_MODE\_MASK     | PRIM\_GLTF\_ALPHA\_MODE\_MASK   | Alpha Masking                                                       |
| PRIM\_ALPHA\_MODE\_EMISSIVE | -                               | Emissive is not applicable to PBR, it has separate emissive texture |

The actual standard blend modes are available for both and work pretty much the same, in LSL the int values of these defines are also the same, so as long as emissive is not used, they end up being interchangeable.

For the rest of this guide, we’ll ignore emissive mode as it is not compatible between the two material types.


# Viewer Differences

While the alpha modes are pretty much the same in server code, there are significant differences in how the viewer handles them. Blinn-Phong follows a special viewer behaviour while PBR follows the glTF specification.


### The special behaviour

Let’s say you have a basic prim cube with the default plywood texture, and a script that sets Blinn-phong Alpha mode to Blend. This will be saved in the object data by the sim, however, once that information reaches the viewer, it might be ignored. If you try to set the blend mode using the viewer’s edit folate, on that same cube, you’ll notice that the alpha mode dropdown is greyed out and forced to Opaque. The viewer checks whether the texture has an alpha channel and disables/enables alpha blending based on that, no matter what the real value is set to on the object. This means that RGB Texture + Blend Mode = None, while RGBA Texture + Blend = Blend. Since the alpha channel information is only available in the viewer, as the simulator does not download textures (it only deals with UUIDs), scripts have no way of knowing if alpha channel is available or not, and can’t verify what the viewer is actually currently showing.

PBR however, does not have this behaviour, it follows the glTF specification, which means it always listens to the Alpha blend mode, no matter if the texture has Alpha channel or not. This means the script and the server always knows the correct state, but that can lead to certain side effects.

Here’s a pair of 0.8 alpha objects in Blend mode, BP on the left PBR on the right:

Here’s the same objects with alpha values of 1.0 in Blend mode:

And for completeness sake, both objects with None/Opaque mode:


# Scripting functions

Here’s a quick list of scripting functions that are relevant.

- llSetLinkAlpha - <https://wiki.secondlife.com/wiki/LlSetLinkAlpha>

  - A simple, BP only, alpha manipulation.

- llSetLinkPrimitiveParamsFast - <https://wiki.secondlife.com/wiki/LlSetPrimitiveParams>

  - A versatile parameter manipulation function that can change any parameter.

  - It has been updated to support PBR as was the primary way of modifying any PBR parameter and is still the primary way to manipulate PBR Override textures.

- llSetLinkRenderMaterial - <https://wiki.secondlife.com/wiki/LlSetLinkRenderMaterial>

  - A simple function to set PBR material.

Here’s some functions that have been added after the initial PBR release:

- llSetLinkGLTFOVerrides - <https://wiki.secondlife.com/wiki/LlSetLinkGLTFOverrides>

  - A function that provides a more granular ability to set material parameters. It cannot set texture UUIDs.

  - It has an important caveat (covered in a later section)!

- llIsLinkGLTFMaterial - <https://wiki.secondlife.com/wiki/LlIsLinkGLTFMaterial>

  - A simple function to check if a face has PBR material, as getting material UUID will return a NULL\_KEY if the owner does not have permissions OR if there is no material set, so regular method of getting material UUID cannot determine if material actually exists, or the script simply does not have permission to see it.


# Materials and Overrides

So, this is the part that confuses a lot of people. PBR materials kind of have two parts, I’m not sure the official terms LL call them, but for the purposes of this guide, we have:

- PBR Material Object

  - This is the item that sits in your inventory, with a little sphere icon, that can be opened and edited.

- PBR Overrides

  - The overrides are a secondary set of PBR data that exist only on the object itself. Editing PBR material on the object (as opposed to inventory), will edit the overrides and NOT the Material Object, unless you press “Save As” which will create a new PBR Material Object that is the previous Material Object with the changes made to Overrides saved on top.

Let’s break it down a little, let’s say you rezz a default plywood cube, you then end up with:

|             |              |               |
| ----------- | ------------ | ------------- |
| BP Material | PBR Material | PBR Overrides |
| Plywood     | None         | None          |

Then, from your inventory you apply a material of your choice, let’s say a Gold PBR Material, you end up with:

|             |                   |               |
| ----------- | ----------------- | ------------- |
| BP Material | PBR Material      | PBR Overrides |
| Plywood     | Gold PBR Material | None          |

BP material is still there, the viewer simply ignores any BP settings or textures and uses the PBR Material instead.

Now, you open the edit floater and change the PBR Base Colour Factor (Tint) to Silver, you end up with:

|             |                   |                  |
| ----------- | ----------------- | ---------------- |
| BP Material | PBR Material      | PBR Overrides    |
| Plywood     | Gold PBR Material | Colour -> Silver |

Notice that the Gold PBR Material and default Plywood were not affected at all. When the viewer renders this object it loads the original Gold PBR material, then checks the Overrides and sees, “Oh I need to change the colour”, and then it changes the colour to Silver and renders it on screen.

If you were to press “Save As” in the edit floater, you’ll end up with a “Silver PBR Material” in your inventory, the gold colour value gets replaced by the silver colour.


### PBR Material Object vs PBR Overrides

PBR Material Object is a single object with a single UUID. The simulator does not load the contents of the material and has no idea what textures or settings are inside, it only knows the UUID. Only the viewer knows what’s inside the Material Object, and based on permissions it may or may not allow the end user to view or edit them.

PBR Overrides are a multiple set of properties that live on the object, which override any value specified in the PBR Material Object. Scripts have full access to all of the override properties. Set/Get Primitive Params functions can access them via PRIM\_GLTF\_BASE\_COLOR, PRIM\_GLTF\_NORMAL, PRIM\_GLTF\_METALLIC\_ROUGHNESS and PRIM\_GLTF\_EMISSIVE. For the purposes of this guide we’ll focus on the PRIM\_GLTF\_BASE\_COLOR as that contains the base colour tint and alpha parameters.


# Bringing it all together

**EDIT:** This bug has been brought to my attention, while my guide provides correct parameters to be used, due to this bug the functionality won’t work as expected, until it is fixed I’d recommend using the _llSetLinkPrimitiveParamsFast_ variants if clearing overrides is important.\
<https://feedback.secondlife.com/scripting-bugs/p/llsetlinkgltfoverrides-fails-to-clear-alpha-override> 

So, with all of that out of the way, how do we actually change transparency on an object?

Let’s start simple and make a box fully transparent using BP:\
_llSetLinkAlpha(LINK\_THIS, 0.0, ALL\_SIDES)_

or

_llSetLinkPrimitiveParamsFast(LINK\_THIS, \[PRIM\_COLOR, ALL\_SIDES, clr, 0.0])_\
Notice one key difference between the two? The LLSPPF requires a colour parameter, which first has to be either read from the primitive, or maybe we want to change colour at the same time as well.

So, can we do the same for PBR? Kind of, but not exactly:\
_llSetLinkGLTFOverrides(LINK\_THIS, ALL\_SIDES, \[OVERRIDE\_GLTF\_BASE\_ALPHA, 0.0])_

or

_llSetLinkPrimitiveParamsFast(LINK\_THIS, \[PRIM\_GLTF\_BASE\_COLOR, ALL\_SIDES, "", "", "", "", "", 0.0, "", "", ""])_

Quite a bit different from BP, but also, there’s many caveats hidden here. The first obvious one is a lot of “” parameters given to the LLSPPF function. The PRIM\_GLTF\_BASE\_COLOR sets all of the overrides for the base colour all at once, which includes the Texture UUID, scale, offsets, tint colour, alpha mode and so on, in this example, I’ve set them all to “” which means - “Remove this override” - when an override is cleared, that specific property will be read from the PBR Material Object that is applied to the face, or it will just pick the default value defined by the glTF specification.

If you actually try running either of those functions, you’ll notice that the object doesn’t actually turn transparent at all, this is because the default blend mode is Opaque, so if the material is Opaque, and the override does not change it, the object will remain visible. Therefore we need to do the following:\
_llSetLinkGLTFOverrides(LINK\_THIS, ALL\_SIDES, \[OVERRIDE\_GLTF\_BASE\_ALPHA, 0.0, OVERRIDE\_GLTF\_BASE\_ALPHA\_MODE, PRIM\_GLTF\_ALPHA\_MODE\_BLEND])_

or

_llSetLinkPrimitiveParamsFast(LINK\_THIS, \[PRIM\_GLTF\_BASE\_COLOR, ALL\_SIDES, "", "", "", "", "", 0.0, PRIM\_GLTF\_ALPHA\_MODE\_BLEND, "", ""])_

We need to also override the alpha mode.

If you run the functions now, you’ll see that the object does turn invisible. To restore it, you can call:

_llSetLinkGLTFOverrides(LINK\_THIS, ALL\_SIDES, \[OVERRIDE\_GLTF\_BASE\_ALPHA, "", OVERRIDE\_GLTF\_BASE\_ALPHA\_MODE, ""])_

or

_llSetLinkPrimitiveParamsFast(LINK\_THIS, \[PRIM\_GLTF\_BASE\_COLOR, ALL\_SIDES, "", "", "", "", "", "", "", "", ""])_

This will clear all the overrides. However we can also do this:

_llSetLinkGLTFOverrides(LINK\_THIS, ALL\_SIDES, \[OVERRIDE\_GLTF\_BASE\_ALPHA, 1.0, OVERRIDE\_GLTF\_BASE\_ALPHA\_MODE, PRIM\_GLTF\_ALPHA\_MODE\_OPAQUE])_

or

_llSetLinkPrimitiveParamsFast(LINK\_THIS, \[PRIM\_GLTF\_BASE\_COLOR, ALL\_SIDES, "", "", "", "", "", 1.0, PRIM\_GLTF\_ALPHA\_MODE\_OPAQUE, "", ""])_

To set the overrides to be explicitly Opaque.

That’s the basics, however, now comes the fun part - the caveats!


### Caveat #1

If you look at the wiki page of llSetLinkGLTFOverrides you’ll see the following caveat listed:

OVERRIDE\_GLTF\_BASE\_COLOR\_FACTOR and OVERRIDE\_GLTF\_BASE\_ALPHA parameters are coupled. If an override is set for one parameter, then the other is automatically given an override. The default OVERRIDE\_GLTF\_BASE\_COLOR\_FACTOR is <1,1,1>, and the default OVERRIDE\_GLTF\_BASE\_ALPHA is 1.0.

There’s also a Canny here (you don’t have to read it right now, but an upvote would be appreciated): <https://feedback.secondlife.com/scripting-bugs/p/pbr-colour-data-is-lost-when-setting-pbr-overrides>

So, what does it actually mean in English?\
Colour and alpha in PBR materials is actually a single RGBA value. That means, if there is no PBR Override set for either colour or alpha, the value is “”, which is empty. Then if you set the colour to, let’s say <0, 1, 0>, the alpha value will be filled in with the default value, so it will end up as <0, 1, 0, 1>. If you do it from the other way around, by setting the alpha to 0.5, you’ll end up with <1, 1, 1, 0.5>.

Seems a bit odd, doesn’t it? Now, here’s where it becomes an issue. Let’s say we have a simple PBR Material Object that is a Gold material, and that material is supposed to represent a perfectly smooth polished gold surface, to create such material we do not need a texture, we can just set the base colour tint, roughness, and metallic factors and leave the rest as default. Since we’re not using textures, we get a perfectly smooth gold material with very minimal performance impact on the viewer, as it has no textures to deal with.

Then, let’s apply that material to a simple cube and use one of the previous functions to hide it. This part will work fine. Then, unhide it using the explicit alpha mode functions - you’ll immediately notice the problem, our gold is now actually kind of silvery.

Why is that?\
Well, because before hiding the object we had:

|             |                   |               |
| ----------- | ----------------- | ------------- |
| BP Material | PBR Material      | PBR Overrides |
| Plywood     | Gold PBR Material | None          |

After unhiding the object we have:

|             |                   |                     |
| ----------- | ----------------- | ------------------- |
| BP Material | PBR Material      | PBR Overrides       |
| Plywood     | Gold PBR Material | Colour -> <1,1,1,1> |

See, the unhide process has overwritten our gold material with <1, 1, 1, 1>


#### How do we fix this?

One method would be to set all overrides to “” to fully clear them, which will work in some cases, but, it will lead to Caveat #2.

Otherwise we need to know the colour value before we hide the object so we can restore it later. But, this is tricky, remember, PBR Material Objects are a black box, scripts cannot read their contents, so if the colour value is not white, setting an override with the default value will result with white being filled in anyway.

On the other hand, if the override is already set, the llSetLinkGLTFOverrides will preserve it, however, for llSetLinkPrimitiveParamsFast (more about it in Caveat #2) the colour value needs to be saved manually.


### Caveat #2

The LLSPPF function sets ALL overrides at the same time, this function call:\
_llSetLinkPrimitiveParamsFast(LINK\_THIS, \[PRIM\_GLTF\_BASE\_COLOR, ALL\_SIDES, "", "", "", "", "", 1.0, PRIM\_GLTF\_ALPHA\_MODE\_OPAQUE, "", ""])_Might look like we’re just unhiding the object, but if you look at Caveat #1, you’ll spot the same issue… and more. This not only clears the colour, but also all the other parameters: texture, texture offsets, texture scale, colour, double sided flag etc.

Let's break it down again, rezz a simple cube and apply any PBR material, then edit the object and change the base colour texture to any texture you want, then also change the tint to some colour that is not white, so red or green will do. If you run a command to hide the cube, it will work just fine, but running the above LLSPPF function will result in, before:

|             |                     |                                   |
| ----------- | ------------------- | --------------------------------- |
| BP Material | PBR Material        | PBR Overrides                     |
| Plywood     | Chosen PBR Material | Texture: Some texture Colour: Red |

And after unhide:

|             |                     |                      |
| ----------- | ------------------- | -------------------- |
| BP Material | PBR Material        | PBR Overrides        |
| Plywood     | Chosen PBR Material | Colour: <1, 1, 1, 1> |

And that’s a problem. We lost all of the manual edits.


#### How do we fix this?

Unlike with Caveat #1 we can't just set all overrides to “”, because that will still remove the override texture and colour, and we’ll have:

|             |                     |               |
| ----------- | ------------------- | ------------- |
| BP Material | PBR Material        | PBR Overrides |
| Plywood     | Chosen PBR Material | None          |

Which is bad…

So, to fix it properly we actually need to have all the other properties as well.


### Caveat #3

This is a simple one - changing a PBR Material Object on an face, will clear all PBR Overrides on that face (except for texture scale/offsets in certain scenarios)


# So, how do I actually show/hide things then?

## Option 1

In my opinion this is the best option and I’d recommend using this approach at the time of me writing this guide.


#### Variant A

Using llSetLinkGLTFOverrides.

To Hide:

1. Read PBR Override Colour (this will give you either “” or the actual colour value)

   1. Ideally, you could also store the alpha mode for later, but that would require storing it longer than the function call. Storing Alpha Mode would preserve any edits people have made, for example changing it to mask mode.

   2. You could also store the colour/alpha/alpha mode in the BP settings, this will also make it compatible with fallback textures. However don’t forget llLinear2sRGB and llsRGB2Linear for the colour value. BP wants sRGB, PBR wants Linear.

      1. Colour is actually optional, since setting it in the override will preserve it. The only problem is when the initial override is not set, or it is cleared at some point

2. Call llSetLinkGLTFOverrides and include both alpha and the colour.

To Show:

1. Read PBR Override Colour (this will give you either “” or the actual colour value)

2. Call llSetLinkGLTFOverrides and include both alpha and the colour

   1. And alpha mode if it was stored.


#### Variant B

Using llSetLinkPrimitiveParamsFast

To Hide:

1. Read all of the PRIM\_GLTF\_BASE\_COLOR properties

2. Do a list replacement to swap out the Alpha value and Alpha Mode

   1. Keeping the rest of the list untouched will preserve any manual edits

   2. You could also store the colour/alpha/alpha mode in the BP settings, this will also make it compatible with fallback textures. However don’t forget llLinear2sRGB and llsRGB2Linear for the colour value. BP wants sRGB, PBR wants Linear.

      1. Colour is actually optional, since setting it in the override will preserve it. The only problem is when the initial override is not set, or it is cleared at some point

3. Call llSetLinkPrimitiveParamsFast and with the updated list

To Show:

1. Read all of the PRIM\_GLTF\_BASE\_COLOR properties

2. Do a list replacement to swap out the Alpha value and Alpha mode

   1. And Alpha mode if it was stored

3. Call llSetLinkPrimitiveParamsFast and with the updated list

Variant B has an advantage if you’re also supporting BP, as you can set BP alpha in the same function call by adding the PRIM\_COLOR parameters to the same list.

This method handles all the caveats the best, if the PBR Override for tint colour is set before any show/hide happens, this will actually handle it perfectly. Also if the PBR Material Object has pure white tint and colour comes from the texture, it will work perfectly as well. Any hand made changes on the object itself will be preserved, and if the end user changes the tint colour, that guarantees the tint colour is set, therefore making this method work.

However, if the alpha mode is not stored, it will lose any overrides in that case.

Depending on how you decide to store colour/alpha mode or if you decide not to store it, the script may not need any long term storage, the overrides list can be read each time, and discarded after the LLSPPF/llSetLinkGLTFOverrides.


## Option 2

Make two PBR Material objects. One of them should be opaque, the other should be an empty transparent PBR material.

To Hide: set the material to the transparent one.

To Show: set the material back to the original one.

I would not recommend this method. Due to Caveat #3 any manual edits will be lost. Also, your script then needs to store UUIDs in the long term. And if each face has a different material, that can add up. This would also clear any custom materials the end user might have applied, not just the overrides.


## Option 3

Using the functions as shown in the “Bringing it all together” section.

I would not recommend, since as explained in that section you’ll be running into all of the caveats.


# What do you use?

In my PBR Alpha Fixer script ( <https://marketplace.secondlife.com/p/PBR-Alpha-Fixer-Script/25990363> ), which can be used to sync BP alpha to PBR, in items that have not been updated to do it themselves. I use a variant of Option 1, the script that syncs the colour as well, has no caveats because it can read the BP colour at all times. For the variant that does not sync the colour, end users have to manually tint the faces, to supply the initial colour value, which once set, doesn’t get reset.

In my other items which show/hide parts, I also use Option 1. I do not store the alpha mode as my items are not really designed to work with textures that would have transparency so Blend/Mask modes are not really needed.


# Other

##### Can I use Mask Mode and Alpha Cutoff at 1.0 for more efficient discarding of invisible objects?

In theory and based on glTF spec, that should work, in practice - Nope:\
[https://feedback.secondlife.com/bug-reports/p/noise-occurs-when-alpha-mode-is-mask\
https://feedback.secondlife.com/bug-reports/p/pbr-material-produces-flickering-noise](https://feedback.secondlife.com/bug-reports/p/noise-occurs-when-alpha-mode-is-mask)

Once that bug is fixed, and the viewer is updated, yes.


##### How do I fix Caveat #1?

Hi, Geenz, Rider, Leviathan, Pepper, Monty, or any other Linden from the Simulator or Viewer team!\
(if you’re not a Linden, you can’t fix it, sorry)\
Simply extend the ‘Generic Streaming Message PBR Overrides’ packet. Add 3 more fields - a flag to indicate use of “v2” behaviour, colour field and alpha field. Edit the viewer so if it sees the “v2” flag it ignores the “bc” field, and instead checks if the new colour and alpha fields exist, if they don’t load colour/alpha from the material, if they do load those fields as the overrides. This will split the colour and alpha parameters on the viewer side. Then make the simulator send those 3 extra fields. Since this packet is simply LLSD, the older viewers will just ignore the extra fields, and the up to date viewer can process the fix. Oh and if the simulator doesn’t send the “v2” flag, it could do what it does now, for compatibility of a newer viewer with older simulators.

\


Written By: Kristy Aurelia - but please don’t poke me about it randomly in-world (Discord IMs are okay), unless there are some important mistakes or incorrect information. If you see me in a conversation in some group chat or Discord or in-world somewhere - where I am clearly not in the middle of anything, then it is fine. Thanks. Feel free to share the link with anyone if you find it useful. Feel free to share the information from this guide with or without attribution, as long as the information is correct, the more people know it, the better for everyone!
