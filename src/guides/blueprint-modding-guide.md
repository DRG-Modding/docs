# Blueprint Modding Guide

<h2><strong>DRG Mods: A Comprehensive Guide to Blueprint Modding</strong></h2>
<blockquote>
<p>Please do not hesitate to ask for help! You can find talented modders in the <a href="https://discord.gg/tmFJcesA6d">DRG RND Discord</a> or the <a href="https://discord.gg/nkPhp2sZfd">DRG Modder's Guild Discord</a>.</p>
<p><strong>Credits:</strong><br />Buckminsterfullerene - Originally wrote and maintains guide.<br /><a href="https://mod.io/u/ourlordandsaviorgabe">OurLordAndSaviourGabe</a> (AKA Banagement) - Advanced replication explanation section.<br /><a href="https://mod.io/u/memechael">MichaelG123</a> - Secondary advanced replication section.</p>
</blockquote>
<hr />
<h2>Contents</h2>
<ul>
<li>1. Introduction
<ul>
<li>Tools</li>
<li>Reading this guide</li>
<li>Final note</li>
</ul>
</li>
<li>2. How blueprints work
<ul>
<li>How BP mods are loaded into DRG</li>
<li>Framework mods</li>
<li>Methods of BP modding</li>
<li>Useful acryonyms to know for BP modding</li>
</ul>
</li>
<li>3. Setting up your UEE workspace</li>
<li>4. No-dummy method
<ul>
<li>Creating your first DRG mod</li>
<li>A bit more complex version</li>
<li>Outputting time dilation to the HUD</li>
<li>Toggle HUD button</li>
<li>Saving and loading settings</li>
<li>Basic replication</li>
<li>Advanced replication</li>
</ul>
</li>
<li>5. Dummy method
<ul>
<li>Kill player on button press</li>
<li>Self-destruct Bosco on button press</li>
</ul>
</li>
<li>6. Packaging your mod
<ul>
<li>Using DRG Modding Automation Scripts</li>
<li>From UE</li>
<li>Directories to never cook</li>
</ul>
</li>
<li>More Help</li>
<li>Video Tutorial</li>
<li>Feedback</li>
</ul>
<hr />
<h2>1. Introduction</h2>
<h3>Tools</h3>
<p>Before you can even get started with your mod, you need to install a few tools:</p>
<ul>
<li>DRGPacker</li>
<li><a href="https://www.unrealengine.com/en-US/download" target="_blank" rel="noopener noreferrer">Unreal Engine</a> <code>4.27.2</code>(it will look like just 4.27.0 on the epic games launcher, which is good to download)</li>
<li><a href="https://visualstudio.microsoft.com/vs/older-downloads/" target="_blank" rel="noopener noreferrer">Visual Studio 2019</a> or <a href="https://visualstudio.microsoft.com/vs/whatsnew/" target="_blank" rel="noopener noreferrer">2022</a>, with "Desktop Development with C++" and "Game Development with C++" packages checked in the Visual Studio Installer</li>
<li>The most up-to-date <a href="https://github.com/DRG-Modding/Header-Dumps" target="_blank" rel="noopener noreferrer">game dumps</a> (if you are using the BP dummy method explained later)</li>
<li>The <a href="https://github.com/DRG-Modding/FSD-Template" target="_blank" rel="noopener noreferrer">FSD template project</a> (if you are using the C++ dummy method explained later)</li>
</ul>
<blockquote><strong>IMPORTANT: By the time you are reading this guide, you should already have these tools and have at least the minimum required knowledge to use them (more on UE4 later). If not, I refer you to Rauliken's more general <a href="https://mod.io/g/drg/r/drg-basic-modding-guide">guide</a>.</strong></blockquote>
<h3>Reading this guide</h3>
<p><strong>Make sure that you read through every detail of this guide thoroughly as missing something may result in many problems down the line. </strong>Of course, you can always refer back to this if you need assistance on anything. Critically important details are highlighted in <strong>bold</strong>, and optional but useful information is highlighted in <em>italics</em>. <code>Inline code</code> is used for 'specifics'.</p>
<p>You should know the basics of Blueprinting in Unreal Engine. There are plenty of tutorials on YouTube. You should also know how Blueprinting associates with C++; <a href="https://www.youtube.com/watch?v=VMZftEVDuCE">here is a really good video</a> on that. <strong>If you haven't already, watching this video is basically a requirement to understand much of what is going on here.</strong></p>
<p>If you are into other forms of UE modding, please don't hesitate to join the <a href="https://discord.gg/zVvsE9mEEa">UE Modding </a>discord server.</p>
<h3>Final note</h3>
<p>Please be aware that as BP modding becomes increasingly advanced, this guide may become out of date until I update it. Since I'm really busy all the time this may not happen for a few days or weeks.</p>
<hr />
<h2>2. How blueprints work</h2>
<h3>How BP mods are loaded into DRG</h3>
<p>Native spawning is something that was added by the developers when the modding update dropped. This allows you to load your blueprint from the <code>BeginPlay</code> event node. If you want your BP mod to have user-interactable UI (like a settings menu), you should use a framework like <code>ModHub</code>.</p>
<h3>Framework mods</h3>
<p>Usually, you will only want to use a framework mod if you want the user to be able to change settings for your mod in game. If you don't need this for your mod, you do not need to use a framework. Framework mods so far have always come in two parts:</p>
<ul>
<li>The devkit tools that you put into your mod's UE project for interfacing with your mod</li>
<li>The in-game mod dependency that runs all of the framework's functions and processes</li>
</ul>
<p>I suggest that for now, as you are learning BP modding, that you don't touch any frameworks, otherwise you'll find yourself spending more time wrangling with widgets than actually learning new techniques.</p>
<p><strong>ModHub</strong></p>
<p>ModHub is a shared-settings mod interface. You can use this to add your mod settings to a central place.</p>
<p><strong>DRGLib</strong></p>
<p>Samamster has been developing this framework as a more feature-rich BP modding library. The great thing about this library, is that it provides helper functions and DRG-like UI objects that makes BP modding just that little bit easier. You can view both the source and guide for use <a href="https://github.com/SamsDRGMods/DRGLib">here</a>.</p>
<h3>Methods of BP modding</h3>
<p>There are two methods, in a way that you can use both at the same time or not if you wish:</p>
<ul>
<li>No-dummy method. This doesn't require any knowledge in C++ to use. You also won't need the game dumps. This is limited to built-in UE BP functions and events or those you have from a framework devkit, or you created yourself. You can still achieve a fair bit from this but are limited.</li>
<li>Dummy method. You can manipulate the functions, variables and events that are running in the game. You can figure out what you need from looking at the game's dumps files. You can find the most up-to-date dumps versions <a href="https://github.com/DRG-Modding/Header-Dumps">here</a> (be aware that GitHub only displays the first thousand classes on its website version).</li>
</ul>
<h3>Useful acronyms to know for BP modding</h3>
<ul>
<li>ABP &ndash; Animated BluePrint</li>
<li>BP &ndash; BluePrint</li>
<li>GD &ndash; Game Data</li>
<li>GM &ndash; GaMe</li>
<li>ID &ndash; IDentifier</li>
<li>ITM &ndash; ITeM</li>
<li>LIB &ndash; LIBrary</li>
<li>LVL &ndash; LeVeL</li>
<li>MUT &ndash; MUTators</li>
<li>OC &ndash; OverClocks</li>
<li>PRJ &ndash; PRoJectile</li>
<li>UI &ndash; User Interface</li>
<li>UPC &ndash; UPgrade Category</li>
<li>UPG &ndash; UPgrade Group</li>
<li>W &ndash; Widget</li>
<li>WND &ndash; WiNdow Widget</li>
<li>WPN - WeaPoN</li>
</ul>
<hr />
<h2>3. Setting up your UEE workspace</h2>
<p><strong>For ANY Blueprint mods, you MUST name your project <code>FSD</code>.</strong> Therefore, I make all my mods inside the same project and then delete the mods I don't want to pak before I pak them. <em>The reason the name must be <code>FSD</code>, is because that is what the original game's UE project is called. "FSD" is probably the code-word for DRG (most games have these for various reasons).</em></p>
<p>First, select the games category, then click next:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_5900585141581493.png" alt="" /><img style="font-size: 1em;" src="https://image.modcdn.io/members/6da1/6436408/profilebp/image001.png" alt="image001" width="431" height="185" /></p>
<p>Then click on blank and click next:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image002.png" alt="image002" width="436" height="171" /></p>
<p>Select the following options on the project settings:</p>
<p><img style="font-size: 1em;" src="https://image.modcdn.io/members/6da1/6436408/profilebp/image003.png" alt="image003" width="435" height="248" /></p>
<p>Then hit create project, and you're ready to move onto the next part!</p>
<hr />
<h2>4. No-dummy method</h2>
<p>I'm going to run through the creation of a super simple mod which then outputs text to the screen, and finally saves and loads mod data. <em>Note that my UE might look different to yours &ndash; don't worry, I'm just using a <a href="https://www.unrealengine.com/marketplace/en-US/product/electronic-nodes">couple</a> of <a href="https://www.unrealengine.com/marketplace/en-US/product/darker-nodes">plugins</a> that makes it looks nicer.</em></p>
<h3>Creating your first DRG mod</h3>
<p>This mod will simply set the global time dilation (the speed) of the game to <code>5</code>. The purpose of this mod is to just run through the process of making a mod &ndash; the time dilation bit will validate that we know the mod is loaded.</p>
<p>To spawn a blueprint into the game, first you need to create a folder with the same name as your mod inside the <code>Content</code> folder. Then create a blueprint inside of that called one of two things:</p>
<ul>
<li>
<p><code>InitSpacerig</code> &ndash; this will load your blueprint when the player is spawned in the spacerig.</p>
</li>
<li>
<p><code>InitCave</code> &ndash; this will load your blueprint when the player is spawned in the drop pod at the start of a mission.</p>
</li>
</ul>
<p><span style="font-size: 1em; font-weight: 400;"> To make a blueprint, right click inside the content browser and hit <code>Create Blueprint Class</code>. Then inherit it from <code>Actor</code>.</span></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_308ca601fd4f7226.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image004.png" alt="image004" width="258" height="208" /></p>
<p>Double click on <code>InitSpacerig</code> first, and navigate to the <code>Event Graph</code> tab. We can ignore <code>Event ActorBeginOverlap</code> and <code>EventTick</code>. Drag off <code>Event BeginPlay</code> and find the set global time dilation node:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_7f8cad1390eebb83.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image005.png" alt="image005" width="480" height="129" /></p>
<p>Now set this to <code>5</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_7e8b57fa9c7fa0b8.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image006.png" alt="image006" width="252" height="126" /></p>
<p>To save, hit the <code>Compile</code> button on the toolbar. <strong>Remember to compile and save your project every now and again</strong>. <em>Tip: you can compile and save at the same time when you click compile, by clicking the little arrow dropdown to the right of compile button and setting save to "always":</em></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_b7bf638fc3bd2fef.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image007.png" alt="image007" width="410" height="168" /></p>
<p>Now we are ready to package and test this simple mod.</p>
<p><strong>Refer to section 6 on <a href="#h_3087274881571653840477152">packaging your mod</a>.</strong></p>
<p>When you load into the game, everything should be moving 5x as fast! More importantly, now you know how to make blueprint mods!</p>
<h3>A bit more complex version</h3>
<p>This time around, the mod will change the time dilation based on a key press.</p>
<p>First though, let's streamline the same mod to be loaded by both <code>InitCave</code> and <code>InitSpacerig</code>, since we don't want to copy and paste everything into both BPs.</p>
<p>Make a new BP called something like <code>Mod</code> (it doesn't matter). Now in <code>InitSpacerig</code> and <code>InitCave</code>, we want to spawn this actor immediately. Get out a <code>Spawn Actor From Class</code> node, and set the class to whatever you called your mod BP. Do this in both <code>InitSpacerig</code> and <code>InitCave</code>:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_2d2d5c9bc17dd00e.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image008.png" alt="image008" width="410" height="170" /></p>
<p>We're not quite done yet, we need to set <code>Collision Handling Override</code> to <code>always spawn, ignore collisions</code>. Also, right click on that orange <code>Spawn Transform</code> pin and hit <code>split struct pin</code>. If you don't do this, it won't know where to spawn the actor. <em>Later on, if you're spawning a physical actor but don't want it to be seen, spawn it at like Z=99999.</em></p>
<p><strong>Make sure to compile and save when you're done in both BPs!</strong></p>
<p>Now inside of your new mod BP, we want to add the functionality that increments or decrements the global time dilation based on what key you press. Let's say that the hyphen key decrements it by <code>0.5</code> and the equals key increments it by <code>0.5</code>.</p>
<p>First, let's make a variable that stores the time dilation. Set its default value to <code>1</code>. Now we need to get the key events. Right click on the graph and type <code>Hyphen</code> and get out that node, and then do the same for <code>Equals</code>. <strong>Make sure that you enable input when the mod is spawned, otherwise this won't work:</strong></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_e6ff2f77bb73c62a.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image009.png" alt="image009" width="501" height="158" /></p>
<p><em>Player index 0 is always host, i.e. your player if you are the only one in the server or hosting a server.</em></p>
<p>Now make your mod functionality like this (you should have done basic blueprinting before as a pre-requisite to this so you should be able to easily follow what this does):</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_78c7136f6403266c.png" alt="" /><img style="font-size: 1em;" src="https://image.modcdn.io/members/6da1/6436408/profilebp/image010.png" alt="image010" width="500" height="165" /></p>
<p>Now let's make sure that the dilation never goes below 0 (otherwise the game crashes) or higher than <code>8</code> (otherwise FPS is reduced the rubble):</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_24ff5fde0eaaa1b3.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image011.png" alt="image011" width="506" height="176" /></p>
<p>Before we go any further, let's compile, package and test the mod.</p>
<h3>Outputting time dilation to the HUD</h3>
<p>Now we want to output the current dilation to the HUD (Heads Up Display) so we can see the value in-game.</p>
<p>In our <code>Tutorial</code> folder, make a new widget blueprint called something like <code>HUD</code>:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_d40666242ac1acb9.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image012.png" alt="image012" width="246" height="129" /></p>
<p>Inside of the widget designer, find the text box widget in the palette and drag it out. Place it anywhere on the window &ndash; I recommend putting it about <code>2/3</code> the way up the left or right side of the window, which is where we know there is empty space on the DRG HUD. This text will just be some text like "Time Dilation:". Next to it, put another text box with the text "1". This is where we will change the value. The default dilation in the game is <code>1</code> so we set the default text to match that.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_ca22594c9dda0df2.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image013.png" alt="image013" width="379" height="221" /></p>
<p>Now name that second text box to something sensible. <strong>You also need to check the <code>Is Variable</code> box, otherwise we won't be able to change it from our mod BP.</strong></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_420d75ccfa813451.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image014.png" alt="image014" width="389" height="40" /></p>
<p>Hit Compile and switch back to the mod BP. To access and set our text box, we need to first construct the widget and get an object reference from it:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_39a72a97de8a92d0.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image015.png" alt="image015" width="554" height="80" /></p>
<p>This <code>HUD Ref</code> variable is our object reference. The variable type is that <code>HUD</code> widget:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_9f1b3da9752d367a.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image016.png" alt="image016" width="552" height="26" /></p>
<p>Now drag out the <code>HUD Ref</code> variable and we want to get the object reference to the <code>txtDilation</code>:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_3b7e406e4d654754.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image017.png" alt="image017" width="286" height="151" /></p>
<p>Now to set its text, we drag out the <code>Set Text (Text)</code> node:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_5c4bbf0726f3ea9d.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image018.png" alt="image018" width="286" height="191" /></p>
<p>Now we want to set the text to the value of time dilation:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_3ecaaa95106ecfac.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image019.png" alt="image019" width="602" height="214" /></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image145.png" alt="image145" width="385" height="224" /></p>
<p>Now we can see our time dilation and that it changes when we hit our key binds. But what if we want to toggle the HUD on or off?</p>
<h3>Toggle HUD button</h3>
<p>First, we need to go back into our HUD designer and set that description text box to a variable so we can control it from our mod:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_6652451f64231cf4.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image021.png" alt="image021" width="384" height="36" /></p>
<p>Now back in our mod, make a variable that will store the boolean value of whether or not the HUD is enabled. Make sure the default value is <code>true</code>.</p>
<p>When a key, say, '#', is pressed, we want to switch the visibily of the text boxes between <code>Visible</code> and <code>Collapsed</code> (you can read the tooltip regarding the differences between <code>Collapsed</code> and <code>Hidden</code>):</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_6bfaf0137ef55f04.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image022.png" alt="image022" width="602" height="178" /></p>
<p>Let's check to see if this works!</p>
<p>Now&hellip; what if we want to save the dilation and if the HUD is enabled so that the next time the mod is loaded we save our settings?</p>
<h3>Saving and loading settings</h3>
<p>We first need to make a new blueprint that inherits the <code>SaveGame</code> class:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_fb775ef195d00df.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image023.png" alt="image023" width="429" height="344" /></p>
<p>Inside of it, simply make the two variables that we want to save, and make sure that they are set to public:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_7d1085e59f00d81c.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image024.png" alt="image024" width="405" height="86" /></p>
<p>Back in our mod, make a variable that is an object reference to this <code>SaveData</code> blueprint:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_7955e05ef9d891ca.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image025.png" alt="image025" width="535" height="33" /></p>
<p>Now let's make a function that will load the save. This checks if the save game exists, and if it does, load the game from the slot. If it does not, we create an empty save game object. The save file name is <code>Mods/[your mod name]/[your save name]</code>. The saves are stored at <code>FSD/Saved/SaveGames</code>. Here my save name is <code>Mods/Tutorial/Settings</code>:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_4472d23c260305b1.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image026.png" alt="image026" width="602" height="156" /></p>
<p>Call this function after we construct our widget (I've added a sequence node to tidy up a bit).</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_23151b5bbd382c76.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image027.png" alt="image027" width="602" height="269" /></p>
<p>Now we want to make another function that loads the save data into our mod variables. We also want it to change the HUD values, i.e. if the HUD is disabled in the save, we want to toggle the HUD off when we load it in:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_2ce4a3d39d42adf5.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image028.png" alt="image028" width="602" height="94" /></p>
<p>Chuck it after load save:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_e3f6ad663533bf51.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image029.png" alt="image029" width="355" height="109" /></p>
<p>The <code>Toggle HUD</code> and <code>Set Dilation Text</code> functions are just what we have already done but put into functions so that we don't have duplicated code.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_637eb01da6721c03.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image030.png" alt="image030" width="476" height="214" /></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_17e6fc395ecc3b9d.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image031.png" alt="image031" width="476" height="158" /></p>
<p>The last thing we want to do is save our values every time we change them. So let's make a new function that does this:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_c3d4dd448aaacbad.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image032.png" alt="image032" width="601" height="140" /></p>
<p>Now we call this function when we change time dilation and the HUD value. Now the mod looks like this (I've tidied it up with comments):</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_b7445234417e5999.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image033.png" alt="image033" width="601" height="554" /></p>
<p>Now, compile, package and test. If you toggle your HUD off, and set dilation to like <code>5</code>, then restart the game, your settings should be saved!</p>
<p>But we want this mod to work in multiplayer, so we need to do a bit of basic replication and handling with that.</p>
<h3>Basic replication</h3>
<p>Before you read this section, I suggest that you go watch some basic UE4 replication video on YouTube as it will explain the basic processes that I will use here. If you're not interested in replication (it's hard), don't worry, you can skip this section without any problems.</p>
<p>While the mod works perfectly fine in singleplayer, it goes all a bit funky in multiplayer. If you're the host, you need to replicate (copy) the time dilation that you're setting for the rest of the clients in the server. Otherwise, anything client side only (like player movement) will not be affected. And if you're not the host, then you shouldn't be able to change the time dilation because otherwise things get even more funky if yours is different to the server's.</p>
<p>So firstly, let's just get the easy bits out of the way. If a client is not the server, disable the input so that they can't change their local time dilation value. We can do this by simply using the <code>Is Server</code> node:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image146.png" alt="image146" width="713" height="186" /></p>
<p>We also need to prevent the time dilation from being set when the save is loaded. While we could do the same <code>Is Server</code> check before the entire load save section, we want to still allow all the clients to load the <code>Is HUD Enabled</code> value from their own saves. So we can just do the check inside of <code>Load Save Data</code> instead.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image149.png" alt="image149" width="986" height="142" /></p>
<p>Now for the fun stuff. First, we need to change the mod settings to allow replication. Click <code>Class Defaults</code> on the top bar and change the replication settings like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image150.png" alt="image150" width="257" height="244" /></p>
<p>You also need to go into the class defaults for <code>InitCave</code> and <code>InitSpacerig</code> and change the replication settings like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image151.png" alt="image151" width="255" height="245" /></p>
<p>Next, we need to actually set the <code>Time Dilation</code> variable to replicate. You can do this by clicking on the variable and setting the <code>Replication</code> parameter to <code>RepNotify</code>. The reason we want to use this, instead of just <code>Replicated</code>, is that we want to notify the clients when the value of it changes.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image147.png" alt="image147" width="330" height="449" /></p>
<p>You may notice that in the functions pane, a new function called <code>OnRep_</code> e.g. <code>OnRep_Time Dilation</code>, has been created. This function is called by every client whenever the value is changed, including the host. You may also notice that 2 white spheres have appeared on all the variables' getters and setters - this shows that the variable is set to <code>RepNotify</code> (one sphere is <code>Replicated</code>). You can also hover over the setters and it'll tell you how it works.</p>
<p>So inside this new <code>OnRep</code> function, when we then set the global time dilation and HUD value, we need to make sure that it only runs for non-host clients (AKA remotes). To achieve this, we can use the <code>Switch Has Authority</code> node and pull off execution only when remote. Then we set the global time dilation and the HUD text:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image148.png" alt="image148" width="949" height="187" /></p>
<p>Then you're done! You don't need to call the <code>OnRep</code> function - this is done automatically everytime the variable is set.</p>
<p>Now to test, you will need a friend (oh no, this isn't looking good for you) to check that all the logic works as expected. To achieve this, you will need to make a new <strong>hidden</strong> mod on mod.io and use the preview link or add your friend as a moderator so that they can subscribe to it.</p>
<h3>Advanced replication</h3>
<p>For this section, you will need to<strong> download and build the <a href="https://github.com/DRG-Modding/FSD-Template">FSD Template</a> project </strong>(explained at the start of the 5th section)<strong>.</strong></p>
<p>"Advanced" replication here is defined as anything more than replicating just variables. This is really, really hard to wrap your head around, because due to the nature of this being modding, 90% of normal replication tutorials aren't going to help you. <strong>I highly suggest that you attempt to make at least one mod of your own first before attempting this.</strong></p>
<p>There are two types of advanced replication you need to know about:</p>
<ul>
<li>Approved replication. This is a mod where we can be certain the code will run on the server, and the clients have the events sent to them due to the <code>RequiredByAll</code> tag that you can give your mod to force everyone else in the server to have the mod.</li>
<li>Verified replication. This is where the mod only runs on the client, but still is able send and recieve events to and from other clients in the lobby. This is best used for mods that say, send chat messages to everyone in the server.</li>
</ul>
<p>Firstly though, Banagement (AKA Our Lord And Saviour Gabe Newell) explains the main ideas of advanced replication using all of his vast experience (and amazingness).</p>
<p>A thorough explanation of advanced replication</p>
<p>Banagement here, I'll be taking over for a bit.<br />For an actor to replicate you will need to change a few class default variables on the new actor, these are - <code>Always Relevant</code>, <code>Replicates</code>.<br />If this was a pawn (character / enemy), some sort of actor with a model, or whatever may affect the world space and it is planned to move at any point then you would also enable Replicate Movement.<br /><code>Actor Components</code> do NOT need to be replicated if the owning actor is replicated, this applies for <code>Child Actors</code> too.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/infopasserclasssettings.png" alt="infopasserclasssettings" width="359" height="495" /></p>
<p>With the above setup this actor will now replicate to clients when spawned by the server / network authority which means that only one has to exist instead of one per player.<br />When this actor is spawned by the server it will behave just like the host variant for clients so no de-sync should occur for most actions (this is explained in the "Info Passer Logic" section).<br />Later on I explain how you can also use this method to create a client spawned replicated actor so that the host / server can "see" the clients version of the actor and update their data accordingly.<br /><br />You pretty much only want to use this client spawned method to pass data and not spawn anything that affects the world space.<br /><br />If you are doing something more than just altering visuals for a verified mod such as "Fancy Steeve" then this can and will cause issues if it is spawned by the client.<br />For example if the client spawned actor were a model, pawn, etcetera it will create new "ghost" instance(s) that exist on the client but not on the host so data can be mismatched such as it being unable to be killed, messing with collision for the client and some logic called by a client owned actor can crash clients.</p>
<p><strong>InitCave / InitSpacerig Logic</strong></p>
<p>So let's take a look at<code>InitCave</code> or <code>InitSpacerig</code> actors, the same logic should apply for both. Ideally you would use <code>InitCave </code>or <code>InitSpacerig</code> to only spawn your own actors to perform your mod logic.<br /><br />Before anything, I highly recommend <strong>NOT</strong> doing is using <code>Child Actor Components</code> on <code>InitCave</code> or <code>InitSpacerig</code> as there is a nasty problem that causes multiple instances of those child actors, usually exponential for clients. This can be fixed by spawning an actor via <code>InitCave</code> or <code>InitSpacerig</code> that spawns its own child actors.</p>
<p>Let's say you want to spawn an actor of some sort; you will usually want this to be a server only action so there are no "ghost" instances (described in previous section).<br />To achieve this it is as simple as using 2 nodes, <code>Is Server</code> pure function and <code>Branch</code> (you can press B and left click to quickly place in an event graph), just plug these two together to make a boolean check using the True or False execution pins. You will want to put the <code>Spawn Actor From Class</code> node on the True output, this essentially says "If Server / Host / Network Authority, then spawn this actor" so it is not possible for clients to do so. Supposedly using the <code>Has Authority</code> macro is what is used for actual game development but it can be confusing to utilize without understanding it well.</p>
<p>I bring the following up because it comes into play when setting up actor spawning. You do not want to use object references from something that has not been spawned yet as it will be a null pointer and will either crash the game or break logic in an actor that is trying to reference a specific class for criteria.<br /><br />Unreal Engine 4 is silly and tells you that <code>Sequence</code> nodes are executed in an order, 1-2-3 as the logic completes. This is false, everything assigned to a sequence node is executed at the same time.<br />This can be circumvented with <code>Delay</code> nodes,<code>Async</code> nodes, <code>Set Timer By Event</code> nodes, delayed loops ( <code>Sequence + Delay</code> ), etcetera.</p>
<p>What I would recommend is using <code>InitCave</code> or <code>InitSpacerig</code> as a means to spawn your own logic actors rather than putting the entirety of the logic within it because it can cause a lot of issues.<br />You want to be able to differentiate your mod from others so you should name these spawned actors appropriately in relation for your mod.<br /><br />This is an example from my Mission Content Randomizer mod:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/init.png" alt="init" width="692" height="374" /></p>
<p><em>You do not need to assign ownership at all from my understanding, I just did it as a precaution since I ran into so many issues for replication before figuring it out. </em></p>
<p>In this example I have the <code>MOD_MissionRandomizer_InfoPasser</code> spawned either on host or on client, I kept them separate so it would assign different owners depending but really you should be able to do it before the server check branch node since the client and server both need it.</p>
<p>Replicated variables used for passing info are limited to "core" variables to some extent, so - <code>bool</code>, <code>int</code>, <code>int64</code>, <code>float</code>, <code>string</code>, etcetera.<br />When it comes to replicated actors everything should replicate if the actor is set to, but passing info from one system to another seems limited from what I recall (could be wrong, have not tested).</p>
<p><strong>Info Passer Logic</strong></p>
<p>What we first want to do is make a brand new, empty actor unrelated to anything vanilla and make it replicate via enabling <code>Always Relevant</code>, <code>Replicates</code>.<br />This custom actor will serve as our "info passer" and everything game data related that needs to be passed will be done through this.<br />So now that we have an <code>Info Passer</code> actor that has replicated and always relevant enabled on it, make it spawned on the server and on the client; the host can now receive it on the network from the client and allow the server to alter the clients actor. <br /><br />Here is an example of how I use the <code>Info Passer</code> for Mission Content Randomizer:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/beginplayinfopasser.png" alt="beginplayinfopasser" width="697" height="260" /></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/settersinfopasser.png" alt="settersinfopasser" width="696" height="326" /></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/multicastsetters.png" alt="multicastsetters" width="695" height="284" /></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/multicastsetters2.png" alt="multicastsetters2" width="696" height="278" /></p>
<p>So, I designed this in a way in which the client spawned <code>Info Passer</code> does not do anything on its own as it always fails the server check so it never assigns the server variable info with data from the client.<br />However, the host spawned version does because each of those "server" variables are replicated so all instances of that <code>Info Passer</code> actor will sync their data when told to do so via this <code>Multicast</code> event.<br />Variables that are replicated have two white balls when viewed as a reference in the event graph.</p>
<p>Notice that all of the "client" variables are not replicated:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/clientvarsrep.png" alt="clientvarsrep" width="695" height="373" /></p>
<p>The "server" variables are replicated. There is no need for an <code>Is Server</code> node like in the screenshot below because this data comes from the host and is applied to the host and client.<br />I just use it for less data changing for the host.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/servervarsrep.png" alt="servervarsrep" width="696" height="310" /></p>
<p><strong>This part is important!</strong> - Events can be assigned <code>Multicast</code>, <code>Run On Server</code>, <code>Run On Owning Client</code> and then <code>Reliable</code> or not:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/multicastevent.png" alt="multicastevent" width="694" height="357" /></p>
<p>A breakdown of the settings:</p>
<ul>
<li><code>Multicast</code> is designed for running an event on the server and client(s).</li>
<li><code>Run On Server</code> is designed for a server only event such as spawning an actor via event.</li>
<li><code>Run On Owning Client</code> is designed for client only logic such as particles, animations, sounds.<br />This will still act as a server event if the owning client is the server though, so I would recommend using it exclusively in a client variant of a logic actor or a simple <code>Is Authority</code> check and running off of false so the server will never do it.</li>
<li><code>Reliable</code> is an interesting topic for game development (not really applicable for modding), it is essentially like a "is this event so important that it should ALWAYS be passed on the network?", the more things that have this the more stuff that is being sent on the network so it can cause latency issues and such.<br />A good example of this is damaging players - you don't want clients to be able to skip damage instances while the network load is heavy, but a particle from an environmental piece somewhere? Nah, not important.</li>
</ul>
<p>Generally speaking for modding you are always good to set it to true for reliability. <strong>BUT! </strong> If you got into a really complex mod such as my Mission Content Randomizer where you need to sync a lot of data you will encounter a problem with events, they have a limit of how many inputs and outputs (or maybe that was only for delegates).<br /><br />Either way, a neat thing you can do is create a <code>Structure</code> in UE and assign variables with specific names and types of variables that you want to pass to clients.<br />Then you use that <code>Structure</code> for the input and output of the event. This will allow you to pass ONE variable that acts like a box, you open that box and take the contents out (different variables):</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/struct.png" alt="struct" /></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/structview.png" alt="structview" width="626" height="548" /></p>
<p>I get all of my replicated "server" variables and turn them into a <code>Client Data Structure</code> variable and pass it via the event call.<br />The <code>Structure</code> is then split on that event to set data for individual data assets on the client as I cannot pass it all at once into a single node:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/structset.png" alt="structset" width="624" height="418" /></p>
<p>Now that we have general explanations out of the way, let's try with a basic example that MichaelG123 runs you through.</p>
<p>Approved replication (written by MichaelG123)</p>
<p>There are 4 main actors that we need:</p>
<ul>
<li><code>InitCave</code></li>
<li><code>InitSpacerig</code></li>
<li><code>MasterActor</code></li>
<li><code>ReplicatedActor</code></li>
</ul>
<p>I'll be referring to them by these names, but in your case you might want to change them to something that makes more sense for your mod.</p>
<p>You can think of <code>MasterActor</code> as the actor that you'll be using to interact with the server, and it handles spawning for <code>ReplicatedActor</code>. It's used for server-based things like changing terrain, spawning bugs, etc.<code>ReplicatedActor</code> is where you handle any clientside stuff such as going into 3rd person mode, spawning widgets, or preparing variables to send to <code>MasterActor</code>, and is usually where most of your code will end up.</p>
<p><strong>Setup</strong></p>
<p>Setting up <code>InitCave</code> and <code>InitSpacerig</code> are simple, just make them spawn <code>MasterActor</code>:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image152.png" alt="image152" width="591" height="260" /></p>
<p><code>ReplicatedActor</code> is simple to set up. First, you need to click <code>Class Defaults</code> near the top of the screen, and change the replication settings to this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image154.png" alt="image154" width="269" height="355" /></p>
<p>Still in <code>ReplicatedActor</code>, create a new variable and set its type to your <code>MasterActor</code>. Also make sure to enable <code>Instance Editable</code> and <code>Expose on Spawn</code>:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image153.png" alt="image153" width="406" height="305" /></p>
<p>Now we need to setup <code>MasterActor</code>. First, create a function called something like <span style="font-weight: 400;"><code>Spawn Replicated Actor Per Player</code> and add a new input of type <code>FSDPlayer Controller</code> to it. Then create a <code>Spawn Actor from Class</code> node and have it spawn <code>ReplicatedActor</code> like this:</span></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image157.png" alt="image157" width="608" height="265" /></p>
<p>Create a function called <span style="font-weight: 400;"><code>Subscribe to Player Join</code>. </span><span style="font-weight: 400;">Add a new input of type <code>FSDGame Mode</code> to <code>Subscribe to Player Join</code>. </span><span style="font-weight: 400;">Inside the function, drag off of the <code>FSDGameMode</code> pin and create a <code>Bind event to On Player Logged In</code>. </span><span style="font-weight: 400;">Drag off of the event pin in that function and select <code>Create Event</code>. Then s</span><span style="font-weight: 400;">elect <code>Spawn Replicated Actor Per Player</code> from the list so that the function looks like this:</span></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image155.1.png" alt="image155 1" width="602" height="270" /></p>
<p>Now we need to run our functions, if the actor is the server:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image156.png" alt="image156" width="914" height="227" /></p>
<p><strong>Creating the mod</strong></p>
<p><span style="font-weight: 400;">For everything you see here, I&rsquo;ll be using examples from a couple of my mods - you can get the source code for a poke around <a href="https://github.com/michaelgoldenn/EmotesMod">here</a>.</span></p>
<p><span style="font-weight: 400;">For any event that you&rsquo;d like to have the client run, just put everything inside the logic inside of the <code>ReplicatedActor</code>. </span><span style="font-weight: 400;">For example, if you want to play music for only for the client, I have this set up:</span></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image158.png" alt="image158" width="915" height="388" /></p>
<p><span style="font-weight: 400;">You can just call this from wherever you want, nothing special going on here. </span><span style="font-weight: 400;">The different part comes when you&rsquo;d want the server to do something (spawn things, destroy terrain, apply status effects, basically anything that everyone sees happen at the same time). </span><span style="font-weight: 400;">Here, we need to make "twin" events, one in <code>MasterActor</code> and one in <code>ReplicatedActor</code>. This is what I have in <code>MasterActor</code>, because it&rsquo;s up to the server to tell everyone to spawn the sound. Make sure you make pins for everything that you&rsquo;ll need. <strong>Your <code>MasterActor</code> events should all be set to <code>Multicast</code>.</strong></span></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image159.png" alt="image159" width="922" height="230" /></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image160.png" alt="image160" width="302" height="190" /></p>
<p><span style="font-weight: 400;">MasterActor events are called by their &ldquo;twin&rdquo; event in <code>ReplicatedActor</code>. Create a new event in <code>ReplicatedActor</code> and <strong>set its replication setting to <code>Run on Server</code></strong>, create pins that match the pins in the <code>MasterActor</code> event, and just plug all of the pins from one into pins of the other.</span></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image161.png" alt="image161" /></p>
<p><span style="font-weight: 400;">And that should be everything. You can repeat this event process as many times as you&rsquo;d like and everything should work out.</span></p>
<hr />
<h2>5. Dummy method</h2>
<p>I'm going to run through making a couple of mods which utilise some of the C++ and dummy methods. Once you know how the basics work, you are then set to much more easily be able to experiment, test, and implement any of your own features that use any of the game's functions and BPs for your mods. The possibilities here are endless. Overview of the two example mods I will step-through in this tutorial:</p>
<ol>
<li>Kill player on button press.</li>
<li>Self-destruct Bosco from a button press.</li>
</ol>
<p>Both examples will still feature info on how to look at the C++ header dumps.</p>
<p><strong>The very first thing you need to do is download and build the <a href="https://github.com/DRG-Modding/FSD-Template">FSD Template</a> project. This has all of the C++ classes automatically reflected in the project source.</strong> This saves modders a LOT of time, as there are thousands of classes!</p>
<p><strong>Then you will also need the latest version's dumps from <a href="https://github.com/DRG-Modding/Header-Dumps">here</a>.</strong> You will need to download a text editor or code viewer such as <a href="https://code.visualstudio.com/download" target="_blank" rel="noopener noreferrer">Visual Studio Code</a> to view and navigate the dumps (I use <a href="https://www.jetbrains.com/clion/" target="_blank" rel="noopener noreferrer">CLion</a>).</p>
<h3>Kill player on button press</h3>
<p><strong>Scouting the C++ header dumps</strong></p>
<p>Now, you will notice that there are a LOT of C++ classes in the dumps. Don't be overwhelmed &ndash; most of the main functions you will need are in <code>FSD.hpp</code> (although saying that, the file alone is 20k lines). <em>If you need C++ from another class you know at the time what you are looking for &ndash; and if you don't, you can always search for key words that you are looking for and look through those files. You will be doing a lot of searching through files anyway, so you'll get good at this pretty quickly. Don't hesitate to ask in <code>#mod-chat</code> in the Discord where stuff might be located though.</em></p>
<p>So, back to this mod example. We need to find the function in the game that deals with killing the player. As you should always do, start by looking in <code>FSD.hpp</code> as it is most likely to be there. Do <code>Ctrl + F</code> to open the search function (this is the same for all IDEs and code editors), and type <code>Kill</code>. You may see about 100 results. <em>You could look through these manually, OR, you could try toggling on "match by word" option that most IDEs and code editors have. In CLion, it is a square button with a "W" on it.</em></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_834ed908a16041f6.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image034.png" alt="image034" width="272" height="29" /></p>
<p>So, here is a function that just kills the player when it is called. An <code>AActor</code> object is passed through but you can just ignore that as we don't need it.<strong> The important thing to do here is to check what class this function is inside. This is again relevant for any other functions you will find for your mods.</strong> So here, <code>Kill()</code> is inside struct <code>UHealthComponentBase</code>, which is a child class of <code>UActorComponent</code>, as denoted by the <code>:</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_b98bb7d02897f799.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image035.png" alt="image035" width="601" height="648" /></p>
<p><em>When you are looking for functions and variables for your mod, you will come across some really interesting looking functions and variables. I have regularly come up with entire mod ideas just from being distracted when looking through the code. Every single function and variable in all these header files can be dummied&hellip; which is a lot of possibilities. Feel free to go wild!</em></p>
<p><strong>Accessing the C++</strong></p>
<p>Now we know where our <code>Kill</code> function is, we can go about accessing it from our BP mod. Make a new mod (I just made a new one in my tutorial folder and changed the <code>InitSpacerig</code> and <code>InitCave</code> to spawn that instead).</p>
<p>Inside your mod, right click and get a node called <code>Get Component by Class</code>. In the dropdown, you should be able to find the <code>HealthComponentBase</code> class:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_53a4e626088f8c95.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image036.jpg" alt="image036" width="262" height="317" /></p>
<p>Then if you drag off the return value pin and type <code>Kill</code>, you should see a function that pops up called <code>Kill</code>. This is your function that you created inside the <code>HealthComponentBase</code> C++.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_7404b395eb530c64.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image037.png" alt="image037" width="266" height="127" /></p>
<p>Now when we press a key, say, the period, we want to kill the player:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_ee927d8282109268.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image038.png" alt="image038" width="268" height="156" /></p>
<p>There is a problem with this though! You cannot just call the <code>Kill</code> player function, as you will notice that the component by class node requires a target, which will be the player to kill.</p>
<p>To get the player you want to kill, we need to get the game state player array, loop through that player array then cast the array elements to <code>BP_PlayerCharacter</code> pawns, which you can then use as the targets.</p>
<p><strong>Dummying player character BP</strong></p>
<p>But what is <code>BP_PlayerCharacter</code>? This is a blueprint class that the game has that controls all the logic for the player. We need to dummy this in order for our killing player to work.</p>
<p>In the dumps, navigate to <code>APlayerCharacter : ACharacter</code>. You will see that this is where all the info about the player is stored. <code>ACharacter</code> inherits from <code>APawn</code>, which inherits from <code>AActor</code>. This is important to know because when you dummy blueprints, you can set the parent to be super parent (i.e. the highest in the hierarchy).</p>
<p>Create a folder inside <code>Content</code> called <code>Character</code>. Here we are recreating the exact file path of the asset (you can find <code>BP_PlayerCharacter</code> inside of your unpacked files at this location). Now you need to create a new blueprint class called <code>BP_PlayerCharacter</code>. Inherit it from <code>PlayerCharacter</code>.</p>
<p><strong>Once you create this blueprint, you don't need to do anything with it. Since we aren't dummying this file, we don't want to overwrite anything in the game with it so that is why we don't change any values. All we are doing is accessing the REFERENCE to the blueprint for the mod.</strong></p>
<p>Now, back in your mod, let's get to work on this logic.</p>
<p>First, there is a node from the UE base <code>Gameplay Statistics</code> library called <code>Get Game State</code>. Drag off the return value and type <code>Player Array</code>; we want to get that. Then drag off player array and create <code>For Each Loop with Break</code> node. This will loop through all players in the player array and stop when it receives the break call. <em>If you are in a multiplayer server, all the players in the game will be registered in this player array. So, if you just use a for each without a break, you could kill all the players in the game, if you wanted. This cluster of nodes will be very useful for your mods.</em></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_a6e336f3887b1c37.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image039.png" alt="image039" width="336" height="284" /></p>
<p>Now drag off the loop's array element and type <code>Pawn Private</code>. Then from the output of that read node, type <code>Cast To BP_PlayerCharacter</code>. Connect the execution node for that cast into the loop body.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_53a08d9d7b6b8a21.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image040.png" alt="image040" width="525" height="231" /></p>
<p>Now plug in your <code>Kill</code> function execution node, and your target from <code>As BP Player Character</code> from the cast. Then, if you want to kill just the first player in the player array, hook up the end of the kill function execution to the break for the for each loop with break node. You don't have to do that though, as mentioned above.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_75af9c4bf1ec4d12.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image041.png" alt="image041" width="602" height="167" /></p>
<p><strong>Make sure that you enable input on <code>BeginPlay</code>!</strong></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_e64b78c1d8cf7fdd.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image163.png" alt="image042" width="600" height="332" /></p>
<p><strong>Before you package your mod, you have to make sure that you set your directories to never cook to include the <code>Character</code> folder. This is because if we pack our dummied <code>BP_PlayerCharacter</code>, the game will crash. This is explained in section 6, packaging your mod.</strong></p>
<h3>Self-destruct Bosco on button press</h3>
<p><strong>Scouting the C++ header dumps</strong></p>
<p>First off, we want to find <code>BP_Bosco</code> in the header dumps. It is nicely called <code>BP_Bosco.h</code>. You will see a function called <code>SelfDestruct()</code> inside of it.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_dc60fff90a21b98.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image043.png" alt="image043" width="260" height="407" /></p>
<p>If you go into <code>FSD.hpp</code>, and search for <code>ABosco</code> (which is what this class inherits from), you will also see <code>SelfDestruct</code> and a bunch of other functions. Let's say that I also want Bosco to salute before he self-destructs. <em>When writing this tutorial I was just planning on him self-destructing but saw that <code>PlaySalute()</code> function and just knew I had to include it. That'll happen a lot when you are looking through the dumps :)</em></p>
<p>Before we make the dummy blueprint, we need to figure out what class it needs to inherit from. So, if you look at the <code>struct ABosco</code> line, you will see a <code>:</code>, which as described previously, means "inherits from", then the class it inherits from to the right. So here we see that <code>ABosco</code> inherits from <code>ADeepPathfinderCharacter</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_5f56a92966782fb0.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image044.png" alt="image044" width="404" height="43" /></p>
<p>If your IDE has the feature, you should be able to just hover over the name and it will tell you what that class inherits from and click to go to the location of it. If not, don't worry, as you can just search the file for that classname manually.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_e466c94b9cca4e73.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image045.png" alt="image045" width="412" height="107" /></p>
<p>So you will see now that this class is inherited from <code>AFSDPawn</code>. Now we search for what <code>AFSDPawn</code> is inherited from, and we will see <code>APawn</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_e7c2345e10c763a8.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image046.png" alt="image046" width="350" height="35" /></p>
<p><code>APawn</code> is a base UE class so that's as far as we need to go &ndash; but now we know, that <code>ABosco</code> is a child class of <code>APawn</code>, even though it is a bit down the inheritance tree.</p>
<p><strong>Creating the dummy BP</strong></p>
<p>Now, we need to recreate a dummy blueprint in the same location where this guy is in the game files, like we do for hex mods. So in your unpacked files, search for <code>BP_Bosco</code>. It should be inside <code>Content\GameElements\Drone</code>.</p>
<p>Inside your project files, navigate to that file location and create a new blueprint class. Since we discovered that <code>ABosco</code> is a child class of <code>Pawn</code>, we can select <code>Pawn</code> class to inherit from. We name the blueprint <code>BP_Bosco</code>. It should look like this:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_61b05603235ab5a2.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image047.png" alt="image047" width="72" height="102" /></p>
<p>Inside of this, all we have to do, is create two functions, one called <code>SelfDestruct</code>, and another called <code>PlaySalute</code>. That is literally all you have to do. This now allows us to call these functions without having to touch any C++.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_88c233e24cd1b740.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image048.png" alt="image048" width="285" height="87" /></p>
<p>Now, to call on our blueprint, all we have to do is create a node <code>Get Actor of Class</code> and select <code>BP_Bosco</code> in the little dropdown. Then drag off the return value and type <code>Play Salute</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_b043c039c53dbbb9.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image049.png" alt="image049" width="602" height="149" /></p>
<p>Now, let's put a short delay between saluting and self-destructing, and then call the self-destruct function.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_a4d80d64a96677eb.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image050.png" alt="image050" width="602" height="99" /></p>
<hr />
<h2>6. Packaging your mod</h2>
<p>When you want to test your mod, there are two options for packing:</p>
<ol>
<li>Automatically using <a href="https://github.com/DRG-Modding/DRGModdingAutomationScripts" target="_blank" rel="noopener noreferrer">DRGModdingAutomationScripts</a> that Samamstar and DrTurtle made. I really highly recommend using this as it saves a LOT of manual time!</li>
<li>Manually from UE then using <code>DRGPacker</code>.</li>
</ol>
<p>There's also directories to never cook, which is an extremely important setting, so make sure you scroll down to read that part too.</p>
<p>For either method, if you are getting a game crash on launch that looks like this:</p>
<pre><code>Error: AddAssetData called with ObjectPath /some/path/ which already exists. This will overwrite and leak the existing AssetData.
Error: AddAssetData called...
...
...
[2023.05.13-21.27.01:936][ 0]LogCompression: Error: FCompression::UncompressMemory - Failed to uncompress memory (3103/9686) from address 000002672E4A73C0 using format LZ4, this may indicate the asset is corrupt!</code></pre>
<p>Then you need to go into packaging settings and uncheck <code>Share Material Shader Code</code>.</p>
<p>&nbsp;</p>
<h3>Using DRG Modding Automation Scripts</h3>
<p>First head to <a href="https://github.com/DRG-Modding/DRGModdingAutomationScripts" target="_blank" rel="noopener noreferrer">this GitHub repository</a>. Then hit the big green <code>Code</code> dropdown and download the code using your preferred method (e.g. download as <code>ZIP</code>). Then extract the <code>DRGModdingAutomationScripts</code> folder and follow the instructions on its <code>README</code> file. Once you are setup, all you need to do is double click a .bat and it'll do whatever the bat says it does in the <code>README</code>.</p>
<h3>From UE</h3>
<p>Go into the <code>Untitled</code> tab. Click <code>File -&gt; Package Project -&gt; Packaging Settings</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_b15a738f201ab28.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image106.png" alt="image106" width="208" height="233" /></p>
<p>Make sure that this <code>Use Pak File</code> option is <code>off</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_ff72eae0e512fd3d.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image107.jpg" alt="image107" width="219" height="108" /><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_42f09e0cf4b0f387.png" alt="" /></p>
<p>Now, package your project by clicking <code>File -&gt; Package Project -&gt; Windows (64-bit)</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_5423d6863c13d912.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image111.png" alt="image111" width="415" height="156" /></p>
<p>Then selecting a folder to package to:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_56daa003645cc717.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image112.png" alt="image112" width="601" height="196" /></p>
<p><em>Tip: if you already have an old cooked folder in there, delete it then click on the parent folder again in the top bar. Because otherwise, since you selected the old folder, it will cook into <code>WindowsNoEditor</code> again, even though you deleted the old one. This sounds confusing so if you just try it yourself you'll understand what I mean.</em></p>
<p>Assuming you didn't do anything wrong, your project should package after about 30 seconds (differs on how large your project is - when using the <code>FSD-Template</code>, the first time around takes many minutes).</p>
<p>Now, navigate to the packaged files and delete any stuff you don't want to pak (other mods etc.).</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_dfe3f919c5bd0ad7.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image113.png" alt="image113" width="367" height="111" /></p>
<p>Now copy both the <code>Content</code> AND <code>AssetRegistry.bin</code> files, and put them into the input folder inside your <code>DRGPacker</code>.</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_766abac0c7a1a54a.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image114.png" alt="image114" width="328" height="100" /></p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_32143836eb62a607.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image115.png" alt="image115" width="201" height="122" /></p>
<p>Then drag the input folder into <code>_Repack.bat</code>. Remember to rename the .pak to your mod name. Then navigate to <code>DeepRockGalactic\FSD\Mods\</code> and make a new folder with the same name as your mod. Then put the <code>.pak</code> into that folder.</p>
<h3>Directories to never cook</h3>
<p>You can set directories to never package, which is useful when you don't want to pack say, any of the dummy BP folders. Although you don't have to do this, it does mean that you don't have to delete these files manually within the cooked files, every time. To do this, click the little dropdown arrow in package settings, just above the <code>Project</code> tab:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_eee622eeef66fe89.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image109.png" alt="image109" width="602" height="70" /></p>
<p>Then in directories to never cook, press the <code>+</code> button to add directories to the array. This is what mine currently looks like:</p>
<p><img src="https://static.mod.io/html/external/tinymce/RackMultipart20220529-1-zcbkdd_html_51715c584d87ba6f.png" alt="" /><img src="https://image.modcdn.io/members/6da1/6436408/profilebp/image110.png" alt="image110" width="259" height="295" /></p>
<hr />
<h2>More Help</h2>
<ul>
<li>codertrevor has created his own blueprint modding guide with some really great examples to run through: <a href="https://codertrevor.github.io/DRG-Modding-Tutorials/">https://codertrevor.github.io/DRG-Modding-Tutorials/</a></li>
<li>There are some forum posts in the modding discord that you can follow/ask questions in about misc questions people have had, e.g. <a href="https://discord.com/channels/676880716142739467/1047646725344591882" target="_blank" rel="noopener noreferrer">"First BP Experience" </a></li>
<li>The #learn-blueprints channel in the modding discord has some nice snippets of blueprints that you may find yourself needing</li>
</ul>
<hr />
<h2>Video Tutorial</h2>
<h3><a href="https://www.youtube.com/playlist?list=PLyexxYrpv6y2y1_tpHMr9CiBVJuY9GDm0">Tutorials playlist</a></h3>
<blockquote><strong>Important announcement:</strong> these video guides are now very out of date, with methods inside still working but newer, less tedious methods now exist. I suggest that you follow this written guide first.</blockquote>
<p>Created by myself, featuring a segment recorded by <a href="https://mod.io/u/samamstar">Samamstar</a> and edited by <a href="https://mod.io/u/handdrawnnerd">Hand Drawn Nerd</a>, this guide aims to walk people through the complex process of BP Modding!</p>
<p>These videos are not intended to replace the written guide.</p>
<p>The segments recorded include:</p>
<ol>
<li>Introduction info and setting up your workspace</li>
<li>The basics of setting up native spawning, native spawning with DRGLib &amp; pre-native spawning with BPMM</li>
<li>The no-dummy BP modding method, using the pre-native BPMM method and then converting the same mod to native using BPMM's interface</li>
<li>The dummy BP modding method, which includes how to navigate the C++ header dumps, as well as reflect them and dummy BPs</li>
<li>Reflection demo, which shows exactly how to reflect some certain C++ objects/delegates/functions/properties that weren't covered in episode 4</li>
</ol>
<hr />
<h2>Feedback</h2>
<p>Hello modder! If you found this guide useful, I invite you to rate it in this <a href="https://forms.gle/LHNyv9W3oquYyeoc6">form</a>. Feedback is optional, but very welcome.</p>
