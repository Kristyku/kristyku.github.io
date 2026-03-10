# Mod vs No-Mod

# Introduction

I've discussed Mod vs No-Mod many times with different people, so I’ve decided to write it all down, so I don’t have to repeat myself.

Disclaimer - This is my personal opinion, and it is pro-Mod. I believe that SL should encourage creativity.


# What does Mod permission actually mean?

First of all, let’s have a look at what Mod permission actually means. By default we’ll assume ‘Yes-Copy, No-Trans’ and see what difference ‘mod’ makes.

**For Meshes and Pirms:**

- Allows - Linking and unlinking parts

- Allows - Resizing (except for rigged meshes)

- Allows - Setting tint, glow, transparency etc.

- Allows - Changing textures (including normal and specular textures) and PBR materials

- Allows - Adding or deleting contents, such as scripts, or sounds or animations etc. (Note: items can be taken out from no-mod items as copies, many product boxes are no-mod, but they can still be unboxed)

- Does not allow - Pulling out texture UUIDs of applied textures. Even with custom scripts, in most cases, the script will return a null key for texture UUID or an empty string for PBR textures. Texture UUID is only returned if the item is full-perm (Copy and Mod and Trans) or the texture item is in the contents of the object.

**For Notecards:**

- Allows - Editing the same notecard.

  - Do note, that if copy permission is enabled, it will be in read only mode, someone could still copy out the continents to make their own copy, but the creator of that notecard will be different.

**For Scripts:**

- Allows - Seeing the source code, same as notecards, could be copied out and modified. **Mod scripts are effectively Full-Perm!**

**For Sounds:**

- Allows - Renaming (applies to most things).

**For Textures:**

- Note - Only full perm textures have the Save As or Copy UUID buttons. (Copy - Mod - No Trans won’t have the buttons)

- Allows - Renaming.

**For PBR Materials:**

- Allows - Textures to be replaced/removed.

- Allows - Settings to be adjusted.

- Does not allow - Pulling out texture UUIDs of applied textures.

  - This might be viewer dependent (I’ve used Firestorm Beta 7.1.7), no-mod materials preview the textures when clicked, while mod materials bring up the texture picker dialog. So technically, one could screenshot the texture preview on a no-mod material, but not on a mod material.

- Does not allow - Transferring modified materials if part of the textures or settings were changed.

**For Animations:**

- Allows - Renaming (applies to most things).

**For Gestures:**

- Allows - Modifying the gesture and adding or removing commands from it.

- Does Not Allow - Pulling out sound or animation assets out of the gesture.

**For Tattoos:**

- Allows - Swapping out/removing textures.

- Allows - Tinting.

- Does not allow - Pulling out texture UUIDs of applied textures.

**For Shapes:**

- Allows - Editing the shape sliders and seeing the slider values, which could be copied or told to other people. **Mod shapes are effectively Full-Perm!**


# Why should you make your items Mod?

As you can see in the previous section, mod permission on meshes allows the users to tweak various things.


### Advantages to the users

Here are a bunch of scenarios, which are enabled by having mod permissions, that are advantages for the user:

- **Resizing unrigged meshes/prims using the Edit menu** - it is a lot more precise than resizer scripts, also allows you to delete or not even use a resizer script. The Edit menu allows resizing using drag-handles or even numerical values for superior precision compared to the common scripts just increasing or reducing size by x%. It also allows scaling separate linked parts for the best fit.

- **Remove contents** - Resized your unrigged item to your liking, or did you customise the item using a HUD? Well, you can throw away the scripts now, while in theory that should have no impact (as idle scripts should not do anything and therefore not cause any performance issues), some poorly made scripts could impact sim’s performance, also reduced memory usage is always nice. Need to customise the item again? Unbox it again or get a redelivery.

- **Link and Unlink Parts #1** - Have you ever bought a pair of shoes that came with a rigged left and right shoe? Have you ever worn them separately? I have not, and now they take up 2 attachment slots. Link them together to save an attachment slot!

- **Link and Unlink Parts #2** - Do you have a set of rigged earrings/piercings? Did you hide a bunch of them and use them in combination with another set from a different brand? Is it taking up multiple attachment slots? Well, you could unlink the piercings you’ve hidden and remove them, then link the remaining ones all together back into a single attachment! Need to customise the item again? Unbox it again or get a redelivery.

- **Link and Unlink Parts #3** - Does your hair come with multiple styles controlled via a HUD, do you just like one of those styles? Well, you could unlink and remove all those extra invisible styles, so you don’t have invisible parts using up avatar complexity and increasing rezzing times. As usual, re-unbox or get a redelivery to recover the removed parts.

- **Link and Unlink Parts #4** - Do you use deformers for your body? Mod deformers and body could be linked. Or deformers can also be linked with each other.

- **Hiding Faces** - Does the item have multiple faces and would go well as part of an outfit with a bunch of other items if one of those faces was not shown? Well, select the face and make it fully transparent!

- **Tinting** - Does the item have a white texture? Well, you can apply it and use the Edit menu to tint it any colour you like, without being limited by the options in the HUD. Perfect for getting that same shade of your favourite hair colour or all your hairstyles!

- **Changing Textures #1** - Are you a fan of shiny rubber clothes? Well, remove the diffuse texture, and in some cases normal map, add a blank specular texture and bump up those Specular/Environmental values - congratulations, you now have a shiny latex item! Want the original texture back? Use the HUD or Unbox it again or get a redelivery.

- **Changing Textures #2** - Do you have a nice tiled texture? Maybe it looks great on the item of your choice. Give it a try! Want the original texture back? Use the HUD or Unbox it again or get a redelivery.

- **Scripts #1** - got a new shiny choker or a collar, and would like to join some adult activities? Well, drop in your favourite RLV scripts and you’re good to go!

- **Scripts #2** - (For Maitreya Lara) - Is the item of clothing causing your body to alpha out too many or not enough parts? Well grab the alpha script from your body provider, throw out the old one, and set up a new one to your liking - it only takes a few minutes.

  - Or was the item using Maitreya Alpha script v4 and does not show hidden body parts after the item is removed? Well, you can update it yourself to v5 and fix it! (Would be better if the creators did it, but they do have loads of items to update, and many different bodies which may not have the issue)

- **Scrips #3** - (For Maitreya Lara) - Did you know that shoes can auto-select feet shape automatically? Oh… the creator did not include that script? Well, you can set one up yourself!

- **Fix Hair Alpha Glitch** - Does your hair have a weird outline because your clothes have transparent parts? Did you know you can fix it by changing the order the attachments are worn in… but… sometimes you need to make the root prim slightly transparent… if it already has a random box, set it to 1-99% transparency. If it does not, link it to one, shove it inside your head, and set the transparency. More details: <https://slalphaglitch.tumblr.com/> or <http://beqsother.blogspot.com/2022/11/alpha-blend-issues-get-them-sorted.html> 

  - Ideally the creator should make sure it is that way from the very beginning, but some creators have made hundreds of hairstyles before this fix was implemented.

  - Same thing applies for any other items with transparency too, you’ll just have to use different attachment points.

- **Updating metal parts (PBR)** - does the item have metal parts, do you want it to be actually metallic looking and shiny with new PBR Materials? Well, set one up and apply it. No more fake baked in reflections! Works perfectly for latex too.

  - Most metal parts don’t need a diffuse texture, therefore replacing it with a solid colour material is fine.

  - This can work for non-metals too, but might require a base colour texture or a normal map to look good, so it will depend on the specific item.

- **Rezzers** - Various ‘rezzer’ systems require a script to be placed inside an object. Which allows you to set up multiple scenes for the same room, and ‘rezz’ or ‘derezz’ them at will, which is great for stuff like holiday decorations, seasonal details, or just having multiple room setups, especially on small parcels where you don’t have space or LI to have both at once.

- **Renaming** - renamed sounds/animations/textures can be used in cases where you have no-mod scripts that use these assets by name, by renaming and swapping out the assets, will make those scripts use the assets of your choice.

- **Fixing creator mistakes** - A real example: someone was furnishing their house and a light was showing up wrong on the ceiling - that was caused by a normal map being black, removing the normal map fixed it. (Normal maps do not encode colour data, but positions, and all black is not “blank”, a specific shade of blue is 127, 127, 255)

**How does enabling mod affect the creators? It must be so much effort! Guess what - it is literally just ticking a checkbox!**


### Advantages to the creators

- **Wider target audience** - People who want to do any of the things listed above, might buy your products.

- **Potentially a more customisable item** - If you have the right skills, and your item can take advantage of it - you could make it with a grayscale texture and tintable, skipping making all the different colour variant textures and allow people to tint the item to any of the 16,777,216 colours.

- **RLV Items without scripting knowledge** - Do you make awesome looking BDSM items, but can’t script? Well, leave them as mod, and allow users to put in their own favourite RLV scripts!


### What about disadvantages?

For users:

- Linking and Unlinking parts might break HUDs/Scripts.

For Creators:

- Some people might ask how to mod X or how to fix something after breaking X.

  - Provide a disclaimer saying - if you mod, you’re on your own.

- People might customise it in a way that does not represent your creative vision.


# Where No-Mod makes sense

## Fat-packs vs Singles

It is okay to have Singles as no-mod, but fat-packs should be mod.

Single being mod, would enable someone with a fat-pack to “acquire” a texture UUID and share it with their friends, who bought a mod single. So that is a clear downside to the creator. But, since applying a different texture requires mod permissions, and fatpack already includes all of the texture options, there is no reason not to make it no-mod, they won’t need “acquire” a texture UUID if they already have all of them in the HUD, and since the single is no-mod, they can’t steal and share it to be used with singles anyway.


## Reverse Engineering/Cheating

The vast majority of yes-mod supporters are concerned about mod clothes, furniture, houses, etc.

I think most of us can agree that there are a few categories of items that do need to be tamper proof like: vendor systems, shop gift cards, board games, RPG HUDs, etc.

Another, I’d say, partial concern, but one that can be worked around is - Applier scripts for bodies or other products where 3rd parties can make additional content.

If someone made a script that intercepted a skin applier message and grabbed the UUID to make their own copy of the applier, that would be bad. However, that is extremely unlikely, a script can only listen to 65 channels at once. Meaning to figure out what channel applier script is using, one would have to make the script open those channels, apply the applier from the HUD, look for any response, and repeat… repeat it up to 4,294,967,295 - 20,000 (let’s assume you’re not going to use channels -9999-9999 for the applier script as that would be too obvious) divided by 65 channels in one go = 66,076,112 attempts at guessing the channel, if one guess attempt takes 2s, to click the HUD, to wait for script to respond, and change to the next set of channels… that would take 132,152,224s, which is 4.19 years, of continuous guessing. Now, if the applier scripts use any kind of basic encryption as well, that would make it a pretty much impossible task.


## “If you’re asking for things to be mod, scripts should be mod too”

There are cases where scripts could be mod too. However, as mentioned earlier - a mod script is effectively full-perm. Having a mod mesh does not allow people to resell it or give it away, a mod script however would allow doing that, which would be an equivalent of mesh creators providing .dae and .psd (or equivalent) files with all of their creations - however, most of us are not asking for that.

It would be quite lovely if no-mod script did come with configurable features though, but of course that would require more effort on the creator's part. For example, a script that toggles light on/off, there are plenty of free ones out there, so it could be mod. It can be quite annoying when a lamp is way too bright, or the wrong colour, and you edit it to fit in with the PBR lighting system and then toggling it on/off resets it.

But there are also cases where scripts are used to change textures, if those scripts were mod they would expose the UUIDs, which would go back to the “Fat-packs vs Singles” issue.

And of course there are cases where scripts are the main product so making them mod, would make it quite easy to give away or re-sell, since then a reseller can just make the script no-mod and you wouldn’t know that it was taken from somewhere else.


## “Because we reserve the right to set any permissions we want, and don't really owe anyone any explanation.”

Fair enough I guess, I rather it wasn’t the case and I hope advantages of mod I’ve mentioned in this document will change your mind, and if not, we also reserve the right to not buy any no-mod content.


# But, but, but… Counter arguments against no-mod concerns

- **_But most users don’t need mod permission!_**

  - Option to mod, does not force them to mod, those who want mod - can mod, those who don’t - don’t have to.

- **_I don’t want people to reverse engineer my stuff!_**

  - Things in the “Reverse Engineering/Cheating” section do make sense.

  - How exactly does one reverse engineer a mesh? You can already see all linked parts and faces of an object without having mod permission, you can see what textures are being used by just looking up close. That won’t give you texture UUIDs and such.

  - In terms of scripted functionality, it is fine to have no-mod scripts.

  - If someone reverse engineers communications between your scripts, that doesn’t oblige you to keep your stuff compatible with theirs. If a future update breaks their custom functionality, that is not your fault at all. This might sound repetitive, but: put in a disclaimer - “mod at your own risk”. However in some cases that might be a benefit, if someone makes a cool plugin that people like, they will have to buy our original item from you anyway.

- **_My mesh will be stolen!_**

  - No-trans permission prevents resale. And if they use an illegal “hacked” viewer, they don’t even need to own it to steal it, the permission doesn’t even matter, all they need to do is to see it anywhere in SL. Because Second Life Viewer is open source, an experienced programmer with enough motivation can intercept and export any data that is sent to the viewer, which includes meshes, textures, animations, pretty much anything, except scripts. As soon as that data reaches the user's PC, if they have the will and the skill, they can steal it and reupload it. Unless Second life migrates to a fully remote play system, where only the video stream is sent to the end users, stealing is possible. So yeah, mod or no mod, that makes no difference.

  - Linking a no-trans item to another item that is yes-trans will make the final object no-trans.

- **_People will ask me to help them with weird questions about modding things!_**

  - Put in a disclaimer saying - “mod at your own risk”. People who do want to mod, tend to be experienced enough to do things themselves, and those who are not experienced will fall into the “most users don’t need mod” category anyway.

  - You probably get as many questions about “is this mod?” or “can I have a mod version of X?” anyway.

- **_Linking and unlinking will break my scripts!_**

  - Put in a disclaimer - “mod at your own risk”

  - Make scripts that don’t rely on link numbers. I am aware that this is additional effort. Again - “mod at your own risk” fits here.

\
\


Written By: Kristy Aurelia - but please don’t poke me about it randomly in-world (Discord IMs are okay), unless there are some important mistakes or incorrect information. If you see me in a conversation in some group chat or Discord or in-world somewhere - where I am clearly not in the middle of anything, then it is fine. Thanks. Feel free to share the link with anyone if you find it useful. Feel free to share the information from this guide with or without attribution, as long as the information is correct, the more people know it, the better for everyone!
