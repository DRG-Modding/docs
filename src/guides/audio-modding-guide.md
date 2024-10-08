# Audio Modding Guide

<h2><strong>DRG Mods: A Comprehensive Guide to Audio Modding</strong></h2>
<blockquote>
<p>Please do not hesitate to ask for help! You can find talented modders in the <a href="https://discord.gg/tmFJcesA6d">DRG RND Discord</a> or the <a href="https://discord.gg/nkPhp2sZfd">DRG Modder's Guild Discord</a>.</p>
<p><strong>Credits:</strong><br />Buckminsterfullerene - Originally wrote and maintains guide.<br />Kraeus - Helping out with knowledge tidbits.<br />Dr Turtle - Typo/grammar/link fix pass, rewrote replacing axe impact sounds section.</p>
</blockquote>
<h2>Watch this video guide while reading the written guide for extra information!</h2>
<p><iframe src="//www.youtube.com/embed/FuAbj7ZmmCI" width="729" height="410" frameborder="0"></iframe></p>
<hr />
<h2>Contents</h2>
<ul>
<li>1. Introduction
<ul>
<li>Tools</li>
<li>Reading this guide</li>
<li>Types of audio mods</li>
<li>Pre-requisites</li>
<li>Audio mods filename prefixes</li>
</ul>
</li>
<li>2. Using the Audio Modding Template</li>
<li>3. Replacing cues method
<ul>
<li>Replaces axe throw sound</li>
<li>Adding new boss music with custom tracks</li>
<li>Volume control</li>
</ul>
</li>
<li>4. Replacing sounds files method
<ul>
<li>Replacing jukebox songs</li>
<li>Replacing axe impact sound</li>
</ul>
</li>
<li>5. Packing your mod
<ul>
<li>Using DRGPacker</li>
<li>Using the new packing method</li>
</ul>
</li>
<li>6. Installing your mod
<ul>
<li>Why won't my mod auto-verify in-game even though I've set the auto-verify tag?</li>
</ul>
</li>
<li>7. Extra notes and help
<ul>
<li>Why don't the game sound waves play anything in the all-assets branch of the audio template?&nbsp;</li>
<li>Why can't I hear anything when I play some cues in-editor using the audio template?&nbsp;</li>
<li>Windows 10 explorer audio file loading time</li>
<li>Memorial Hall Cue (thanks Kraeus/KrysPolezoes)</li>
<li>What is single vs looping cues in Music_Wave?</li>
<li>Spacerig music cue solution (thanks SASHA)</li>
<li>Getting help</li>
<li>Kraeus' Music Breakdown
<ul>
<li>Boss</li>
<li>End Wave</li>
<li>Level</li>
<li>Menu</li>
<li>Special Events</li>
<li>Wave</li>
<li>Spacerig</li>
</ul>
</li>
</ul>
</li>
<li>Feedback</li>
</ul>
<hr />
<h2>1. Introduction</h2>
<h3>Tools</h3>
<p>Before you get started with your mod, you should know about a few tools:</p>
<ul>
<li><a href="https://drg-modding.github.io/tools/basic-tools.html" target="_blank" rel="noopener noreferrer">DRGPacker</a></li>
<li><a href="https://github.com/iAmAsval/FModel/releases" target="_blank" rel="noopener noreferrer">FModel</a></li>
<li><a href="https://github.com/DRG-Modding/Audio-Modding-Template" target="_blank" rel="noopener noreferrer">Audio Modding Template Project</a></li>
<li><a href="https://github.com/DRG-Modding/Frameworks/tree/main/Empty%20Content%20Hierarchies" target="_blank" rel="noopener noreferrer">EmptyContentHierachy</a> (not really a tool but very useful)</li>
<li><a href="https://www.unrealengine.com/en-US/download" target="_blank" rel="noopener noreferrer">Unreal Engine</a> <code>4.27.2</code>(it will look like just 4.27.0 on the epic games launcher, which is good to download)</li>
</ul>
<blockquote><strong>IMPORTANT: By the time you are reading this guide, you should already have these tools and have at least the minimum required knowledge to use them (more on UE4 later). If not, I refer you to Rauliken's more general <a href="https://mod.io/g/drg/r/drg-basic-modding-guide">guide</a>.</strong></blockquote>
<h3>Reading this guide</h3>
<p><strong>Make sure that you read through every detail of this guide thoroughly as missing something may result in many problems down the line. Watch the <a href="https://youtu.be/FuAbj7ZmmCI">video guide</a> for easier following along.</strong> Of course, you can always refer back to this if you need assistance on anything. Critically important details are highlighted in <strong>bold</strong>, and optional but useful information is highlighted in <em>italics</em>. <code>Inline code</code> is used for 'specifics'. Also, if you are stuck on something, it may be useful to refer to the <a href="#extra-notes-and-help">random but possibly useful notes</a> at the end.</p>
<h3>Types of audio mods</h3>
<p>There are two types of audio mods:</p>
<ol>
<li>Editing the audio cue files to play new sounds of your own. The cue files have to use the same name and location in the game structure so that they will be loaded by the game. This method is best because it gives you more control of the sounds you are playing, as you can add logic in the blueprinting part (we will get to that later).</li>
<li>Adding sounds with the same name and location as the original sounds, so the original ones get replaced and the original (untouched) cue file now plays your sounds. This method is much faster than the first but gives you far less control of your sounds.</li>
</ol>
<p>You can use either method, depending on your situation. If you want to just replace some say, background music, with some of your own, you could use the second method. If you wanted to add new music alongside vanilla music, you would have to use the first method.</p>
<h3>Pre-requisites</h3>
<p>For either method, you will need your audio files to be in the<code> .wav</code> or<code> .ogg</code> file formats. There are plenty of online audio converters to and from various file formats.</p>
<p>The reason there are two different formats is that there are pros and cons of using either of them:</p>
<ol>
<li><code>.wav</code> is around 4x larger in file size; however, it requires very little messing about with volume control.</li>
<li><code>.ogg</code> is more compact than <code>.wav</code> but <strong>may</strong> require gain to be reduced using Audacity (or a similar external program) and output volume to be reduced by ~25% otherwise it <strong>may</strong> produce horrible audio popping artefacts. Volume control is slightly more difficult (subjective), but that is explained in the <a href="#volume-control">Volume control</a> section.</li>
</ol>
<p><strong>If you're getting audio popping, super low quality, or bass-boosted sort of sounds, then try switching to the other format. It seems like you have to play it by ear for which format you use for every project. Some projects do better with </strong><code>.wav</code><strong>, some better with </strong><code>.ogg</code><strong>.</strong></p>
<h3>Audio mods filename prefixes</h3>
<p><code>ST</code> &ndash; Soundtrack</p>
<p><code>MSC</code> &ndash; MuSiC</p>
<p><code>Cue</code> &ndash; Sound Cue</p>
<p><code>UEE</code> &ndash; Unreal Engine Editor</p>
<hr />
<h2>2. Using the Audio Modding Template</h2>
<p>The <a href="https://github.com/DRG-Modding/Audio-Modding-Template" target="_blank" rel="noopener noreferrer">audio modding template</a> is a set of files that allows you to skip a lot of manual work when making mods. All of the assets inside of it are generated by some tools developed by the community. This means that they have all of the same properties as if you had the game's original project open.</p>
<p><strong>There are two branches:</strong></p>
<ul>
<li>The main branch, which is the default one. The vast majority of audio modders will want to use this branch, as it does not include the sound cues and sound waves, but includes everything else. It means that you just need to make the sound cues yourself, which will be explained later.</li>
<li>The all-assets branch, which contains every sound asset in the Audio folder in the game, including sound cue graphs and sound waves. Switch to this branch if you want to copy the reconstructed sound cues with the reconstructed node graphs.</li>
</ul>
<p>You can switch branch by hitting the branch dropdown on the GitHub repository like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/branch_dropdown.png" alt="branch dropdown" width="347" height="294" /></p>
<p>To download the template, select your desired branch, and then select an option from the green code dropdown (the easiest being download ZIP):</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/code_dropdown.png" alt="code dropdown" width="347" height="292" /></p>
<p>Then to use, just double click the <code>FSD.uproject</code> file (making sure that you have UE installed of course).</p>
<p><strong>If you downloaded the main branch: </strong>navigate to your new project in file explorer. Copy the <code>Characters</code>, <code>Music</code>, and <code>SFX</code> folders from inside <code>EmptyContentHierachyU36/Content/Audio</code> (these are the same as the game) and put them inside your project&rsquo;s <code>Content/Audio</code> folder. It should look something like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture32.png" alt="picture32" width="823" height="169" /></p>
<p><em>You don&rsquo;t have to do this &ndash; you could just manually create or copy the folder structure for only the folders that you plan to use (e.g. in our case the Audio folder). But this makes things nice and simple for you as it reduces manual labour.</em></p>
<p>Add a new folder within <code>Content</code> called <code>_&lt;your mod name&gt;</code>. For this tutorial I'll use <code>_CustomSounds</code>. Whatever you call it, do not forgot to have an underscore at the start of the name. The underscored files are always at the top of the content browser for ease of use, so that is where you can store the sounds that will be used in the audio cue files. They are also scanned by mod.io for assets that help determine the auto-verified tag.</p>
<hr />
<h2>3. Replacing cues method</h2>
<p>I'm now going to show you 2 different examples within this method (as they require slightly different processes):</p>
<ul>
<li>Replacing the impact axe throw sound with a custom one</li>
<li>Adding new boss music with custom tracks <strong>alongside</strong> the vanilla ones</li>
</ul>
<h3>Replacing axe throw sound</h3>
<p><strong>Making the cue</strong></p>
<p>For this example, use the <code>main</code> branch project from the audio modding template.</p>
<p>First select and download any sound effect of your choice that fits what you want, from the internet. Make sure it&rsquo;s in the <code>.wav</code> or <code>.ogg</code> formats. I went with a cry for pain sound. Place it into the <code>_CustomSounds</code> (or whatever you called it) folder within your project &ndash; <strong>the filename does not matter.</strong></p>
<p>If you have the project open when you do this, a little window should pop up in the bottom right corner asking if you wish to import the file:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture4.png" alt="picture4" width="255" height="40" /></p>
<p>Click Import. This will automatically create a paired <code>.uasset</code> file.</p>
<p>If the import window does not show, click the big green "Import" button on the content browser and do it manually that way.</p>
<p>Next, you need to create the cue file with the same name and location as the <code>AxeGrenadeFoldOut_Cue</code> file. If you look through the game&rsquo;s unpacked files, you should find <code>AxeGrenadeFoldOut_Cue</code> in <code>Content\Audio\SFX\WeaponsNTools\ITM_Grenades\AxeGrenade</code> &ndash; it should look like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture5.1.png" alt="picture5 1" width="602" height="172" /></p>
<p><em>Side note: when looking for other files, generally you have to use a bit of common sense to find them. However, even that is not enough, as the game&rsquo;s file naming conventions are practically non-existent &ndash; some files are named in really, really dumb ways that make them hard to locate. If you have any experience of other modding types, you will understand what I mean.</em></p>
<p>So now back within your UEE project, navigate to the right file location in the file explorer. Then, right-click, go down to sounds, and click sound cue:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture6.png" alt="picture6" width="319" height="377" /></p>
<p>You need to name this file exactly the same as the cue you are changing, e.g. in this example <code>AxeGrenadeFoldOut_Cue</code>.</p>
<p>Just to check, your content browser should look like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture7.1.png" alt="picture7 1" width="602" height="89" /></p>
<p>Double click the sound cue you just made. It should open a new window in an audio blueprint editor.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture8.png" alt="picture8" width="330" height="255" /></p>
<p>To add the custom sound, you can either navigate to the <code>_CustomSounds</code> folder and drag the sound in, or you can add a <code>Wave Player</code> node and in details (click on it), select your sound.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture9.png" alt="picture9" width="373" height="118" /></p>
<p>The most basic thing you can do here is directly connecting the <code>Wave Player</code> node to the <code>Output</code> node.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture10.png" alt="picture10" width="424" height="137" /></p>
<p>This works if you press <code>Play Cue</code> in the editor, however, with very little effort you can do a lot better.</p>
<p><em>Side note: if your sound effect produces weird audio artefacts (like crackling) even within UEE when you hit the Play Cue button, you will need to manually go into a program like Audacity and reduce the amplification (Google how to do that if unsure; it is very simple to do). This can either happen if you are using the wrong audio format (make sure you are using .wav or .ogg!) or if the audio amplification is too high for UEE.</em></p>
<p><strong>Setting soundclass/attenuation/submix references in your cue</strong></p>
<p>If you click on the <code>Output</code> node, you will see in the details pane that you need to <em>select</em> a <code>Sound Class</code>, an <code>Attenuation</code> and a <code>Base Submix</code> to inherit from. If you click the little drop-down box for each, you will see all of the assets inside of our project already - this is due to our template project already having these inside for you to use instantly!</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture33.png" alt="picture33" width="408" height="586" /></p>
<p>Because the game's file naming convention is practically non-existent, it may not be clear which to use in your mod. This is where FModel comes in. You can download it from <a href="https://github.com/iAmAsval/FModel/releases" target="_blank" rel="noopener noreferrer">here</a>.</p>
<p>To use FModel, open it up and hit the Directory button in the top right, then Selector.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture24.png" alt="picture24" width="314" height="144" /></p>
<p>A window will pop up and you should enter your DRG install path. FModel will then detect all the Pak files inside it. You can then select <code>FSD-WindowsNoEditor.pak</code> and hit Load.</p>
<p>Now, navigate through the folder dropdowns to the <code>Content\Audio\SFX\WeaponsNTools\ITM_Grenades\AxeGrenade</code> folder. You will see that one of the tabs at the top will say &ldquo;3 Packages&rdquo;; click on that, and you will see the 3 cue files. Double click on the <code>AxeGrenadeFoldOut_Cue.uasset</code>.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture35.png" alt="picture35" width="633" height="394" /></p>
<p>Now, this JSON gives us some details about the cue. The bit we care the most about, however, is this part:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture34.png" alt="picture34" width="632" height="198" /></p>
<p>It tells us:</p>
<ul>
<li>The sound class that it uses is called <code>ExplosionsManualMix</code></li>
<li>The attenuation that it uses is called <code>DistantWide_Misc_Attenuation_Cue</code></li>
<li>The submix that it uses is called <code>GeneralWeaponsToolsSubmix</code></li>
</ul>
<p>Make sure you select these assets in the drop-down on the cue. So now you know how to find out which assets any of the cues you are replacing use.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture36.png" alt="picture36" width="405" height="438" /></p>
<p>So now, you should have a programmed cue file! There are some other nodes you can use, like random and modulator, but I will explain those in a later example.</p>
<p>Make sure you hit save or Ctrl+S!</p>
<p>You need to do <a href="#h_8434116321361652145131167">volume control</a> on your mod to make sure that the sound levels seem about right, which means getting into a game and playtesting!</p>
<p><strong>Refer to section 5 on <a href="#h_161987188221652147544375">packing your mod</a> to finish up.</strong></p>
<h3>Adding new boss music with custom tracks</h3>
<p><strong>Making the cues</strong></p>
<p>For this section, I highly recommend downloading the <code>all-assets</code>branch from the audio modding template, and then copying in the specific files that you need for your mod into your existing project (simple copy and paste via file explorer works fine here).</p>
<p><strong>If you plan on making any type of music mod, please go to section 5 on Kraeus&rsquo; music breakdown. There, he lays out the associations between the music cues, audio files, and song names. If you don&rsquo;t look at this, you will likely struggle. Alternately, you can open it separately <a title="Music Breakdown" href="https://github.com/DRG-Modding/Guides/blob/main/Music%20Breakdown/Music_Breakdown.pdf" target="_blank" rel="noopener noreferrer">here.</a></strong></p>
<p>Now, we are going to add boss music. Download any music of your choice, again in the <code>.wav</code> or <code>.ogg</code> file formats. Place these tracks (as many as you would like) into the<code> _CustomSounds</code> folder like before. It is important to note that because this example is adding custom music <em>on top</em> of vanilla music, but we are replacing cue files, we will also need to get referemces to the vanilla music.</p>
<p>Therefore, what we can do, is - <strong>making sure that we have UE closed for this -</strong> copy in the 3 cues and the entire <code>AudioFiles</code> folder from <code>Content/Audio/Music/Boss</code> folder from the<code> all-assets</code> branch on the audio modding template, to the same locations in our own project. We also need to copy in the entire <code>Content/Audio/SoundControl</code> folder if you haven't already got it in your project. This contains all the sound classes, attenuations, etc.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture37.1.png" alt="picture37 1" width="367" height="180" /></p>
<p>The reason that there are 3 cue files, is that there is one for each track. <em>The reason that GSG uses one song per Cue is so everyone hears the same song. With <code>Random</code> nodes in our Cues, we all hear the same Cue but the song we hear is random per each client. So there is no guaranteed song parity. </em>When you have multiple cue files, there are 2 ways of using them (in this example):</p>
<ol>
<li>Put one vanilla track in each, and then split your custom tracks between the 3 cues. I will explain how to combine them a bit further on.</li>
<li>Put everything in one cue, and then just copy and paste everything inside the first cue into the others. That way, you only have to make changes in the first and just copy and paste into the others without having to match in each. This is super time-saving when working in other cue types (e.g. wave music) that may have 10 or more cues. <strong>We will use this option.</strong></li>
</ol>
<p>Open one of the cue files, say <code>St_Boss_Music_Cue</code>. Inside this, first, you want to drag in the new sounds that you want to play. Since we will be going with the 2nd method, chuck everything in and set them to <code>looping</code> (most music cues by default have looping wave players), like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture38.png" alt="picture38" width="352" height="454" /></p>
<p>To connect these, but only have one play at once, you can use the <strong>random</strong> node. You can add more inputs by clicking the &ldquo;Add input&rdquo; plus symbol on the node. Now, just connect the tracks to the input, and the output on the random node to the Output node, like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture39.png" alt="picture39" width="879" height="421" /></p>
<p>You can see that, since we copied the sound cues from the audio modding template, they already have their sound class and submix references set. <em>Music cues don't have attenuation because of course they are global sounds - no matter where in the world you are listening from, the volume should stay the same</em>.</p>
<p>Now, copy and paste these nodes into the other 2 boss cues, making sure you hook up the random node to the output node in the cues.</p>
<p>You need to do <a href="#h_8434116321361652145131167">volume control</a> on your mod to make sure that the sound levels seem about right, which means getting into a game and playtesting!</p>
<p><strong>Packaging</strong></p>
<p>When you go to package this mod and test it in-game, you will probably notice that it sometimes plays silence or never plays the vanilla tracks. This is because we haven't explicitly told UE to not package the dummy sound waves inside of the <code>AudioFiles</code> folder. So when the mod is being loaded into the game, it's replacing the vanilla boss tracks with empty ones, which is no good.</p>
<p>So, make sure that in the <code>Directories To Never Cook</code> setting explained in the packaging your mod section, you need to add the <code>Audio/Music/Boss/AudioFiles</code> folder to that list. That way, when your mod is packaged, it leaves out the dummy files, like the sound control ones. Make sure that you remember to do this for any mod that makes references to dummied vanilla sound waves! <em>It's nice and handy that the devs usually put the sound waves in a seperate <code>AudioFiles</code> folder meaning that you can do this easily. However, if the sound waves are in the same directory as the cues, then you'll have to manually delete the sound waves after packaging from UE.</em></p>
<p><strong>Refer to section 5 on <a href="#h_161987188221652147544375">packing your mod</a> to finish up.</strong></p>
<h3>Volume control</h3>
<p>When testing either of these methods, it may come apparent that some sounds are too loud whilst some are too quiet. So, there are few methods for volume control that do the same thing:</p>
<ul>
<li>Modulator node method</li>
<li>Audio file modulating method</li>
<li>Just changing the overall volume multiplier in the main sound cue settings (self-explanatory)</li>
</ul>
<p><strong>Modulator node method</strong></p>
<p>Of course, you could go into the individual sound files and adjust them manually in Audacity or something. But that could get very time-consuming very quickly (if you have many audio files), and there is a much better way of doing it. <code>Modulator</code> nodes!</p>
<p>The modulator node allows you to change the minimum and maximum pitch and volume of the input sound. So, say you want the volume of a specific track to be twice as loud, you can set the min and max volume to <code>2</code>. For twice as quiet, you&rsquo;d want <code>0.5</code>. I haven&rsquo;t messed with changing min and max to different values and seeing if they actually work, so feel free to mess with them yourself.</p>
<p>So, to use the <code>Modulator</code> nodes, just place them between the <code>Wave Player</code> nodes and the <code>Random</code> node, like this:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture17.png" alt="picture17" width="333" height="56" /></p>
<p><strong>Audio file modulating method</strong></p>
<p>This is basically another form of the previous method. Instead of changing the volumes inside the cues, you can just change output volume from the actual sounds themselves. If you don&rsquo;t know how to do that; you can double-click on an audio file, and it will display volume, pitch, and other settings. It has its own built-in modulator. This method is good if you use the same audio file in many different places <em>unless</em> you want it to have different volumes or pitches in the different places, as you would need to use modulators inside the cues anyway.</p>
<hr />
<h2>4. Replacing sounds files method</h2>
<p>For this method, you will need your replacement audio files to be in the <code>.ogg</code> or <code>.wav</code> format, with the same pros and cons that came with the previous method. <strong>You will still need UEE for this.</strong></p>
<p>Set up a new project using the <code>main</code> branch from the audio modding template, as you would in the previous method.</p>
<p>I&rsquo;m going to go through and explain 2 examples as they differ slightly:</p>
<ul>
<li>Replacing a jukebox song with a custom one</li>
<li>Replacing the axe impact sound</li>
</ul>
<h3>Replacing jukebox songs</h3>
<p>Download the audio you want to replace in the, again, <code>.ogg</code> or <code>.wav</code> format. Locate the location and name of the jukebox song(s) you wish to replace. For example, let&rsquo;s replace the <code>Jukebox_Blues_TheBluesSchool</code> song with our own. This is located in <code>Content\Audio\Music\JukeBox</code>.</p>
<p><em>There are some other folders with even more songs in, like <code>NewMusic</code> and <code>NewMusic_june2020</code> which is blatantly rubbish file naming/organization. However, the <code>TRJMusic</code> folder contains all of the non-copyrighted music provided when you check the streamer mode box in-game.</em></p>
<p>Get your song, and import it into your UEE project, into that location, naming it the same as the file you wish to replace. <strong>Earthcomputer wrote a script that renames all your jukebox replacement songs automatically, which can turn 10-20 mins into 30 seconds (if you know how to use Python). You can find that <a href="https://github.com/DRG-Modding/Useful-Scripts/tree/main/Auto-rename%20Jukebox%20Songs" target="_blank" rel="noopener noreferrer">here</a>.</strong></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture18.png" alt="picture18" width="602" height="116" /></p>
<p>There is a bit of volume control you can do. You can use the same method as the <a href="#h_664798342211652145534360">audio file modulating</a> method, but basically, you can double click an audio file and edit its volume. <em>I&rsquo;ve found that I tend to set the volumes to <code>1.5</code> or <code>2</code> when downloading songs from <code>YouTube</code> to <code>.wav</code>. You&rsquo;ll find that different volumes work for different things anyway.</em></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture19.png" alt="picture19" width="260" height="126" /></p>
<p>And you&rsquo;re done! <strong>Refer to section 5 on <a href="#h_161987188221652147544375">packing your mod</a> to finish up.</strong></p>
<p><em>There is no way of using method 1 to change jukebox songs, as there are no actual cues for it in the unpacked game files (the files Jukebox_Cue are for the dwarf voice lines when you start the jukebox). You will notice that this is a recurring theme for some other sounds. You can use BP modding as there is a blueprint which does Jukebox stuff, but that's far too out of scope for this guide.</em></p>
<h3>Replacing axe impact sound</h3>
<p>The reason that this is a separate example from the one above, is that for some game elements like weapons and tools, one of several similar audio files is selected at random by the cue. This is likely to prevent the same cues from sounding unnaturally identical. To keep things simple, you can just use the same file for each.</p>
<p>So, for the files in <code>Content\Audio\SFX\WeaponsNTools\ITM_Grenades\AxeGrenade\AxeGrenadeImpactCombined</code>, there are 10 files.</p>
<p><img src="https://image.modcdn.io/members/80ea/7594990/profile/axegrenadeimpactcombined.png" alt="axegrenadeimpactcombined" width="589" height="452" /></p>
<p>So, when you replace them, you also need to duplicate your replacement file, and rename each of them to the names of the files you're replacing, in this case, <code>AxeGrenadeCombined_01.ogg</code>, <code>AxeGrenadeCombined_02.ogg</code>, etc. (<code>uasset</code> files are generated when the audio files are imported, and <code>uexp</code> files are generated upon packaging).</p>
<p>To test if UE4 is properly playing your audio files (sometimes issues will only be present when played in UEE/in-game but not in the original file), add a single file first, then play it. If crackling, popping, and/or other issues occur, try using the methods described <a href="#pre-requisites">here</a>.</p>
<p>Once you test your mod in-game (see <a href="#h_161987188221652147544375">Packing your mod</a>), you will probably need to adjust the volume or other settings. To adjust every file's settings at once, select the first file in UE, hold shift and select the last one, then right-click and select edit.</p>
<hr />
<h2>5. Packing your mod</h2>
<p>There are 2 ways to pack your mod:</p>
<ol>
<li>Using <code>DRGPacker</code>. If you already know how to use it and how it works, that&rsquo;s fine. But many of you reading this section have chosen to ignore the call to read the general guide and so tend to not understand how <code>DRGPacker</code> works at all. This has been causing a lot of problems for us so please, PLEASE, <strong><em>PLEASE</em></strong>, read the general guide explaining how to use <code>DRGPacker</code> before you read this section!</li>
<li>Using an &ldquo;experimental&rdquo; (despite being in the engine for years) UE auto-packing will pack your selected files automatically.</li>
</ol>
<h3>Using DRGPacker</h3>
<p>Now, you need to pack your mod. <strong>Before you skip this step, you need to pack your mod from UEE first, <em>then</em> in the DRGPacker.</strong> To pack your mod in UEE, first, you need to navigate to the editor tab (called Untitled) and click <code>File &gt; Package Project &gt; Packaging Settings</code>:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture21.png" alt="picture21" width="196" height="334" /></p>
<p>You need to <strong>uncheck</strong> the <code>Use Pak File</code> setting, so make sure that it is unchecked when you create every audio project.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture40.png" alt="picture40" width="625" height="294" /></p>
<p>Also, click on the little arrow also marked by the red box at the bottom of the above image. This will pull down a bunch of new settings. Scroll down to the bottom of these settings and find the section that says <code>Directories to never cook</code>. This setting is <strong>very important</strong>, as it allows you to select folders to not get packaged. <strong>It is absolutely imperetive that you make sure the following directory is set to never cook, otherwise your mod WILL cause unintentional behaviour with other sounds in the game:</strong></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture41.png" alt="picture41" width="625" height="55" /></p>
<p>You may add other folders if you are not using them for your current mod and they have assets inside of them (as you wouldn't want to replace other assets that are not included in your mod!).</p>
<p>Now go to <code>File &gt; Package Project &gt; Windows &gt; Windows (64-bit)</code>. Select the folder you wish to put the packed project into. Give it a couple of minutes, and it should be done!</p>
<p>Next, navigate to the packed project in your file explorer. Go to <code>WindowsNoEditor/FSD/</code> and copy the <code>Content</code> folder. Then, paste it into the input folder in your <code>DRGPacker</code>, and pack it. Because some of you twits somehow missed all of the warnings from me telling you to read the general guide on how to use DRGPacker, here's how you do it:</p>
<ol>
<li>Copy your <code>Content</code> folder into your <code>input</code> folder (the name of the folder is the name of the created pak file, which is why I suggest naming it with _P).</li>
<li>Drag and drop the input folder onto<code> _Repack.bat</code></li>
</ol>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture43.png" alt="picture43" width="272" height="191" /></p>
<h3>Using the alternate packing method</h3>
<p><em>Theoretically</em> it should be a lot easier and a lot quicker for you, however it seems that a lot of people find this confusing and so I moved it to the archived guide section because for some reason it seems to be more difficult than I thought. So I don't recommend using this over the above method. But if you're interested, I wrote a mini-guide on how to use this packing method <a href="https://github.com/DRG-Modding/Guides/blob/main/Archived/Alt%20BP%20Paking%20Guide/Alternate%20BP%20Mod%20Packaging%20Method.pdf" target="_blank" rel="noopener noreferrer">here</a>.</p>
<hr />
<h2>6. Installing your mod</h2>
<p>Since the mod.io integration, there are two ways of installing your mod for <strong>testing</strong>:</p>
<ol>
<li>Navigate to the <code>Paks</code> folder in your DRG install. Then place your <code>.pak</code> into it, <strong>making sure that the name of your <code>.pak</code> ends with <code>_P</code> otherwise the game will NOT load it</strong>. Inside the modding menu, the game will mark your mod as <code>DEPRECIATED</code> &ndash; this is fine, it will still load the mod although will force you into a sandbox save to test it. This is the best way to test your audio mods because it&rsquo;s quick and easy and doesn&rsquo;t require any hassle!</li>
<li>You can make hidden mods on mod.io which will be sandbox by default, but you can then add friends as &ldquo;moderators&rdquo; or &ldquo;testers&rdquo; in the mod settings so that they can subscribe to it and test it in-game with you.</li>
</ol>
<p>Once you are happy with your mod, you can upload a public version to mod.io! When you upload to mod.io, you need to send the .pak to .zip and upload that.</p>
<p><strong>This is VERY important: for audio mods, you MUST NOT select a requested category, as the game will automatically tag it as auto-verified if it detects that your mod only changes audio files! Then you can select the &ldquo;auto-verified&rdquo; type on the mod. This was implemented to save mods having to wait to be verified and to save the devs from spending so much time verifying mods.</strong></p>
<h3>Why won't my mod auto-verify in-game even though I've set the auto-verify tag?</h3>
<p>Everytime I've seen an audio mod not verify in-game, it's because the mod author hasn't followed the guide when it tells you to delete the dummied sound control files. If these files are not deleted from your pak file, your mod will not auto-verify in-game! Example assets:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/examplenonautoverifyfiles.png" alt="example non auto verify files" width="900" height="442" /></p>
<p><strong>How do I check exactly which files won't auto-verify?</strong></p>
<p>There is a handy tool called <code>mod_lint</code>, which you can see how to use in section <strong>2.3</strong> of the basic modding guide. As shown in the screenshot above, any files that say "no" means the mod will not auto-verify in-game.</p>
<hr />
<h2>7. Extra notes and help</h2>
<p>Here's some random info that may be useful for you when making audio mods.</p>
<h3>Why don't the game sound waves play anything in the all-assets branch of the audio template?</h3>
<p>This happens 95% of the time due to the sound waves being cooked assets imported into the editor. Since you aren't going to be cooking these assets, it doesn't cause any problems. They are purely in the project for reference, as are all the other assets.</p>
<p><strong>If you wish to listen to the original game sound waves, you can either: </strong></p>
<ul>
<li>Open the sound in FModel<strong><br /></strong></li>
<li>Listen to any voice line in-game using the <a href="https://mod.io/g/drg/m/shout-framework">shout framework</a> mod.</li>
</ul>
<p>Alternately, for more advanced users, you can reimport the cooked sound waves into the editor by closing UE, then copying in the cooked waves from the unpacked game files and overwriting the ones that are already there (you need both the .uasset and .uexp files). This should work for you for most circumstances, but only for that project.</p>
<h3>Why can't I hear anything when I play some cues in-editor using the audio template?</h3>
<p>Firstly, check that the sound wave asset itself plays. If it doesn't, check the section above this. If it does, continue reading this section.</p>
<p>When you play a sound cue in the editor, it will take into account any of the attached settings to the cue, such as sound class, submix, etc. Since most of the cues have these attached settings, and the settings control what the audio cue sounds like based on parameters in the game, many of them are by default set to volume 0 or not to play in the editor. There isn't really anything I can do to change this as some people are silly and miss the part about not packaging the sound control folder in their mods, so if they had the wrong settings they could very well mess up the audio for the whole game without realising it.</p>
<p>The solution, is to open the sound cue file, click on the sound wave node you want to play, and click "play node" at the top. If you have a big cue with multiple sound waves, modulators, delays, etc. that you need to test easily, then you can temporarily remove the sound control files from the cue.</p>
<h3>Windows 10 explorer audio file loading time</h3>
<p>With default settings in windows 10, audio files will slowly load the information about each one. To fix this, open <code>control panel &gt; indexing options &gt; advanced &gt; file types</code> and unckeck <code>ogg</code> and <code>wav</code>. If this doesn't work, uninstall <code>web media extensions</code>.</p>
<h3>Memorial Hall Cue (thanks Kraeus/KrysPolezoes)</h3>
<p>The cue for the <code>Memorial Hall</code> music isn&rsquo;t in the same place as others in the spacerig. It is called <code>St_Googbye_SpaceRig_Cue</code> in <code>Music/Levels</code>. This is how you should set up your attenuation so that it only plays in that room:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/memorial_hall_attenuation.png" alt="memorial hall attenuation" width="305" height="307" /></p>
<h3>What is single vs looping cues in Music_Wave?</h3>
<p>Single cues play once and then the queue moves it onto the next song, and looping plays the song until the wave has ended. In your looping cues, just duplicate the single cues but click on the wave player node and check <code>Looping</code>. The <code>Wave player</code> node should change to <code>Looping wave player</code> or similar.</p>
<h3>Looping cue but different song (thanks SASHA)</h3>
<p>If you're using a random node with <code>Looping wave players</code>, you might run into a minor issue where the same song plays on loop when the cue is playing for a long time. To fix this, you can put a <code>Looping</code> node between the <code>Random</code> and <code>Output</code> nodes, making sure that your <code>Wave players</code> are not set to <code>Looping</code> themselves.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profile/picture42.png" alt="picture42" width="496" height="276" /></p>
<h3>Getting help</h3>
<p>If you need any extra help, feel free to ask in the <code>#mod-chat</code> channel. Please do tag me (<code>Buckminsterfullerene#6666</code>) or one of the other audio mod authors, such as <code>Kraeus#9233</code>. Kraeus is especially good to ask for finding music cues, as he figured out where all of them are.</p>
<p>If you have had headaches when audio modding certain parts of the game, that require special attention, contact me so I can get it added to the guide.</p>
<h3>Kraeus' Music Breakdown</h3>
<p><span style="text-decoration: underline;">Boss</span></p>
<p><strong>Plays during:</strong></p>
<ol>
<li>Elimination Boss Battles</li>
</ol>
<p><strong>SoundClass for Cues:</strong> Music_Boss</p>
<p><strong>Files:</strong></p>
<ol>
<li>St_Boss_Music_Cue &rarr; St_Nova_Master_1 &rarr; Horrors of Hoxxes</li>
<li>St_InsterstellarNightmares_Cue &rarr; ST_InterstellarNightmares_Master_3 &rarr; Interstellar Nightmares</li>
<li>St_Stalker_Cue &rarr; St_Stalker_Master_1 &rarr; Fighting the Shadows</li>
</ol>
<p>&nbsp;</p>
<p><span style="text-decoration: underline;">End Wave</span></p>
<p><strong>Plays during:</strong></p>
<ol>
<li>Mining, Egg Hunt, Escort, and Elimination during the escape to the DropPod.</li>
<li>Refining &ndash; once 100% is reached.</li>
<li>Salvage &ndash; during both point defenses, 2nd point defense song continues until the DropPod leaves.</li>
<li>Extraction &ndash; during the wait for DropPod until the DropPod leaves.</li>
<li>ST_Action_Master_1 &ndash; Stat screen song is used by St_EndMission_Completed_Cue in Music_Menu Folder. OST Song is Beneath the Crust.</li>
</ol>
<p><strong>SoundClass for Cues:</strong> Music_Endwave</p>
<p><strong>Files:</strong></p>
<ol>
<li>St_Marching_Edited_A_2_Cue &rarr; St_Marching_Master_1 &rarr; Leave No Dwarf Behind</li>
<li>St_OperasFascination_Cue &rarr; ST_OperasFascination_Master_3 &rarr; Follow Molly</li>
<li>St_RobotGetAway_EditedA_01_Cue &rarr; St_RobotGetaway_Master_1 &rarr; Robot Getaway</li>
<li>St_SabotageOfMolly_Cue &rarr; ST_SabotageOfMolly_Master_3 &rarr; I Welcome the Darkness</li>
<li>St_SW_Edited_A_01_Cue &rarr; St_SW_Master_1 &rarr; March of the Brave</li>
<li>St_WhereTheyReallyDare_Cue &rarr; ST_WhereTheyReallyDare_Master_3 &rarr; The Last Ascent</li>
</ol>
<p><strong>Other Files:</strong></p>
<ol>
<li>St_Action_Edited_B_3 &ndash; Not sure if in use. Is the same as ST_Action_Master_1</li>
</ol>
<p>&nbsp;</p>
<p><span style="text-decoration: underline;">Level</span></p>
<p><strong>Plays during:</strong></p>
<ol>
<li>Ambient Level music during all missions (not SpaceRig)</li>
<li>LoadingScreenMusic_Cue is played while loading a mission.</li>
<li>St_Goodbye_SpaceRig_Cue is played in Memorial Hall only.</li>
</ol>
<p><strong>SoundClass for Cues:</strong> Music_Background except St_Goodbye_SpaceRig_Cue is Music_MemorialHall</p>
<p><strong>Files:</strong></p>
<ol>
<li>LoadingScreenMusic_Cue &rarr; St_Deep_Master_1 &rarr; The Descent</li>
<li>St_Alien_Cue &rarr; St_Alien_Master_1 &rarr; Fathomless Tomb</li>
<li>St_AxeRunner_Cue &rarr; ST_AxeRunner_Master_3 &rarr; Let's Go Deeper</li>
<li>St_Carp_Cue &rarr; St_Carp_Master_1 &rarr; Into the Abyss</li>
<li>St_Clutch_Cue &rarr; St_Clutch_Master_1 &rarr; Karl's End</li>
<li>St_Cold_Cue &rarr; ST_Cold_Master_3 &rarr; Absolute Zero</li>
<li>St_Crawl_Cue &rarr; St_Crawl_Master_1 &rarr; Coward's Crossing</li>
<li>St_Deep_Cue &rarr; St_Deep_Master_1 &rarr; The Descent</li>
<li>St_Goodbye_SpaceRig_Cue &rarr; St_Goodbye_Master_1 &rarr; Ode to the Fallen</li>
<li>St_Horror_Cue &rarr; St_Horror_Master_1 &rarr; A Matter of Skill and Ammunition</li>
<li>St_JourneyOfTheProspector_Cue &rarr; A_Quiet_Tomb_ST_Master_1 &rarr; Journey Of The Prospector</li>
<li>St_LOTD_Cue &ndash;&gt; ST_LOTD_Master_3 &rarr; Echoes from the Past</li>
<li>St_Pod_Cue &rarr; St_Pod_Master_1 &rarr; Principle of Darkness</li>
<li>St_Slow_Cue &rarr; St_Slow_Master_1 &rarr; I am Lost</li>
<li>St_ST_Cue &rarr; St_ST_Master_1 &rarr; The Only Way Out is Through</li>
<li>St_ValleyOfDeath_Cue &rarr; ST_ValleyOfDeath_Master_3 &rarr; Deceived by Light</li>
</ol>
<p>&nbsp;</p>
<p><span style="text-decoration: underline;">Menu</span></p>
<p><strong>Plays during:</strong></p>
<ol>
<li>St_DeepDives_InbetweenScreen_Cue plays between Deep Dive missions.</li>
<li>St_EndMission_Completed_Cue plays during the Stat Screen after a mission.</li>
</ol>
<p><strong>SoundClass for Cues:</strong> Music_Menu</p>
<p><strong>Files:</strong></p>
<ol>
<li>St_DeepDives_InbetweenScreen_Cue &rarr; ST_Where_They_Really_DareDLoop_1 &rarr; The Last Ascent (Excerpt)</li>
<li>St_EndMission_Completed_Cue &rarr; ST_Action_Master_1 (From AudioFiles in the Level folder) &rarr; Beneath the Crust</li>
</ol>
<p><strong>Other Files:</strong></p>
<ol>
<li>DeepDives_InbetweenScreen_Music - Not sure if in use. Is the same as ST_Where_They_Really_DareDLoop_1</li>
</ol>
<p>&nbsp;</p>
<p><span style="text-decoration: underline;">Special Events</span></p>
<p><strong>Plays during:</strong></p>
<ol>
<li>ST_GameEventA_Cue plays during Machine Events</li>
<li>DiscoverMusic_1 is the music played when treasure (Crate or Pack) is found.</li>
</ol>
<p><strong>SoundClass for Cues:</strong> Music_Action and Music_Discovery for the DiscoverMusic_1.</p>
<p><strong>Files:</strong></p>
<ol>
<li>ST_GameEventA_Cue &rarr; ST_GameEvent_Master_1 &rarr; The Core Infuser</li>
<li>DiscoverMusic_1 is the music played when treasure (Crate or Pack) is found</li>
</ol>
<p><strong>Other Files:</strong></p>
<ol>
<li>ST_GameEventA_4 - Not sure if in use. Is the same as ST_GameEvent_Master_1</li>
</ol>
<p>&nbsp;</p>
<p><span style="text-decoration: underline;">Wave</span></p>
<p><strong>Plays during:</strong></p>
<ol>
<li>CueSingle plays during Mission Control announced waves</li>
<li>CueLooping plays in Refinery during the pumping stage and in Escort during Ommoran when the wave music doesn't stop</li>
</ol>
<p><strong>SoundClass for Cues:</strong> Music_Action</p>
<p><strong>Files:</strong></p>
<p><strong>Single:</strong></p>
<ol>
<li>St_Boss_wave_Cue &rarr; St_Boss_Master_1 &rarr; They're Here!</li>
<li>St_DOTSA_Cue &rarr; ST_DOTSA_Master_3 &rarr; Dance of The Dreadnaughts</li>
<li>St_HoldMyBeard_Cue &rarr; St_HoldMyBeard_Master_1 &rarr; Hold My Beard</li>
<li>St_Hole_Cue &rarr; St_Hole_Master_1 &rarr; The Shadows are Moving</li>
<li>St_MorkiteIsADancer_Cue &rarr; ST_MorkiteIsADancer_Master_3 &rarr; Axes Out</li>
<li>St_MountainBlaster_Cue &rarr; ST_MountainBlaster_Master_3 &rarr; In the Belly of the Beast</li>
<li>St_NotTheBees_Cue &rarr; ST_NotTheBees_Master_3 &rarr; A Distant Terror</li>
<li>St_SpaceFire_Cue &rarr; ST_SpaceFire_Master_3 &rarr; RUN!</li>
<li>St_Tick_Cue &rarr; St_Tick_Master_1 &rarr; Petrified Fury</li>
<li>St_Wave_Cue &rarr; St_Wave_Master_1 &rarr; Attack of the Glyphids</li>
</ol>
<p><strong>Looping:</strong></p>
<ol>
<li>A_Distant_Terror_Looping_Cue &rarr; A_Distant_Terror_Looping_01 &rarr; A Distant Terror</li>
<li>Dance_Of_The_Dreadnaught_Looping_Cue &rarr; Dance_Of_The_Dreadnaught_Looping_01 &rarr; Dance of the Dreadnaught</li>
<li>MorkiteIsADancer_Looping_1_Cue &rarr; MorkiteIsADancer_Looping_1 &rarr; Axes Out</li>
<li>MountainBlaster_Looping_1_Cue &rarr; MountainBlaster_Looping_1 &rarr; In the Belly of the Beast</li>
<li>PetrifiedFury_Looping_Cue &rarr; PetrifiedFury_Looping_1 &rarr; Petrified Fury</li>
<li>SpaceFire_Looping_1_Cue &rarr; SpaceFire_Looping_1 &rarr; RUN!</li>
<li>Theyre_Here_Looping_Cue &rarr; Theyre_Here_Looping_1 &rarr; They're Here!</li>
</ol>
<p>&nbsp;</p>
<p><span style="text-decoration: underline;">Spacerig</span></p>
<p><strong>Plays during:</strong></p>
<ol>
<li>Ambience_Music_Cue is the Ambient SpaceRig Music</li>
<li>Ambience_Music_Chrsitmas_Cue plays Ambient SpaceRig music during the Christmas Event</li>
<li>Ambience_Music_DiscoBeer_Cue plays when you drink a Blackreach Blonde</li>
<li>Ambience_Music_DiscoBeer_Safe_Cue plays when you drink a Blackreach Blonde with Streamer Mode enabled</li>
<li>Fanfare_promotion_Cue plays in Memorial Hall</li>
<li>YearTwoFanfare_Cue played during the Year Two reward screen so isn't used anymore (I think)</li>
</ol>
<p><strong>SoundClass for Cues:</strong> Music_Action</p>
<ol>
<li>Ambience_Music_Cue and Ambience_Music_Chrsitmas_Cue &ndash; Music_Background</li>
<li>Ambience_Music_DiscoBeer_Cue and Ambience_Music_DiscoBeer_Safe_Cue - Music_BeerEffect</li>
<li>Fanfare_promotion_Cue &ndash; Music_PromotionMenu</li>
</ol>
<p><strong>Files:</strong></p>
<ol>
<li>Ambience_Music_Cue &rarr; St_Ambience_Master_1 &rarr; The Deep Dive</li>
<li>Ambience_Music_Chrsitmas_Cue &rarr; Christmas Song 4,5,6,8, and 9</li>
<li>Ambience_Music_DiscoBeer_Cue &rarr; JukeBox_Disco_Night_Disco, Jukebox_Techno_di-the-chance-032414-81, and Techno_TRJ_02 in Jukebox</li>
<li>Ambience_Music_DiscoBeer_Safe_Cue &rarr; Techno_TRJ_02 in Jukebox</li>
<li>Fanfare_promotion_Cue &rarr; PromotionFanfare &rarr; Might be an excerpt from OST song</li>
</ol>
<p><strong>Other Files:</strong></p>
<ol>
<li>Fanfare 3 and 4 &ndash; probably used for Year Celebration reward screens</li>
</ol>
