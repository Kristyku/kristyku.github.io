# PBR, BP, glTF and You

# Introduction

Okay, so first of all let’s figure out the letter soup (no need to read those websites, unless you really want to):

- **PBR** - Physically Based Rendering. It refers to a common set of guidelines for realistic rendering.

  - <https://en.wikipedia.org/wiki/Physically_based_rendering>

- **BP** - Blinn-Phong, named after people who invented it, it is a specific rendering technique.

  - <https://en.wikipedia.org/wiki/Blinn%E2%80%93Phong_reflection_model>

- **glTF** - GL Transmission Format. GL in this case is more like a brand name and it comes from OpenGL, which is Open Graphics Library. 

  - <https://registry.khronos.org/glTF/specs/2.0/glTF-2.0.html>

Now that we got that out of the way, what on earth does that mean and what does any of that have to do with Second Life?

First of all, I’d like to mention that this is quite a complex topic, but I’ll try to keep it as simple as possible. If you’re a SL content creator you might find the Wiki page written by Linden Lab more useful <https://wiki.secondlife.com/wiki/PBR_Materials> as it is a bit more straight to the point, while I’ll keep this guide simpler and directed towards non-creators and DIYers, however I will include some information for creators as well (the Material Overrides section is mostly for creators, it has some info gathered from few other wiki pages too).


## Blinn-Phong

BP is what SL used to use before PBR, when you had Advanced Lighting Model enabled, BP is a rendering technique invented in 1977 and was widely used in games in the 90s and early 00s, so it is pretty old.


## PBR

PBR is the new kid on the block, that started out in 1997 and grew in popularity between 2004-2010 and has been very wide spread since then, which means SL adopting it brings us more in line with the rest of the games industry. (While SL might not be a game, the SL viewer works exactly like any other video game application and therefore all the same technology is applicable)

If you want more history about those two, there is a very good video that is quite technical, but it does a pretty good job in simplifying the details so you should be able to understand it quite well even without prior knowledge. Definitely worth a watch if you have time and it will provide you a lot more context about all of it - [The Secret Behind Photorealistic And Stylized Graphics](https://www.youtube.com/watch?v=KkOkx0FiHDA) Do note, that video is not about Second Life at all, because PBR is an industry wide standard, applicable to many many applications.


## So… Where does SL Come in?

Second Life is moving (very slowly) towards the industry standard glTF implementation, and Physically Based Rendering is a small subset of it. Which means Physically accurate visual style that is used by many many other software applications, including Blender, Substance Painter, Unity, Unreal, thousands of games and so on.

So, what is this glTF thing? If you want the boring version, there is a very very big technical document explaining it, that I’ve linked earlier… but nobody got time for that, so the short version is, it describes in detail how various assets are stored and how they should be handled, so exported files from one application can be opened in another, it has standard definitions of how meshes, animations, skeletons, lights and whole load of other stuff defined too. And among them all as well, PBR Materials (we’ll get to those later, as most of this guide will be about them).

That means it will be a lot easier to make content for SL, as there are lots of different tools that follow that standard, it also helps to ensure that content looks almost the same in all of this different software, so if someone makes something in Blender, it doesn't look completely different in SL.

I say ‘almost’ and ‘completely different’ because, there will still be differences, most of which will be determined by environment settings, different coloured lights etc.


## Where are we at now?

As of me writing this guide in July 2024. Second Life has implemented PBR Rendering and PBR Metallic-Roughness materials since Nov 2023 and majority of viewers have caught up with it as well and are PBR capable.

Linden Lab are focusing on implementing Transmission Material, and glTF scene import (mesh & animation parts).

Since we only have a part of glTF in Second Life, things will not look exactly the same as they do in other software that fully supports glTF.


## Where are we heading?

Linden Lab stated multiple times that they want to support the entire baseline glTF specification, as well as certain extensions that are applicable, or generally useful for SL, and maybe even more extensions based on user feedback and requests from creators.

If you’d like to see what it actually looks like, you can head over to <https://github.khronos.org/glTF-Sample-Viewer-Release/> this is a sample viewer, used by a lot of developers of various software to ensure that their glTF implementations are compatible with each other. Check out IORTestGrid in the Models dropdown to see an example of what Transmission and IOR should enable in SL. ClearCoatTest is another interesting material, but that one is considered an extension, so might not reach SL any time soon, unless a lot of creators ask for it, and so on. There are many examples to check out, which demonstrate different parts of the glTF specification.

Does that mean more drastic changes like the one we just had?

No, not exactly the rendering part of PBR changes is the most visually impactful one, and the most obvious even to people who know nothing about PBR. Large portions of other changes will be a lot more incremental, and some even invisible to non-creators.


# PBR Materials

All examples shown are in an environment with Midday lighting preset and manual reflection probes - to learn more about reflection probes see my other guide [Reflection probes and You](https://docs.google.com/document/u/0/d/18ut5mR_S9sAYDwWvFHNpRqrJ31y2hpqVSZHd8sbeua4/edit)


## What are those materials and didn’t we already have “materials”?

So, before PBR, with BP, Second Life had:

- Diffuse Texture

- Normal map (Bumpiness)

- Specular (Shininess) textures.

_(Most viewers - Left, Firestorm - Right. Firestorm can be swapped to regular mode in the settings in Preferences -> Firestorm -> Build 2 -> Use the New Firestorm Texture panel)_

Those are now referred to as either BP Materials or Legacy Materials. They’re not going away anywhere since there’s way too much content using them, but we’ve grown out of them so it is time to put them in the attic and get some new toys - the PBR materials.

Unlike Legacy Materials, PBR materials come as a little bundle, as you can see in the legacy material edit floater, you can set each of them individually, to a texture of your choice, but with PBR it is a little different:

_(Most viewers - Left, Firestorm - Right)_

As this is just a plain plywood box, we can see that all material related things are just grey X, and there’s no PBR material applied.

As mentioned the PBR comes as a bundle that has:

- Base Colour - similar to the old diffuse, but not the same. A small selection of diffuse textures can be reused. (More about it later)

- Occlusion-Metallic-Roughness - a brand new texture type, commonly referred to as just Metallic-Roughness map.

- Emissive - an emissive texture

- Normal - same as BP normal map. This is the only texture than can be safely reused from Blinn-Phong


## How do we make one?

There are a couple different ways to make a PBR material:

- If you’re a creator, a lot of the software you might be already using can export glTF materials, and you can upload them as any other texture, and the viewer will pick up all of the bundled textures.

- You can create one, just like any other inventory item in SL, just right click a folder and choose New Material:

  - We’ll get to what to do with those in a bit.

- And the last way is to apply PBR materials on an object by hand, and then Save to Inventory

  - We’ll cover those a bit later as well.

Okay, let’s make a material in our inventory and open it up! You’ll see something like this:

Which looks like a big fat version of the edit floater’s texture tab. As you can see we have all 4 of the component textures that are not set to anything, as well as a couple parameters. If you’re uploading the material, then you’ll also see the price, but since we’re just editing a blank one, there are no fees. And as you can see you can either save any changes (assuming we make some), to the current material, or save it as a separate one.

In terms of parameters, we have tint, alpha, alpha mode, alpha cutoff, metallic factor, roughness factor, and another tint. Which as you can see are grouped next to a texture they’re applicable to.


## We have a material, now what?

Well, the obvious thing would be to apply it to an object! Rezz yourself a cube, and drag and drop your new material onto it.

Well… that just made one face of the cube grey… not very fun…

Let’s grab yourself a whole bunch of premade materials, and learn the different ways of applying them. There is a very handy full perm and copyright free pack right here on the marketplace <https://marketplace.secondlife.com/p/AmbientCG-PBR-Material-Megapack/25595913> it contains most of the materials from this website, they share the same names, so it wouldn’t be too hard to find - <https://ambientcg.com/list?type=Material,Atlas,Decal> . Feel free to unbox and have a look through the different materials, you’ll see what texture they have set once you open them up, and if they use any special parameters and so on. Oh and there also are a couple materials provided by Linden Lab in your Library folder. I’ll wait…

Right, so as we’ve seen we can drag and drop a material on a face of an object, and that will apply it to that face, let’s see how else we can apply materials. Edit an object and go to the Texture tab, and if using LL Viewer, select PBR material in the dropdown, if using Firestorm switch to PBR tab. Click the grey X for the Material, and you’ll be presented with your standard inventory object picker dialog, from there it works mostly like anything else, if you have the whole object selected, it will apply the materials to the whole object, if you select just one face, it will apply it to that face.

Now, I said “it works mostly like anything else”, there are some important differences.

- PBR Materials do not remove Legacy materials. That means if you have an item which already has a texture, you can add a PBR material on top of it.

  - Only PBR supported viewers will see the material, while older viewers will still see the BP Material. Some people call this behaviour ‘fallback textures’

- If you set the material to None. It will remove the PBR Material, but not Legacy materials.

  - PBR viewers **must** display PBR material if there is one applied, if there is none, then Legacy materials will be used.

- Blank - Will apply a brand new material to the object without creating an inventory item, you can use Edit Selected to edit the material and Save to save it back to inventory. (Firestorm allows editing in the same UI without having to click edit selected)


## Let’s Make some simple materials

### Basic Metals - Gold

One of the areas PBR really shines is metals, so let’s throw together a very simple gold material, this way we can learn what all the new shiny things mean.

1. Let’s start with creating a new material in the inventory, rename it to Simple Gold and let’s also rezz a simple Torus to be our golden ring… or a sphere… or any other shape.

2. Now, let’s open up the material so we can make some edits. Since we’re doing simple things, we’re not going to make any textures, but we still need our gold to be a golden colour. Thankfully, we have a Tint option for our Base Colour. And this is one of the great parts of PBR and glTF being an industry standard, a quick online search for “PBR gold colour” should yield plenty of very similar results, the first one that showed up for me was “1.000, 0.766, 0.336”, so now let’s click the Tint colour box, which is white by default, and because the colours I have are in 0-1 range, I’ll use the LSL tab to enter them, if you have 0-255 rage colours use the RGB tab, or Hex if you have a hexadecimal colour.

3. Now, unlike when tinting Legacy objects, we do not need to set the Base Colour texture to white, according to glTF standard, if a texture is not provided a default value must be used. Which in case for the Base Colour is pure white (same for all other channels except Normal, normal has a default “flat normal” value instead)

4. Let’s do a quick save of the changes, and apply the material.\
   \
   Hmm… it is kind of yellow, but I wouldn’t call it gold…

5. Open up the material in our inventory again. And let’s have a look at the Metallic Roughness details, normally these values will be provided by the texture, but since we’re being simple, we can use the two numeric parameters. As mentioned before, if no texture is provided a default value will be used, which for Metallic-Roughness texture is pure white, which is 1.0. This value is then multiplied with the two Metallic and Roughness factors. Those two factors have a few similarities to the old Glossiness and Environment values, but they’re very different.\
   Metallic value indicates if the material is a metal or not, if it is a metal, the value should be 1.0 and if it is not metal, then 0.0. While you can set any value in between, they’re not actually realistic and are only possible because of maths stuff, however, if you have slightly rusted metal, metallic of 0.95-0.99 might work. And of course if it fits your creative vision, which does not require full realism, you can use any value that looks good.\
   Roughness value is a close equivalent to the old shininess, it indicates how rough the surface is, with 1.0 effectively being something like sandpaper, and 0.0 being a perfect mirror.

   1. Side note: there should also be Ambient Occlusion strength value, but LL decided not to implement it yet, there is a feature request on the Canny I believe, so if you’re a creator that needs it, you should find and upvote it

6. Now then, since the two values are just 1.0 Metallic, 1.0 Roughness, we need to tweak them a bit. Since gold is definitely a metal, we can leave Metallic at 1.0, but most rings are pretty smooth overall, though not quite mirror smooth… so let’s try something like 0.3 for Roughness. And save the changes again.

   1. One thing you might notice, that while the material is saved, our object did not change. Every material save is effectively a new copy of the material, and existing objects using it are not affected.

7. Let’s apply the material to our object again.\
   \
   Now that looks a lot more golden to me!

8. But you know what, I think I want an expensive ring, that’s well polished and super shiny, let's set the roughness to 0.12 and reapply the material:\
   \
   Shiny! We can even make out the reflections of the room now!

You can use this method to create any kind of metal, for realistic values you can look up colours online. If you want more realistic textures, you’ll have to make some to add some scuffs or dents, or add a brushed metal effect, or find or buy a premade material that fits your needs.

Also worth noting that ‘Roughness’ value is non-linear, so small changes have significant differences, in PBR maths that the graphics card does the roughness value is square and in some cases square twice, so it is quite exponential.

Some other metal colours:

Iron (0.560, 0.570, 0.580)

Silver (0.972, 0.960, 0.915)

Aluminium (0.913, 0.921, 0.925)

Gold (1.000, 0.766, 0.336)

Copper (0.955, 0.637, 0.538)

Chromium (0.550, 0.556, 0.554)

Nickel (0.660, 0.609, 0.526)

Titanium (0.542, 0.497, 0.449)

Cobalt (0.662, 0.655, 0.634)

Platinum (0.672, 0.637, 0.585)


### Basic Metals - Let’s DIY

**So, first of all, a warning - this will not work on all items, and will be very dependent on the type of item and techniques used to make it. In a lot of cases you’ll need at least a normal map, which most creators do not provide, and in many cases the base texture will have important details that will be hidden by PBR.**

And of course, in order for this to be possible in the first place, your items need to be mod, if you’d like to know more about why I think mod items are superior, you can read about it over here [Mod vs No-Mod](https://docs.google.com/document/u/0/d/1Kosnx9oPTZMrMixtH35zvKUhDC2XlVt2HKZ-uH7csOM/edit)

Having said that…

1. So I have this cute pair of boots:\
   \
   And they have some metal hearts and buckles…

2. Now, I can open up my edit tool, select the faces of the hearts and the buckles, and apply a quick blank material, and set metallic to 0.2 keeping the tint white. And (I’ve changed only the left boot for side by side comparison):\
   \
   Especially when there are bright objects in the room:\
   \
   The room setup - so you can see where the red and blue colour is coming from:


### Basic Latex - Let’s get Shiny

Latex is a material that has some unique properties when used in SL. First of all, latex items generally tend to be smooth, and in many cases don’t even need a normal map. This is excellent for DIY-PBR-ification.

1. So I have this cute little apron\
   \
   But… it has this white “highlight” on it… which doesn’t make sense… I don’t really have any lights in front of me, and if you look at my arms and legs, you can see shadows in the front, meaning there are lights coming from the sides…

2. Let’s go apply a PBR material again, this time we’ll use black tint for black parts, and white for white ones, since Latex is not metal, we’ll set Metallic to 0, and roughness 0.1 is decent enough starting point for smooth latex.\
   \
   Now this is a lot better, isn’t it? I can see the red and blue walls reflecting on the sides, and you can just about make out some part of the room as well. And best of all, the effect is fully dynamic - it looks way better in motion, well, all PBR look way better in motion:


### Basic Glass

Since we do not have glTF transmission in SL just yet (coming… eventually). The best we can do is a transparent metal.

Set Metallic to 1, Roughness to 0, Alpha mode to Blend, and alpha anywhere from 0.1 up to 0.6 - I’d highly recommend experimenting with, depending on the scene and lighting settings, as Alpha will mostly determine the reflection strength. Also, the colour should be near white, but you can add a touch of green or blue, something like RGB 237, 255, 246 or RGB 237, 252, 255. If you're making glass bottles or stained glass, you can make the colour a lot stronger.

Here’s an example of glass at 0.1 Alpha with Midday settings:

And here’s an example of 0.5

The red cube is in front of the glass and is being reflected by a basic reflection probe, and the blue cube is behind the glass.

Depending on the scene, you could even set up a mirror probe in front of the glass, for even more realistic reflections.


### Basic Emissive

Emissive with PBR works quite differently than Blinn-Phong emissive. First of all it is not tied to a special alpha blend mode, that turns alpha channel into emissive strength, with PBR, it is its own completely separate channel, that can have a different colour than the base texture.

Emissive should be used for things and parts that glow, and the texture show contains the base colour of that glow - for example a billboard might use the same texture as in the Base colour, as the entire billboard is likely to be glowing, the billboard frame should not be glowing. Another use would be to have lava and the patterns between solidified rock glowing.

The colour multiplier, which defaults to black, allows you to adjust the strength of the glow for each channel… or make it glow a solid colour if no texture is provided.

Here’s an example:

Left is Blinn-Phong Full-Bright, middle is PBR Base Colour and Emissive Texture with the same texture and colour emissive tint as white, and the last is a completely different Emissive texture.

Here’s the same exact setup at midnight:

As you can see, on the right most box, the base texture becomes barely visible, as there are no lights in the scene, and the Emissive texture is glowing as expected.

Here’s an example of glowing Lava on a moonless night:

And here’s during the day:

You can now make out all the details of the rocks illuminated by the sun, while before only the red hot parts were visible.

Glow is a tricky one… it is not quite finished yet and is subject to change and adjustment, as current implementation does not match glTF standard. So in general, currently it works nearly the same as BP glow, except it does need to have an emissive colour or texture for parts to glow. And the glow colour is determined by the colour of the emissive channel:

Here’s 0.5 Glow on all 3 cubes from before:

**Note**: Make sure any 3rd party software you’re using is not exporting a full black 1k/2k texture into the emissive channel, when not using emissive, leave the texture blank, and leave the emissive colour as black - this will proper disable it without viewers having to waste memory and bandwidth downloading a 2k pure black texture. That’s 12.5MB of VRAM for nothing!


## Tips and Tricks

- Another way to understand the Metallic factor is “Additive” vs “Multiplicative” reflections.

  - With metallic 0, Additive reflection is used, so: Final Colour = Base Colour + Reflected Colour. So if you have black object, which has a colour of 0, when it reflects something white, colour 1, you get 0 + 1 = 1, which results in a bright white spot on the dark object, think of it as a window reflecting on a TV or mobile phone screen that is off.

  - With metallic 1, Multiplicative reflection is used, so: Final Colour = Base Colour \* Reflected Colour. So if you have black object, which has a colour of 0, when it reflects something white, colour 1, you get 0 \* 1 = 0, which results in pure black. However, if we take the example of gold material we used before, it has a very low blue component, and middle green, so reflected light becomes tinted that shade.

  - This is why metals need to have bright colours to have strong reflections, and nonmetals need to be dark colours to have strong reflections. Because if you have a white item that receives white reflections 1 + 1 = 2, however colour maxes out at 1, so you still get just 1.

- For certain materials that tend to have uniform colour, such as smooth plastic, latex, some leathers, painted cement, some fabrics, glass, metals, etc. you might find that a base colour texture is not needed and just choosing a tint colour does the job as the Metallic-Roughness and normal maps combined add majority of the detail.

  - Obviously this won’t work for anything with multiple shades of colours or patterns. Or where you do want more detailed patterns or shades.

  - It would however save on texture upload costs, as well as on viewer asset download and rendering impact.


# Other Considerations

## Texture baking - aka shine, reflections, highlights

So, as you might have seen in the [Basic Latex - Let’s get Shiny](#basic-latex---lets-get-shiny) section, loads of SL items have baked in reflections, which remain static at all times, those should have been gone when ALM was added as specular and normal maps can create a similar effect. However PBR takes it way way further with addition of reflection probes, meaning you can have real reflections of the stuff around you in the room. See my guide - [Reflection probes and You](https://docs.google.com/document/u/0/d/18ut5mR_S9sAYDwWvFHNpRqrJ31y2hpqVSZHd8sbeua4/edit) . Now, since we have real reflections now, there **should be no** baked-in lights, shadows, highlights, reflections, shine, etc. on PBR textures. The base colour is just that - base, it must have just plain unlit base colour. From wiki written by Linden Lab: “This differs from the “Diffuse” texture that Second Life uses. Diffuse textures often include faked reflection and specular information as well as added Ambient Occlusion shadows. **Base Colour textures do not get any of this added information**” (See: <https://wiki.secondlife.com/wiki/PBR_Materials> )

In current SL Implementation of PBR only Ambient Occlusion is a map that is baked in, but that has its own dedicated Red channel as part of Occlusion-Metallic-Roughness map.

Not baking unnecessary textures, saves you a lot of work as a creator, and makes your item look realistic in all scenarios - having shine and reflections on metal, latex, plastic, any other shiny polished objects while in a dark room makes no sense.

And for non-creators I’d like to encourage you to avoid items with baked-in shine and reflections.


### Environment boxes

You might have seen some so-called ‘environment boxes’ or ‘shine boxes’ that use projector lights to shine a picture onto your shiny clothes, whether they have a picture of an italian village, photo studio or just some coloured patterns, what they are actually trying to achieve is what the reflection probes do. Effectively reflection probes are the proper way of doing what those ‘boxes’ were trying to mimic.

Also those do not display the texture on PBR materials. This is intentional, so people would stop using them, also it would be nice if you'd stop using facelights too. Thanks!\
\
_I like realistic lights and reflections, okay?_ 😛


## Reusing BP Textures

Let’s start with the official warning from SL Wiki:\
“Warning: While there may be a temptation to try and place a Blinn-Phong “Diffuse” texture into the “Base Color” slot and a Blinn-Phong “Specular" into the “Metallic/Roughness” slot, doing so will not produce desirable results. However, trying this won’t generate any warnings as there is no way to check that you’ve put the right texture type into the inputs, and the material will save and be applicable to objects. Unfortunately, what will be created is not a functional PBR material. Please do not do this.”


### Diffuse

As explained in the Texture Baking section, PBR Base Colour texture is not supposed to have any baked stuff, while a lot of existing BP textures do, which means reusing those as PBR materials would lead to incorrect results, and generally would just look bad. However, if you have a texture that does not have anything baked in, it might work reasonably well, but may not be colour accurate.


### Specular

Current PBR materials do not have a Specular texture, while glTF has a standard for specular colour, LL has not yet implemented it… yet, If you’d like to have it, I’d encourage you to forward your requests to LL so they can add it in the future.

However, even then, the current legacy specular textures will not work - they use the alpha channel for a SL specific “specular exponent”, and glTF has a specular strength in the alpha value, which is used in maths way differently from each other and therefore is not compatible.


### Normal

Normal map texture can be reused. However, similar to specular texture the alpha channel is used by SL in as “Environment intensity”, however glTF specifies that alpha channel on a normal map should be ignored, therefore using legacy normal maps will not have side effects. Having access to a normal map, can enable you to mod your old items to use PBR materials without losing too much detail.

It is worth noting however, that when uploading glTF materials directly, the normal map is uploaded with lossless compression, providing higher quality to the final material, while all of the legacy normal maps uploaded prior are compressed in a lossy way. (Other channels of the texture are compressed in the lossy way)


## Double-Sided

You might have noticed a double-sided checkbox in the material edit window. Double-sided makes the face the material is applied to double-sided. You might have seen plenty objects that behave like this:\
\
Where a face is only visible from one side. This is great for solid objects that don’t have interior, or are not meant to be looked at from that side. But in scenarios where you do want the object to be double-sided you’d have to make the mesh in such a way that it has a second layer of vertex data, all facing the opposite way. Enabling double-sided will make the viewer re-render the existing face with inverted triangles as well, making it visible from both sides.


### **Beware!**

Before using this options please read about it on the SL Wiki <https://wiki.secondlife.com/wiki/PBR_Materials#Double_Sided_Parameter_:_Uses_and_Dangers> and pay special attention to **“since placing materials with this parameter checked on normal objects will simply cause the viewer to draw it twice ( and thereby create additional viewer lag ) for no observable change.”**


## Scaling and offsets

PBR and BP materials use slightly different coordinates, BP are centred, PBR are based from a corner. In short, in some cases you might need to adjust UV offsets by 0.5 to make it match existing mesh. For more details see:\
<https://wiki.secondlife.com/wiki/PBR_Materials#Texture_Transforms>


## Editing Legacy Materials

One thing you’ll quickly notice, that applying PBR materials stops you from editing Legacy materials. This is intentional, as mentioned before the viewer must show PBR materials if PBR materials are applied. Therefore you should not need to change any legacy parameters once you have PBR, and Linden Lab is encouraging creators to only use PBR materials and leave Legacy materials in the past. Sure, you might want to use Specular texture in some cases, which is not available for PBR.

If you do need to change Legacy materials, you’ll first have to remove PBR material, do all the changes and reapply PBR material again later.

_Note: Firestorm custom UI does allow changing BP and PBR without removing PBR._


## Firestorm Custom Texture UI

While this new UI layout is quite neat, and compact, it has few issues for the future - glTF specification defines many other materials that are not yet available in SL, such as Transmission or Clear Coat or Iridescence, which will add additional textures and properties, which are unlikely to fit on that UI. Which means that the UI might have to change a lot over time, or revert back to the pop-out material editing, since that has a lot more space for a lot more stuff.


# PBR Material Overrides and Permissions

Okay, so this one is the super confusing, but also important part about PBR Materials. As mentioned before, if PBR material is applied to an object, the viewer must show it instead of Legacy materials; that part is simple enough, however - there are two layers of PBR materials on each object.

When you create a Material in your inventory, setup all the textures and parameters, you end up with a “Material Object”. That object has a UUID, which represents the entire bundle of textures, settings etc. as a whole. Now, you can then apply that object onto any face of any rezzed object. However, this is treated as applying a single texture to it. So that means when you edit a face of an object, you’re not editing the “Material Object”, but instead you’re editing a “Material Override” - I don’t know if these are the official terms, but I will use them in the rest of the guide to distinguish between the two: the “Material Object” being the inventory item or its UUID, and “Material Override” being adjustments made of the material that is already applied to an object.

The way Material Override works, is that the viewer applies the Material Object, and then it applies any Overrides on top of it. So for example, if you apply a gold material to an item, and then change PBR tint colour to something else, you’re actually changing PBR Material Override tint colour.

Firestorm custom UI shows it off quite well:\
\
As you can see the Material is gold, but the tint colour is red. The UI also changes the name of an active override to brown.

Editing the object manually always edits the overrides. Editing the object with scripts always edits the overrides. Unless you’re replacing the actual material and not the individual textures/parameters.


## So, what exactly does that mean?

This mostly has implications to scripts and creators. I do not think the viewer can currently clear an override (please do let me know if it can and how) without you manually re-applying the material object. Meanwhile the scripts can edit PBR material properties and set the override to “” which will clear it, and if there is a “Material Object” applied to it, it will show up again.

This goes into detail about the overrides, but I don’t think it explains the full-picture <https://wiki.secondlife.com/wiki/GLTF_Overrides> so I’ll try to explain what it actually means.

Scripts can only change materials in two ways:

1. Replace entire material using UUID or inventory name (if the object contains the material object)

2. Replace specific parts of the material, such as texture or tint colour or metallic factor or alpha and so on, using llSetLinkPrimitiveParamsFast() and providing the data for the overridden parts.

Both of these methods have different implications. Especially for creators making texture or applier HUDs.

First of all, let’s be very clear on this - Material Object UUID works the same way as Legacy texture UUIDs, in order for scripts to read it, the item must be one of the two:

- Copy **and** Mod **and** Transfer (aka, Full-Perm)

- Have the Material Object inside the inventory

  - **However!** This will give scripts the Material Object name and not the UUID

Under every other condition, the Material Object UUID read by scripts will be 00000000-0000-0000-0000-000000000000.

That means it is safe to sell ‘Copy and Mod and No-Transfer’ items without exposing your Material Object UUIDs.

Another important point, as mentioned above, LSL can only read or write Material Overrides when trying to change individual properties of the materials. That means, there is no way for a script to read individual texture UUIDs from the Material Object, no matter the item permissions. Textures inside the Material Object are fully protected. Viewers also do not show UUIDs of copy/mod materials, or even full-perm materials for that matter.

Note, from my earlier testing: this might be viewer dependent (I’ve used Firestorm Beta 7.1.7), no-mod materials preview the textures when clicked, while mod materials bring up the texture picker dialog. So technically, one could screenshot the texture preview on a no-mod material, but not on a yes-mod material.

Now, Material Overrides are different, as mentioned LSL can read and write them. Since overrides are effectively changes on top of the base material, scripts can apply them without any impact to the underlying Material Object that is applied. Using the example above, where I’ve tinted the gold with red, the script could do the same, however after changing the tint, it cannot read the original tint value from the Material Object - so it has no clue what RGB value the gold is. In order to get around that, the Override can be cleared, by setting it to “” (an empty string), this effectively tells the viewer “this property is not overridden, please render whatever colour is defined in the Material Object”

The viewer only allows Full-Perm textures to be set as Material Overrides as well as in the Material Objects. Which can be frustrating when modding items, as various texture modding kits that come with copy and mod and no-transfer textures cannot be applied as PBR. My understanding is that this is because of the ‘Save to Inventory’ feature that could potentially lead to converting restricted permission texture into a full permission material. This means any textures you might have sold in the past, are still safe from being copied and transferred.


### Example of how the Material Objects and Overrides **should** be used

Okay… Let's put it all into an example to see what it actually means for creators. Let’s say you’ve made a pair of shoes that come with socks. It is quite common to have the ability in the HUD to hide the socks, for people who only want the shoes, or have their own socks.

1. Create Material Objects for your socks, for example - one Material that has stripped socks, and one that has polka dot socks.

2. Create a HUD system that applies those two Material Objects using the Material Object UUID.\
   (Using either llSetRenderMaterial or llSetLinkPrimitiveParamsFast with PRIM\_RENDER\_MATERIAL)

   1. This will enable users to swap between two materials like they're used to, but this will also protect your Material Object and Texture UUIDs even if the shoes/socks are mod, as described before.

3. Setup your ‘Hide socks’ button to set the PBR Alpha value and blend mode that hides the socks.\
   (Using llSetLinkPrimitiveParamsFast and PRIM\_GLTF\_BASE\_COLOR)

4. Setup your ‘Show socks’ to set the PBR Alpha values that have been changed by the ‘Hide Socks’ button to just “”, this will clear the override and restore the original material values.


### Example of how Overrides **should not** be used.

Do not make your HUD set individual textures as overrides, this can be bad due to multiple reasons:

- Material Overrides can be overridden by other scripts, for example when making the item show/hide specific face under various conditions. If the UUID of a texture is in override, or any other parameter is, a script that does not expect that could accidentally override the value and clear it when it should not. Having it as part of the base material prevents that.

- Especially if you’re a body/head creator, and your item allows third party creators to make appliers for your item. Setting entire material at once, which is not only more efficient, but also future proof. As more material channels are added over time, such as Transmission, Clear Coat or any other glTF extensions that support additional textures, the same script that can apply material as a whole, will work to apply any new updated materials. This is a big benefit of the Material Objects.\
  If your script only sets overrides, you would have to release an update to the scripts to make sure any few features are supported.

  - You can implement features to change tint or roughness, or other properties as part of your scripts using overrides, as clearing the overrides gives you a way to revert back to the original properties safely.

- **Unconfirmed**, take with a grain of salt: I was told that changing just the material, and keeping overrides to a minimum is better for network performance as there is a little bit less data sent to the viewer when changes happen or when an object is rezzed.


## Material Object Permissions

Material objects have the usual Copy, Mod, Transfer permissions just like any other SL object. However it does have some nuances.

- Texture UUIDs cannot be extracted from Material Objects, even if they were given to you as Copy and Mod and Transfer.

- No-Copy, Mod, Transfer - will prevent you from saving the material as a new material even if you make changes

- No-Copy, No-Mod, Transfer - Will allow you to preview the Materials floater and see the texture previews as well as numeric parameters, as well as preview larger versions of the textures, but you will not be able to change or save any more copies of the texture.

- Copy, No-Mod, No-Transfer - Will allow you to preview the Materials floater and see the texture previews as well as numeric parameters, as well as preview larger versions of the textures, and will also allow saving out new copies of the material. However the material itself cannot be changed.

- Saving out a modified No-Transfer material will keep its No-Transfer permission.


## Tips and Tricks

- Making overrides might be easier, especially with the Firestorm’s compact UI when compared to editing material in the inventory and reapplying it all the time - this is where the Save to Inventory button comes in, it converts Material Object that is on the object already and any Overrides into a new Material Object and saves it you your inventory. You can then use this new material on the object, that will clear all the Overrides for you, and you can also use UUID of that new material in HUDs and such.


# Other Guides

Other handy guides, that will cover the same or similar topics - not made by me. (in no particular order)

- <https://lindenlab.freshdesk.com/support/solutions/articles/31000173550-2024-materials-graphics-updates-faq>

- <https://www.youtube.com/playlist?list=PLcX5gwjHNEnD7cVEQcGFqzOrcXAF4EU7h>

- [Second Life University - Introduction to .glTF](https://www.youtube.com/watch?v=z96ZhS2dL6s)

A specialised guide focused on scripting related to PBR alpha show/hide functionality (written by me) [Scripting PBR Alpha](https://docs.google.com/document/d/18mou3gLccEbSRDxWDNCmGTz3-oCJdU-2xATfxYywo3o).


# Known Bugs / Unconfirmed Behaviours

## Potentially Unexpected Material Override behaviour

Currently scripts can always read Texture UUIDs in Material Overrides, but according to:\
\[2024/07/09 12:13:41] Brad Linden: the PBR texture UUID permissions should follow existing patterns for how plain texture UUIDs work in LSL.


## Overrides for No-Mod Materials

According to the glTF Override wiki page:\
<https://wiki.secondlife.com/wiki/GLTF_Overrides>\
“When a no-mod glTF material is applied to a prim’s face, its glTF overrides cannot be modified, with the exception of texture transforms.”

This restriction only applies when using No-Mod materials given by someone else. So if someone gives you a no-mod material, you won’t be able to change the overrides on the face you apply it to.

If you make your own materials, and apply any combination of permissions, including:

- No-Mod

- No-Copy

- No-Copy and No-Mod

And then use that Material Object and apply it on an item, and then give that item to someone as Copy and Mod, they will be able to change the Material Overrides.


## UUID Leak from Material Object

While everything in the previous section is correct and how it is supposed to work, due to a bug:\
<https://feedback.secondlife.com/scripting-bugs/p/pbr-individual-material-uuids-accessible-via-llgetprimitiveparams-on-non-full-pe>\
The underlying texture UUIDs can be exposed from the Material Object. Once this bug is fixed, your materials and textures will be fully safe as described in this guide.

\
\


Written By: Kristy Aurelia - but please don’t poke me about it randomly in-world (Discord IMs are okay), unless there are some important mistakes or incorrect information. If you see me in a conversation in some group chat or Discord or in-world somewhere - where I am clearly not in the middle of anything, then it is fine. Thanks. Feel free to share the link with anyone if you find it useful. Feel free to share the information from this guide with or without attribution, as long as the information is correct, the more people know it, the better for everyone!
