# Quake Engine Crash Course

## Introduction

I've seen many reddit posts, forum discussions, and extremely outdated wiki posts attempting to give an answer to the unsuspecting developer seeking to figure out how to actually use the Quake engine to make a game from scratch. Often times this individual is linked to the GPL release of the [source code](https://github.com/id-Software/Quake) on the iD Software GitHub. While this may be of use to a developer with a lot of advanced understanding of C and graphic programming this repository is frankly virtually useless to anyone trying to make a game in the engine these days. These quick comments in a thread will never be able to actually help a new developer create much of anything in the Quake engine and I hope in this guide to get a curious developer on the path to making a game from zero knowledge of the very complicated world of Quake development.

## Why to Make/Not Make a Game in the Quake Engine

Before we actually tackle all of the basics of making a game in the Quake engine you as a developer need to take a step back and consider why you want to use the Quake engine in the first place. This is not a measure of gatekeeping from the engine but rather a way to make sure you are making a well informed decision and don't end up losing large sums of your time and having nothing to show for it.

Also, I do want to say if your goal first and foremost is to publish a retro shooter in the style of Quake with minimal effort **DO NOT** use the Quake engine, go install [Qodot](https://qodotplugin.github.io/) so that you can use Quake maps in Godot and write netcode using [Steam Networking Sockets](https://godotsteam.com/). The Quake engine has a steep learning curve and you will have to invest a lot of time understanding the ins and outs of it before making much of anything original.

Why not to use the Quake engine:
- You like having an editor as your game engine (godot, unity, unreal, etc)
- You don't want to deal with old/esoteric file formats
- You don't want to learn a new programming language
- You don't want to have to work with limited/old documentation

Why to use the Quake engine:
- You want to have a very close relationship between game scripting and engine functionality
- You like the tooling, particularly the mapping workflow
- You like the physics system, movement, and controls of Quake
- You don't want to write any netcode

If all or some of these bullet points don't make complete sense don't worry I will cover more of how the engine works in the following sections and it will become more clear.

## Source Ports

As you may have realized by now the Quake engine is functionally just the executable that you launch the game from. It handles loading the assets, rendering, physics, menu/HUD elements, and other game code facets. The Quake engine is not an editor akin to somethine like Godot or Unity. When the Quake source code was released by iD Software developers took to tearing it all apart and making their own improvements upon the original source code. Most people who make games or mods in the engine today do not use the original executable that shipped with Quake in 1996 but instead improved versions of the engine with different specializations.

### Vanilla Source Ports

The most common type of Quake source port is what I will refer to as a vanilla source port. These ports are primarily focused on aiding in making singleplayer mods for the original Quake game. They aim to be faithful to Quake as it was on release with most changes being rendering/graphical improvements or extended features for loading and managing mods. You should choose to use a vanilla source port if you want to make a mod of Quake with a singleplayer focus or are interested in the [NetQuake](https://quakewiki.org/wiki/NetQuake) multiplayer. 

List of most popular vanilla source ports:
- [Quakespasm](https://github.com/sezero/quakespasm)
- [Quakespasm Spiked](https://github.com/Shpoike/Quakespasm)
- [vkQuake](https://github.com/Novum/vkQuake)
- [Ironwail](https://github.com/andrei-drexler/ironwail)

### QuakeWorld Source Ports

QuakeWorld is the dedicated multiplayer only engine for Quake. The original QuakeWorld engine was released by iD Software in 1997 as a way to improve upon the issues with NetQuake and allow for a better focus on competitive multiplayer elements. If your goal is to make a multiplayer only game in the Quake engine you will more than likely want to be using a QuakeWorld source port. The source ports I will be covering below are actually also outfitted to make singleplayer games and are even compatible with NetQuake servers. Most QuakeWorld only engines are not ideal for making an original game since they very much are featured with playing Quake exclusively in mind.

#### FTEQW

[FTEQW](https://www.fteqw.org/) (ForeThought Engine QuakeWorld) is in my opinion the best source port for making original Quake engine games. FTE has extensive support for many different file formats from the first 3 Quake games and tons of graphical and networking improvements. It also has extended QuakeC language support allowing the user to not only define game behavior but also make custom HUDs and menus without needing to modify the engine, which is a feature that vanilla engines do not support. The FTE engine also has a very active developer community that will be able to help you get started. [Discord Link](https://discord.gg/CFYxYGPYsQ)

#### Darkplaces

The [Darkplaces](https://github.com/DarkPlacesEngine/darkplaces) engine is most well known as being the engine behind Xonotic and now more recently Wrath: Aeon of Ruin. Darkplaces is a very powerful engine focused on maximizing for graphics. It also supports both NetQuake and QuakeWorld multiplayer and extended QuakeC support. Darkplaces however in my opinion is a fair bit more challenging to use and has less of a strong community behind it. I don't know many people using it in 2024 outside of the Wrath team and I would not recommend it as a first time engine unless you feel confident in your skills as a developer.

## QuakeC

At this point you might be wondering what this QuakeC programming I've been mentioning is all about. QuakeC is a scripting language for writing game code in the engine akin to GDScript in godot or using C# in Unity. It uses a psuedo object-oriented style but has a lot of quirks that makes it pretty different from a lot of modern programming languages. You will also need a compiler in order to build your code. The most popular one is [fteqcc](https://www.fteqcc.org/). The tooling for QuakeC in modern text editors is very lacking, there are a couple extensions for vscode but outside of that little has been done. QuakeC is much too complicated of a topic for this crash course unfortunately, but I will save you some time and link you to the best starting points on learning it. [QuakeC Manuals](https://github.com/USDQC/quakec-resources), [QuakeC Tutorials](https://quakewiki.org/wiki/QuakeC_tutorials), [Quake Mapping Discord](https://discord.gg/9Ze66J4ere) (the discord isnt only for mapping but also has channels for getting help with debugging and learning QuakeC)

## Modeling

The Quake engine uses a completely orignal model format known as the mdl file. The mdl model is fortunately pretty simple to work with using modern tools but there are some things to know about it. Most people maked mdl models in blender and use [this plugin](https://github.com/victorfeitosa/quake-hexen2-mdl-export-import) to import and export mdl models. There are some restrictions on how you can make your mdl models however that you need to be aware of. Firstly all animations are baked in, the iD developers used vertex animation back in the 90s. Luckily you wont have to do that because of the way the blender plugin works but after you export a model all skeletal/rigging information is not going to be stored. Make sure you keep your .blend files so that you have rigged versions of your models if skeletal is something you desire to use. The other thing to know about animation is that you store all animations in one sequence. If you arent sure what that means try importing a Quake mdl file into blender and run the animation to see. 

When dealing with model textures there are also some restrictions. Quake has a limited [color palette](https://quakewiki.org/wiki/Quake_palette) and while the blender plugin will approximate to the nearest color on the palette if your texture has colors outside of the palette it is in your best interest to stick to the palette while making textures to achieve the best look. Another thing to know is that Quake models are actually able to have multiple textures but they all must be the same size. The only model that uses this in the base game is armor.mdl to my knowledge.

## Mapping

Quake mapping is probably the easiest part of making a game in Quake and personally one of my favorite parts of working with the engine. There have been many different mapping tools for Quake over the years but in 2024 most people are using the wonderful tool [Trenchbroom](https://trenchbroom.github.io/). This mapper is fantastic and intuitive, I wont be covering how to use it in this guide but go check out dumptruck_ds's guides on [YoutTube](https://www.youtube.com/watch?v=gONePWocbqA&list=PLgDKRPte5Y0AZ_K_PZbWbgBAEt5xf74aE) to learn everything you need to know.

## Texture WADs

Quake stores texture files in a format known as WAD files (stands for where's all the data and I still find that funny). I'm also not going to go into making texture WADs because dumptruck_ds has done a great job on this topic as well. Linked [here](https://www.youtube.com/watch?v=gONePWocbqA&list=PLgDKRPte5Y0AZ_K_PZbWbgBAEt5xf74aE). The other thing to note is if you are using a vanilla Quake engine the HUD elements are in a file known as gfx.wad and you'll need to modify or make your own gfx.wad to change HUD visuals.

## Lmp Files

This is really only an issue if you are using a vanilla source port, but all of the menu visuals are stored in a binary image format known as lmp (lump) files. This page on the [Quake wiki](https://quakewiki.org/wiki/.lmp) has most of what you need to get started making lmp files. I honestly just don't think it's worth working with them as a first time developer and you would be better off utilizing the fact that FTE and Darkplaces are compatible with more modern image formats like tga and png when making UI elements.

## Shameless Plug and Conclusion

If this seems like a lot to take in all at once don't worry. The Quake engine is complicated at first but it gets managable over time. Focus on making small minigames or tweaks to the original Quake experience before diving headfirst into a new game. Reach out for help in the discord servers linked above and don't feel discouraged when things dont work, because they will break a lot at first.

Due to how complicated this learning curve is and frankly how painful some of the old tools can be to use as an experienced developer I personally have been working on an editor for the Quake engine to hopefully solve some of these issues. It's a semi all-in-one tool that helps with the modification of the binary file formats of Quake and writing QuakeC code. Check it out [here](https://github.com/BanceDev/QuakePrism). It has a built in compiler and project templates to help get you started making mods and games out of the box. 
