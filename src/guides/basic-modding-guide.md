<h2>DRG Mods: An Introduction to Modding</h2>
<blockquote>
<p>Please do not hesitate to ask for help on the <a href="https://discord.gg/gUw32ayWGt">DRG Modding Discord</a> in #mod-questions!</p>
<p><strong>Credits:<br /></strong>Rauliken - Originally wrote guide.<br />Jen Walter - Original guide contributor.<br />Pacagma - Original guide contributor.<br />NaturalBornCamper - Original guide contributor.<br />Akira Fudo - Original guide contributor.<br />Buckminsterfullerene - Maintains guide and moved to mod.io.<br />Fancyneer - Update with common issues and new tools.</p>
</blockquote>
<hr />
<h2>Contents</h2>
<ul>
<li>1. Introduction and tools
<ul>
<li>1.1 The basic knowledge</li>
<li>1.2 The basic tools</li>
</ul>
</li>
<li>2. Things to learn before modding
<ul>
<li>2.1 Unpacked the game's files</li>
<li>2.2 Checking the content of the files</li>
<li>2.3 Using mod_lint to check if your mod will auto-verify</li>
<li>2.4 Packing your mod files
<ul>
<li>2.4.1 Preparing your project for packaging&nbsp;</li>
<li>2.4.2 File packing method</li>
<li>2.4.3 Mod installation and uploading</li>
</ul>
</li>
</ul>
</li>
<li>3. How to mod
<ul>
<li>3.1 Hex mods</li>
<li>3.2 Other mods</li>
</ul>
</li>
<li>4. Common issues and how to solve them
<ul>
<li>4.1 My mod isn't working/vanilla files are still playing or present</li>
<li>4.2 My mod is crashing when I go to test it</li>
</ul>
</li>
<li>5. Conclusion</li>
</ul>
<hr />
<h2>1. Introduction and tools</h2>
<h3>1.1 The basic knowledge</h3>
<p>There&rsquo;s still a lot of things to do in DRG even if you have all the cosmetics and promoted all characters to the max level. Welcome to DRG modding, you are going to like this game even more after learning how to mod it. If you find it too difficult just try the current mods that the modders have released and have some fun. I&rsquo;m at 1k hours myself and I would have stopped playing if it wasn&rsquo;t for the amazing mods.</p>
<p><strong>Some VERY IMPORTANT THINGS before starting:</strong></p>
<ol>
<li>Make sure you&rsquo;ve read the official info about DRG&rsquo;s mod support in the links below. I&rsquo;m not going to go into details here so make sure you read them to understand the basics. The main post <a href="https://steamcommunity.com/games/DeepRockGalactic/announcements/detail/2953787944888179529">here</a>. The FAQ with some additional info <a href="https://www.deeprockgalactic.com/modding-support-faq">here</a>. You can also check the 3 guides created by GSG in the mod.io guides section.</li>
<li>Mods are only hosted here on mod.io. Use the modding menu inside the game to create an account automatically so you can upload your mods.</li>
<li>Your vanilla or modded save files are safe because mods will not corrupt/mess with it unless you use the tools maliciously. If you are concerned about this use the save clone and backup feature that the game provides in the menu.</li>
<li>This guide only covers the bare minimum so if you want to learn more make sure you check all the other guides in the <code>#learn-guides</code> channel in the <a href="https://discord.gg/gUw32ayWGt">DRG Modding Discord</a>.</li>
</ol>
<p>I highly recommend reading sections 1. and 2. from top to bottom if it&rsquo;s your first time.</p>
<h3>1.2 Basic tools</h3>
<p>First you are gonna need some tools to start modding. You can find all of the tools explained here on <a href="https://drg-modding.github.io/tools/" target="_blank" rel="noopener noreferrer">this lovely site. </a>This is an overview of the basic tools, and then in the next section I will show you how to use them.</p>
<p><strong>1. FSD-WindowsNoEditor.pak </strong></p>
<p>This is the main file of the game, which contains all the rest of the files. It&rsquo;s &ldquo;paked&rdquo; with Unreal Engine so we need to extract the files if we want to modify them.</p>
<p>You can find it in <code>&hellip;\Program Files (x86)\Steam\steamapps\common\Deep Rock Galactic\FSD\Content\Paks</code>. You obviously need the game installed and NEVER DELETE THIS FILE, just copy it if you need it later. If you don&rsquo;t know how to access this folder go to <code>Steam library -&gt; right click DRG &gt; Properties &gt; Local Files &gt; Browse Local Files</code>. If you want to make a lot of mods it is recommended to make a dedicated space for the extracted files inside it when we get to that point.</p>
<p><strong>2. DRG Packer</strong></p>
<p>This tool will allow us to extract the files from <code>FSD-WindowsNoEditor.pak</code> and will also let us pack our own files. The files that you extract will have the extensions <code>.uexp</code> and <code>.uasset</code> which are Unreal Engine files. More on this on <code>2.1 Unpacking the game&rsquo;s files</code>.</p>
<p><strong>For older users of the guide:</strong> <em>you don&rsquo;t need UEE anymore with the new version of the packer if you just want to make hex mods. </em></p>
<p><strong>3. UAssetGUI</strong></p>
<p>This is used to see the contents of the files and change values. You are going to need it if you want to make mods that change a value/s inside the game files. There will be an example in <code>3.1 Hex mods</code>.&nbsp; If the tools don&rsquo;t work then you may need to download the <a href="https://dotnet.microsoft.com/download">.NET Runtime</a>. You can find the Github repository for this open-source tool <a href="https://github.com/atenfyr/UAssetGUI/">here</a>.</p>
<p><strong>4. EmptyContentHierarchy</strong></p>
<p>These are all the folders inside an extracted <code>FSD-WindowsNoEditor.pak</code> but empty, just the folders. This will be useful later to pack files and for UEE in other guides. Each major update will have a different one, so make sure you get the one from the most recent version of the DRG Modding tools <a href="https://drg-modding.github.io/tools/" target="_blank" rel="noopener noreferrer">website</a>.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/ech_1.png" alt="ech 1" /></p>
<p><strong>5. UModel</strong></p>
<p>This tool will allow you to extract the audio files, textures, 3D models etc. This is different from DRG Packer because that just extracts the <em>cooked </em><code>.uexp</code> and <code>.uasset</code> files whereas if you want to dive deeper and get the raw original sounds, textures, etc. you are going to need this. You can find it in the modding tools website as well as at the <a href="https://www.gildor.org/en/projects/umodel">official website</a>.</p>
<p><strong>6. FModel</strong></p>
<p>This tool will give you "readable" files (JSON) from the <code>.uexp</code> and <code>.uasset</code> pair files. UAssetGUI already parses the files but they may have problems and in my opinion it&rsquo;s easier to search for that value you want to change in FModel before you edit it. It can also extract raw sounds but not as well or as easily as with UModel. Again, you can find this at their official <a href="https://fmodel.app">website</a>.</p>
<p><strong>7. Unreal Engine Editor (UEE)</strong></p>
<p>If you want to make mods to replace audio and textures you need this. You can also create your own Unreal Engine blueprints (which are way more complex than hex editing mods or audio mods but can do more stuff). You can find it <a href="https://www.unrealengine.com/en-US/">here</a>. Unfortunately you need to download the Epic Games Store even though we just want Unreal Engine and not the games but if you want to avoid downloading the store, or use Linux, you will need to build from source, which you can google how to do.</p>
<p><strong>You are going to need to download the version that DRG is made with, which is version 4.27.2.</strong></p>
<p>Go to the Library, add a new version with the + symbol -&gt; click the arrow next to install -&gt; and the version. You only need the <strong>basic installation</strong> so you can configure it and remove everything but the basic stuff so the installation takes only like <strong>13 GB</strong> of storage instead of 30+ GB.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/ue.png" alt="ue" width="375" height="214" /></p>
<p><strong>8. Other tools</strong></p>
<p>Those are the main tools you need but there&rsquo;s way more in the tools channel in the modding discord (tools for specific types of mods, console unlocker, research tools etc).</p>
<hr />
<h2>2. Things to learn before modding</h2>
<h3>2.1 Unpacking the game's files</h3>
<p>First you are going to need to download DRG Packer from our <a href="https://drg-modding.github.io/tools/basic-tools.html" target="_blank" rel="noopener noreferrer">modding tools site</a>. This is our fresh DRG Packer folder after extraction:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/drgpacker_1.png" alt="drgpacker 1" width="272" height="196" /></p>
<p>Now we want to extract all the files from <code>FSD-WindowsNoEditor.pak</code>. Simply copy drag and drop <code>FSD-WindowsNoEditor.pak</code> into <code>_Unpack.bat</code>.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/drgpacker_2.png" alt="drgpacker 2" width="543" height="240" /></p>
<p>Now wait for the extraction to complete. The command line window will say &ldquo;Press any key to continue&hellip;&rdquo; when it&rsquo;s done you will see a new folder called <code>FSDWindowsNoEditor</code>. The files of the game are inside it.</p>
<p>We are not going to use the contents of the <code>Engine</code> subfolder and the other folders<br />inside <code>FSD</code> that are not <code>Content</code>.</p>
<p><strong>You need to re-extract the files again for each update the game receives.</strong></p>
<h3>2.2 Checking the content of the files</h3>
<p>If you go to the <code>FSD/Content</code> folder, you will find the extracted files. All of these contain the pairs of <code>.uexp</code> and <code>.uasset</code> files that we are going to need.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/drgpacker_3.png" alt="drgpacker 3" width="546" height="418" /></p>
<p>For example, if we go to <code>FSD\Content\WeaponsNTools\FlameThrower</code> we can see all the files for the flamethrower. For editing values like upgrades or base stats we can open some of the files using a software like <code>FModel</code> or <code>UAssetGUI</code> to have more readable text.</p>
<p>We are going to be working on this file later in <code>3.1 Hex mods</code> to learn how to search for and change the values inside. If you want to extract other things like sounds or textures you will need to use UModel and extract the desired file/s. For example, we open it to get one of Molly&rsquo;s sound files.</p>
<p>Select the <code>FSD-WindowsNoEditor\FSD\Content</code> folder, select these settings and press OK:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/umodel_1.png" alt="umodel 1" /></p>
<p>We navigate the folder tree until we find our file or a folder with all the files we need (you can extract the whole Audio folder for example if you want all the sounds in the game), and after selecting extract we select an output folder:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/umodel_2.png" alt="umodel 2" width="626" height="300" /></p>
<p>If it&rsquo;s a sound, 3D model, texture file etc you will get a few files in the extraction folder.</p>
<h3>2.3&nbsp;Using mod_lint to check if your mod will auto-verify</h3>
<p>Mod_lint is an invaluable tool developed by AssemblyStorm. which can be used to check if your mod will pass the mod.io verification check. You can download it from <a href="https://github.com/trumank/drg-mod-tools/releases">here</a>. To use it, simply open up Windows Powershell/command prompt, drag <code>mod_lint.exe</code> into it, press space, then drag your packed mod&rsquo;s zip file into it as well, and press enter. The tool will now go through your mod and give you a list of all the assets you&rsquo;re replacing and if they&rsquo;re eligible for auto-verification.</p>
<p><iframe src="https://www.youtube.com/embed/kLB0RwFOX80" width="560" height="314"></iframe></p>
<p>A single <code>No</code> flag in your file will make it ineligible for auto-verification, and you have to either remove those files if you don&rsquo;t need them, or wait for manual verification if said files are required for your mod.</p>
<h3>2.4 Packing your mod files</h3>
<p><strong>2.4.1 Preparing your project for packaging</strong></p>
<p>First and foremost, before we pack our mod files, we need to tweak a few options in the <code>Packaging Settings</code>. Go to <code>File -&gt; Package Project</code> and click <code>Packaging Settings</code>.</p>
<p>Untick <code>Use Pak File</code>.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/screenshot_2023-05-30_210840.png" alt="" width="251" height="155" /></p>
<p>Use the search bar at the top and type &ldquo;shader&rdquo; and then untick <code>Share Material Shader Code</code></p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/screenshot_2023-05-30_201002.png" alt="" width="257" height="171" /></p>
<p>Use the search bar again and type &ldquo;cook&rdquo;. This will take you to the packaging settings and in it you will see a field called &ldquo;Directories to never cook&rdquo;. This is your project&rsquo;s directory blacklist. Configure it in a way that is appropriate for your mod. For example with sound mods, you will want to put <code>SoundControl</code> in this list.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/screenshot_2023-05-30_2108402.png" alt="" width="442" height="193" /></p>
<p>As a side note, if you are using the <code>FSD-Template</code> project, you will need to clear out the blacklist of any entries you don&rsquo;t need, as it is set to blacklist everything by default.</p>
<p><strong>2.4.2 File packing method</strong></p>
<p>I&rsquo;m going to use the example from the flamethrower file before (imagine we already edited it even though that comes later in the tutorial). You should probably read about <code>Empty Content Hierarchy</code> if you haven&rsquo;t already.</p>
<p>In the flamethrower example, the file is <code>UPG_FlameThrower_A_RangeIncrease</code> which is inside <code>FSD\Content\WeaponsNTools\FlameThrower</code>. To pack mods correctly we are going to need to replicate the same folder structure that we got on the output folder (unpacked <code>FSD-WindowsNoEditor.pak</code>) but inside the <code>Input</code> folder.</p>
<p>You can just open the .zip from <code>Empty Content Hierarchy</code> (which has all the folders from the game but empty) and drop the <code>Content</code> folder in it inside <code>Input</code> and replace the placeholder one there:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/ech_2.png" alt="ech 2" width="454" height="209" /></p>
<p>In this case we just want to work with 1 weapon so you don&rsquo;t need to have the rest of the folders. You can just drop the <code>WeaponsNTools</code> folder inside <code>Input -&gt; Content</code>.</p>
<p>You can also delete everything but the <code>FlameThrower</code> folder too. I recommend having a backup of each folder structure for each mod you make, for easier packing, instead of just having all of the empty folders and having to search where you put the files.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/ech_3.png" alt="ech 3" width="455" height="335" /></p>
<p>This will be the final path of the flamethrower files:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/2.3_1.png" alt="2 3 1" width="458" height="132" /></p>
<p>You can do the process before for any number of files that you modded.</p>
<p>Be careful when leaving folders in <code>Input -&gt; Content</code> after packing because maybe you&rsquo;ll start making a new mod and forget you still had the old mod files in there.</p>
<p>Now just drag and drop the Input folder into <code>_Repack.bat</code>. (FSD here is just the extracted files from before, I recommend renaming them to the update where they are from).</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/drgpacker_4.png" alt="drgpacker 4" width="373" height="204" /></p>
<p>Check the &ldquo;Added x files&rdquo; line. If you have for example 2 files in the mod check that the number is correct so it says &ldquo;Added 2 files&rdquo;. If there&rsquo;s less than what you expect you forgot to put some files in the input folder, if it&rsquo;s more than expected then you forgot to remove files from a previously packed mod.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/drgpacker_5.png" alt="drgpacker 5" width="712" height="230" /></p>
<p>So, the summary of packing is that we only need to add or remove files inside the <code>Empty Content Hierarchy</code> (FSD and subfolders) with the correct folder structure compared to the vanilla files. It can get pretty messy if you forget to add/remove files when making new mods so (again) I recommend having a backup of each folder structure for each mod you make, for better organization and future updating.</p>
<p><strong>2.4.3 Mod installation and uploading</strong></p>
<p>To install a mod <strong>for your own testing purposes before uploading</strong>, simply move the .pak than you made in the previous step to: <code>&hellip;\Steam\steamapps\common\Deep Rock Galactic\FSD\Content\Paks</code>. You <strong>MUST</strong> put <code>_P</code> at the end of the <code>.pak</code> name in order for the game to load it.</p>
<p>Unreal Engine automatically replaces the original file(s) in <code>FSD-WindowsNoEditor.pak</code> when the game loads, with the file(s) that you modified in your mod.</p>
<p>In the in-game modding menu, your mod will show up as "depreciated" and will force you into a sandbox save.</p>
<p>To <strong>upload your mod to mod.io</strong> after testing your modname_P.pak, first read the following guidelines if you have not already done so:</p>
<ul>
<li><a href="https://mod.io/g/drg/r/mod-guidelines-and-status-categories">Mod categories</a></li>
<li><a href="https://mod.io/g/drg/r/approval-process-and-checklist-for-upload">And this other one to learn what you need to do before uploading a mod</a></li>
</ul>
<p>After that click on the + button on drg.mod.io and follow the simple steps.</p>
<hr />
<h2>3. How to mod</h2>
<p>There&rsquo;s different types of mods that you can make for DRG:</p>
<ol>
<li>Hex - can be used to edit values and are extremely easy to make</li>
<li>Audio - is also easy but takes a bit more time as well as Unreal Engine Editor</li>
<li>Model/texture - can be fiddly to make and ideally requires knowledge in the field before starting but leads to very appealing mods</li>
<li>Blueprint - uses Unreal Engine blueprint to create complex behaviour as it has full logical capabilities and can interact with the game's underlying code</li>
</ol>
<p>In this guide I&rsquo;ll be explaining the basic hex mods. At the end I&rsquo;ll mention a few of the other guides to get you started with more complex mods. As always you can find everything in the <em>Tutorials</em> channel category in our modding discord.</p>
<h3>3.1 Hex mods</h3>
<p>We have the hex mods, which edit values inside the .uasset and <code>.uexp</code> files (all files are in pairs of these types) and replace the original files with your new ones when the game loads. These values can be integers, text, booleans, floats etc. Most of the values should be easy to find just by using the armory inside DRG but you need to keep a few things in mind.</p>
<p>We need to know the value that we are going to change but also the file where it is located. You can check the <code>File Prefix List</code> made by Elythnwaen in the <code>#learn-tools</code> channel.</p>
<p>I&rsquo;m gonna be working with the file that I used in the packing example before, in <code>2.3.1 File packing</code> method. In this case our mod is going to have only one file which is called <code>UPG_FlameThrower_A_RangeIncrease.uexp</code> and as you can tell by the prefix <code>UPG</code> this is the file that controls the range upgrade for driller&rsquo;s flamethrower.</p>
<p>I&rsquo;m going to be using <code>UAssetGUI</code>. Open the <code>.uasset</code> and explore a bit. To change a value go one of the categories:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/uagui_1.png" alt="uagui 1" width="779" height="240" /></p>
<p>In the <code>Amount</code> field is the value we are looking for. It&rsquo;s <code>500</code> because the upgrade of <code>+5</code> in game is measured in meters and it's centimeters in the files. Some values can be more obvious than this. We&rsquo;ll change it to <code>2000</code> so it becomes a <code>20</code> meter range upgrade.</p>
<p>If you can&rsquo;t find the value try using <code>FModel</code>. Open the main <code>.pak</code> from the game, navigate to the folder, go to the <code>Assets</code> tab on the top right and open the file. Use <code>CTRL+F</code> to search stuff. This is pretty useful in other files that have a lot of stuff but isn&rsquo;t really needed for this small change.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/fmodel_2.png" alt="fmodel 2" width="782" height="323" /></p>
<p>Finally, we just save the file in <code>UAssetGUI</code> with <code>CTRL+S</code> or <code>File -&gt; Save</code> and we are ready to pack our mod. You will get 2 new files and the tools might make a backup (.bak) of the original ones.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/uagui_2.png" alt="uagui 2" width="462" height="137" /></p>
<p>You should know how to pack that those file pairs with one of the methods that I showed you earlier in <code>2.3 Packing your mod files</code>. I used these exact files in both examples to make it easier to understand the whole process.</p>
<p>After packing, just follow <code>2.3.2 Mod installation</code> and you are ready to test your new flamethrower upgrade that will add <code>+20</code> meters instead of <code>+5</code>. As you can see in the armory when I hover over the range upgrade it now gives us <code>20</code> extra range. You can try the range overclock too for some insane flame reach :P</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/demo_1.png" alt="demo 1" width="599" height="484" /></p>
<p>Here&rsquo;s a quick guide of all the steps for hex mods:</p>
<ol>
<li>Extract <code>.pak</code> files with DRG Packer by dragging the <code>.pak</code> into <code>_Unpack.bat</code>.</li>
<li>(Optional) Read the files with <code>FModel</code> if you want more info about the value you want to change.</li>
<li>Open the <code>.uasset</code> file you want to change with a <code>UAssetGUI</code> and edit the values you want.</li>
<li>Now to turn it back into <code>.pak</code> again follow these steps: (an example for the barrel kick force mod that modifies only <code>PST_BarrelKicking.uexp</code>):
<ol>
<li>Create the same folder structure inside <code>DRGpacker -&gt; Input -&gt; Content</code> as the original extracted files had and add the modified files, so: <code>/GameElements/PawnStats/PST_BarrelKicking.uexp</code></li>
<li>Drag and drop the <code>Input</code> folder to <code>_Repack.bat</code> and check that there are no errors.</li>
</ol>
</li>
<li>Put the generated <code>new_P.pak</code> file in to test it: <code>&hellip;\Steam\steamapps\common\DeepRockGalactic\FSD\Content\Paks</code></li>
<li>After testing it to make sure it works, rename it to <code>InsertNameHere.pak</code>, as you don&rsquo;t need the <code>_P</code> anymore.</li>
<li>Upload to mod.io after reading the guidelines. Write proper descriptions and choose the correct category. You should have no issue if you read the info links.</li>
</ol>
<p>And there&rsquo;s a few important things about mods:</p>
<ol>
<li>If your game crashes, <strong>DO NOT send the crash report to the devs</strong>. You probably changed the wrong value or changed file size.</li>
<li>Most mods are client-side only (as opposed to mods that have the RequiredByAll tag on mod.io are meant to have server replication). Client side only means that installing these mods only effects stuff for your game, but not for other players in the lobby.</li>
<li>As I explained in the packing tutorial, you can have more than 1 file in a mod. You don&rsquo;t need to make 10 .pak files if you modify 10 files, just make a single one by adding more files with the correct paths to the <code>Input -&gt; Content</code> folder.</li>
<li>You can change a lot of things with hex editing, not only the weapon&rsquo;s stats. If for example you change a perk, you will need to try the changes inside a mission to make sure they work because those don&rsquo;t show up in the armory.</li>
<li>This type of modding is the most basic but It will complement the others. For complex "hex style" mods like adding overclocks, changing character inventories&hellip; you should use blueprint modding with the reconstructed content to make it all very easy for you to edit.</li>
</ol>
<h3>3.2 Other mods</h3>
<p>Now that you know the basics you are free to check all the other guides and tools.</p>
<p>For audio and blueprint mods the user <code>Buckminsterfullerene#6666</code> has made some great ones.</p>
<p>For model and texture replacement <code>Pacagma#1515</code> is your guy.</p>
<p><code> Fancyneer#1553</code> is also someone who is keen to help out with a wide array of mods such as audio, model replacement, and animation.</p>
<p>Make sure to checkout the <a href="https://drg-modding.github.io/tools" target="_blank" rel="noopener noreferrer">modding tools</a> site to find other tools.</p>
<hr />
<h2>4. Common issues and how to solve them</h2>
<h3>4.1 My mod isn't working/vanilla files are still playing or present</h3>
<p>This issue is caused by your content folder hierarchy being incorrect and/or your file names not matching with the files you want to replace. Go back to your mod content folders and make absolutely sure the folder structure and names match. GSG&rsquo;s naming conventions are a bit weird sometimes so you have to be very vigilant about this. Being just one letter off will make your mod not work. So double, triple, even quadruple check the names and hierarchy so that they match.</p>
<p>As an example; here I am replacing the Glyphid death scream sound cue, but my replacement isn&rsquo;t working.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/grunt_death_scream2.png" alt="" width="401" height="109" /></p>
<p>That is because my folder hierarchy isn&rsquo;t correct. Here is the same file being replaced but this time it&rsquo;s using the correct hierarchy:</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/grunt_death_scream1.png" alt="" width="393" height="114" /></p>
<p>Notice any difference? The one which isn&rsquo;t working has a hierarchy of <code>Content/Audio/Enemies/SpiderGrunt/DeathScream</code>. However the correct hierarchy was actually <code>Content/Audio/SFX/Enemies/SpiderGrunt/DeathScream</code>. There needed to be an <code>SFX</code> folder after <code>Audio</code> and before <code>Enemies</code> in order for the replacement to work.</p>
<h3>4.2 My mod is crashing when I go to test it</h3>
<p>This largely depends on what kind of mod you&rsquo;re making. With Blueprint mods it can be anything from null references to bad logic and those are often project specific things to troubleshoot. If it&rsquo;s a simple asset replacement mod, then go to your packaging settings and make sure <code>Share Material Shader Code</code> is disabled.</p>
<p><img src="https://image.modcdn.io/members/6da1/6436408/profilegeneral/screenshot_2023-05-30_201002.png" alt="" width="257" height="171" /></p>
<p>If the problem still persists, and you&rsquo;re creating a model replacer mod, you&rsquo;ll have to go and find a common pattern to the crashing and work from there. Here are two common crashes and their causes;</p>
<p>If you&rsquo;re replacing the model for the minigun and your game crashes when you start firing, this means you haven&rsquo;t assigned the heat material to any part of your model. You will need to assign every base material somewhere in your model to avoid crashes. So go look at the material setup of the model you are trying to replace and see how many material slots it already has. Then assign all of them somewhere on your model.</p>
<p>If you&rsquo;re replacing an enemy model and the game crashes when it despawns, this is because your model&rsquo;s material lacks the dissolve logic the base glyphid material has. You will need to copy that material over to your own project and then edit it to fit your own model.</p>
<hr />
<h2>5. Conclusion</h2>
<p>I hope you found the guide simple enough to understand and that you make many great mods. It&rsquo;s been a long time, we started this community with 2-3 modders and a small private discord and now we have members in the thousands, the developers added official mod support and with this guide we have a new modder to welcome :) - Rauliken (Original author)</p>
<p>If you ever feel frustrated when making mods don&rsquo;t forget that there&rsquo;s always someone willing to help, it&rsquo;s a very wholesome community. You can find a ton of info in the <code>#mod-chat</code> channel in our discord and there&rsquo;s even a channel where people stream themselves making mods so you can all learn together.</p>
<p>If you ever want to check some old mods for inspiration, extracting the files to see which ones they changed etc, this is the <a href="https://github.com/ArcticEcho/DRG-Mods">Github repository</a> where the old Update 33 mods were before the modding support update.</p>
<p>See you in discord and in the mines, ROCK AND STONE!</p>