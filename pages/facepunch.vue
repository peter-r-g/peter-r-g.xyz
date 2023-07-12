<template>
  <main>
    <div class="main-container">
      <div class="header">
        <h1 class="title">Hello FP Team</h1>
        <h3 class="subtitle">Welcome to my portfolio in the style of a blog</h3>
      </div>

      <div id="overview" class="blog-entry">
        <a href="#overview" class="title">My time in the Dev Preview</a>
        <p>
          I have been in the developer preview for just over two years now.
          I came in during the nomination program on May 28th, 2021.
          Throughout this time, I have been known in the community as "gunman435," "Gooman," and "peter-r-g."
          I have contributed to several games, libraries, and tools.
          I have also opened over fifty issues and notified Garry of four security concerns/exploits.
        </p>
      </div>

      <div id="mocha" class="blog-entry">
        <a href="#mocha" class="title">Mocha</a>
        <h4>Made by Alex Guthrie</h4>
        <div></div>
        <img class="icon-img" src="projects/Mocha Logo.webp"/>
        <p>
          Mocha is a C++ engine with a C# interop created by Alex Guthrie.
          It uses a Vulkan renderer, html/css UI, Jolt physics, PBR, an ImGui editor, and a C# scripting environment.
          Some of my contributions to the project were: Improving the various tools, re-writing the C# compile process, and adding runtime NuGet support.
        </p>
      </div>

      <div id="mochacompile" class="blog-entry">
        <a href="#mochacompile" class="title">Mocha - Compile Process</a>
        <div></div>
        <p>
          In Mocha, we do not have to worry about an access list.
          The engine is for people to create games and re-distribute them akin to Unity, UE5, etc.
          This allows us to simplify our compile process and better implement standard C# components.
          Mocha supports all standard C# references: Literal DLL, project, and <a href="#mochanuget" class="reference">NuGet</a> packages.
          We also roll our own csproj reader so that we are not trapped into depending on the monolithic MSBuild system for end users.
          With our implementation, we can also support typical options used by developers.
          Those include but are not limited to: language version, root namespace, nullable reference types, allow unsafe code, and more.
        </p>
      </div>

      <div id="mochanuget" class="blog-entry">
        <a href="#mochanuget" class="title">Mocha - Runtime NuGet</a>
        <div></div>
        <p>
          NuGet was a huge thing I wanted to support in Mocha.
          It allows developers to make use of large libraries of code to improve their projects.
          At the same time, it also allows Mocha to only implement what is required in the engine.
          Things like Json can be outsourced to the standard .NET library or the <a href="https://www.newtonsoft.com/json" class="reference">Newtonsoft.Json</a> package.
          Implementing NuGet into the project was surprisingly trivial using NuGets own NuGet package.
          All that needed to happen was the following:
        </p>
        <ul>
          <li>Retrieve the NuGet package from their API.</li>
          <li>Search that package for the version of the DLL that best matches our environment.</li>
          <li>Extract it and add it as a reference in our build process.</li>
          <li>Search the packages dependencies and repeat the process for them</li>
        </ul>
        <p>
          My first test was to download the NuGet package used to create this and cycle through all of its dependencies:
        </p>
        <video controls width="600" height="338">
          <source src="videos/Mocha NuGet Test.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
        <p>
          A resounding success! Transplanting this logic to Mocha was simple since the whole process is already implemented in C#.
          Just add an extra step to download referenced NuGet packages then add their DLLs to the references list.
          With that done we now have NuGet support for all to use:
        </p>
        <video controls width="600" height="338">
          <source src="videos/Mocha NuGet Complete.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="mochaincremental" class="blog-entry">
        <a href="#mochaincremental" class="title">Mocha - Incremental Compilation</a>
        <div></div>
        <p>
          When it comes to hotloading we used to run a full compile of the project before we do swaps.
          This works, but it can be much faster and we do not need to do a full recompile for a single line change.
          My solution to this was to make use of the <a href="https://learn.microsoft.com/en-us/dotnet/api/microsoft.codeanalysis.adhocworkspace?view=roslyn-dotnet-4.3.0" class="reference">AdhocWorkspace</a>.
          This workspace allows you to apply changed files to a compilation and can compile only the changed files which greatly increases performance.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Mocha Incremental Compilation.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="latte" class="blog-entry">
        <a href="#latte" class="title">Latte</a>
        <h4>Made by Me</h4>
        <div></div>
        <img class="icon-img" src="projects/Latte.jpg"/>
        <p>
          Latte is a 100% C# engine made by myself
          It uses a Vulkan renderer, a robust hotloading layer, and scripting also in C#.
        </p>
      </div>

      <div id="lattehotload" class="blog-entry">
        <a href="#lattehotload" class="title">Latte - Hotloading</a>
        <div></div>
        <p>
          One of the design choices I have made with Latte is to make the entire engine hotloadble.
          This way I can dog food the hotloading system to be an excellent foundation for fast iterating within the engine.
          With this in mind there had to be a lot of improvements from my work in <a href="#mocha" class="reference">Mocha</a>'s implementation.
        </p>
        <p>
          One big pain point was my NuGet implementation.
          While it did work at times, once you scale it up everything falls apart.
          Race conditions all over the place and hard to debug behavior.
          Due to this I completely re-wrote it all to work concurrently with high performance.
          In my test I was able to install my windowing projects thirty-two NuGet dependencies concurrently without any issue.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Latte Hotloading1.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
        <p>
          Another thing I wanted a proper implementation for was project references.
          Its current implementation was to look for project references, then point to a DLL file in Mocha's build directory.
          This worked in very primitive circumstances but would completely break if what was essentially a guess was wrong.
          Also if you had any dependencies that were not already loaded then it would also break there.
          With this new implementation it will hotload any project references recursively and concurrently.
          This way you can have hotloading capabilities for your main assembly and any of its references.
          All of this can be handled automatically so you do not have to think about loading anything other than the assembly you want.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Latte Hotloading2.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="grubs" class="blog-entry">
        <a href="#grubs" class="title">Grubs</a>
        <h4>Made by Ape Tavern</h4>
        <div></div>
        <img class="icon-img" src="projects/Grubs.png"/>
        <p>
          Grubs is a worms inspired turn-based artillery shooter.
          I joined Ape Tavern and Grubs' development just after they decided to start the project from scratch.
          Some of the sections that I contributed to were the game loop, weapon asset conversions, grub death quips, and improving the terrain networking.
          <br />
          <br />
          You can view the project <a href="https://github.com/apetavern/sbox-grubs" class="reference">on GitHub here</a>.
          <br />
          The game can also be played on S&amp;box with the identifier "apetavern.grubs"
        </p>
      </div>

      <div id="grubsloop" class="blog-entry">
        <a href="#grubsloop" class="title">Grubs - Game Loop</a>
        <div></div>
        <p>
          The game loop was my first project upon joining the team.
          I built the foundation of the turn-based loop that people love from Worms.
          Having your time to move around, picking your weapon, utilizing it, and running for the hills.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Grubs Game Loop1.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
        <p>
          Damaging Grubs could not happen instantly, we wanted it to be a ceremonious event of watching everything falling apart.
          Our solution was that whenever a Grub took damage we would throw them around as normal; But no damage numbers would be applied.
          We instead write down the damage so that when the turn ends we can run back through the damages applied during the turn.
          It was also implemented in a way that we knew what kind of damage was applied so that we could make use of it in the <a href="#grubsdeaths" class="reference">grub death quips</a>.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Grubs Game Loop2.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="grubsassets" class="blog-entry">
        <a href="#grubsassets" class="title">Grubs - Weapon Assets</a>
        <div></div>
        <p>
          At the start of development, weapons were just implemented as C# classes.
          While this worked, we could be using <a href="https://wiki.facepunch.com/sbox/Custom_Asset_Types" class="reference">assets</a> instead.
          Moving to assets allowed us to create weapons in a more generic fashion.
          Where before we would have four different classes that are all some variation of a projectile weapon.
          We now have a single base class that can be given a weapon asset.
        </p>
      </div>

      <div id="grubsdeaths" class="blog-entry">
        <a href="#grubsdeaths" class="title">Grubs - Death Quips</a>
        <div></div>
        <p>
          Voice lines from Worms are well known.
          While we did not have voice lines available to us.
          We did still have a chatbox we could throw messages into.
          Using the Grub damage we set up in the <a href="#grubsloop" class="reference">game loop</a> we can figure out a death cause quite easily:
        </p>
        <pre>
          <code v-highlight class="csharp">
public struct DeathReason
{
  public readonly Grub Grub;
  public readonly DamageInfo? FirstInfo;
  public readonly DamageType FirstReason;
  public readonly DamageInfo? SecondInfo;
  public readonly DamageType SecondReason;

  public override string ToString()
  {
    switch ( FirstReason )
    {
      case DamageType.HitScan:
        switch ( SecondReason )
        {
          case DamageType.Explosion:
            return $"{Grub.Name} got shot so hard they blew up.";
          case DamageType.Fall:
            return $"{Grub.Name} suffered an unfortunate fall.";
          case DamageType.KillTrigger:
            return $"{Grub.Name} was no-scoped into hell.";
          case DamageType.HitScan:
            return $"{Grub.Name} was made into swiss cheese.";
          case DamageType.Melee:
            return $"{Grub.Name} was pulverized.";
        }
        break;
    }
  }
}
          </code>
        </pre>
      </div>

      <div id="coldopen" class="blog-entry">
        <a href="#coldopen" class="title">Cold Open</a>
        <h4>Made by Ape Tavern</h4>
        <div></div>
        <img class="icon-img" src="projects/Cold Open.png"/>
        <p>
          Cold Open is Ape Tavern's submission to the 2022 horror game jam hosted by Gvardos.
          The game is about a sole survivor on a ship that wakes up from cryosleep to discover his situation.
          While adventuring the ship for clues, he releases a monster that created the mess he is in and now has to escape the ship.
          My main contribution to the game was creating the interaction system that is used throughout the game for set-pieces and item collection.
        </p>
      </div>

      <div id="coldopeninteraction" class="blog-entry">
        <a href="#coldopeninteraction" class="title">Cold Open - Interactions</a>
        <div></div>
        <p>
          For interactions in Cold Open we needed something that could be used for better immersion and also serve a gameplay purpose.
          What we came up with was a third person camera watching the player interact with the world.
          This camera can also rotate so that the player can look out for the monster coming after them.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Cold Open Interaction.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
        <p>
          With this system in place we could implement instant use items like the note. Or a delayed action with steps like the battery pack.
          We could also add a camera system that you could interact with to view the map.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Cold Open Cameras.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="parkourpainters" class="blog-entry">
        <a href="#parkourpainters" class="title">Parkour Painters</a>
        <h4>Made by Ape Tavern</h4>
        <div></div>
        <img class="icon-img" src="projects/Parkour Painters.png"/>
        <p>
          Parkour Painters is Ape Tavern's submission to the 2023 gang jam hosted by Three Thieves.
          The game is a multiplayer team versus team parkour vandalizing game taking inspiration from <a href="https://en.wikipedia.org/wiki/Jet_Set_Radio" class="reference">Jet Set Radio</a>.
          My main contributions to the game were creating the game loop and the implementation of teams.
          <br />
          <br />
          You can view the project <a href="https://github.com/apetavern/parkour-painters" class="reference">on GitHub here</a>.
          <br />
          The game can also be played on S&amp;box with the identifier "apetavern.parkour_painters"
        </p>
      </div>

      <div id="pong" class="blog-entry">
        <a href="#pong" class="title">Menu Pong</a>
        <div></div>
        <p>
          With the creation of <a href="https://sbox.facepunch.com/news/game-menus" class="reference">game menus</a> I saw an opportunity to learn some UI while still making a game.
          It was a quick 24 hour game deal that stays true to the original creation.
          <br />
          <br />
          You can view the project <a href="https://github.com/peter-r-g/Sbox-MenuPong" class="reference">on GitHub here</a>.
          <br />
          The game can also be played on S&box with the identifier "gooman.menu_pong"
        </p>
        <video controls width="600" height="338">
          <source src="videos/Menu Pong.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="netbolt" class="blog-entry">
        <a href="#netbolt" class="title">NetBolt</a>
        <h4>Made by Me</h4>
        <div></div>
        <img class="icon-img" src="projects/NetBolt.png"/>
        <p>
          NetBolt is a project to implement a custom networking architecture to S&amp;box.
          It has features such as: networkable types, generics, delta updates, and bi-directional plus asynchronous reponse RPCs.
          Input is implemented through a client authority system that servers can rollback on if something does not seem right.
          <br />
          <br />
          You can view the project <a href="https://github.com/Sbox-NetBolt/NetBolt.Core" class="reference">on GitHub here</a>.
          <br />
          The waywo thread related to this project can be found <a href="https://discord.com/channels/833983068468936704/1025058217262317568/1025058217262317568" class="reference">on the S&amp;box Discord here</a>.
        </p>
      </div>

      <div id="netbolttypes" class="blog-entry">
        <a href="#netbolttypes" class="title">NetBolt - Networkable Types</a>
        <div></div>
        <p>
          Networkable types are implemented in a different way from S&amp;box. Instead of using base C# types and working around them.
          You create your own types and wrap around the values you want to use.
          This allows us to better see when changes occur and whether we want to support lerping, delta updates, etc.
          To create your own networkable type, you just need to add the INetworkable interface to it.
        </p>
        <pre>
          <code v-highlight class="csharp">
public interface INetworkable
{
	int NetworkId { get; }
	bool SupportEquals { get; }
	bool SupportLerp { get; }

	bool Changed();
	bool Equals( INetworkable? oldValue );
	void Lerp( float fraction, INetworkable oldValue, INetworkable newValue );

	void Deserialize( NetworkReader reader );
	void DeserializeChanges( NetworkReader reader );
	void Serialize( NetworkWriter writer );
	void SerializeChanges( NetworkWriter writer );
}
          </code>
        </pre>
        <p>
          In ComplexNetworkables (NetBolt equivalent of BaseNetworkables) any properties no matter their accessors are subject to networking.
          NetBolt includes a few attributes that can augment their usability.
          <h3>ClientAuthority</h3>
          This attribute marks a property that can be edited by the client for the purposes of their input affecting it.
          A server can still stomp this value if needed.
          <pre>
            <code v-highlight class="csharp">
[ClientAuthority]
public NetworkedVector3 Position { get; set; }
            </code>
          </pre>
          <h3>Lerp</h3>
          This attributes marks a property that should utilize an INetworkables lerping functionality.
          This is useful in a case where you have a 3D position of an entity that needs to be lerped to its next value.
          <pre>
            <code v-highlight class="csharp">
[ClientAuthority, Lerp]
public NetworkedVector3 Position { get; set; }
            </code>
          </pre>
          <h3>NoNetwork</h3>
          This attribute marks a property to be ignored when networking.
          This is used in cases where you keep a reference to an INetworkable but do not want it networked.
          <pre>
            <code v-highlight class="csharp">
[NoNetwork]
public NetworkedVector3 CopyOfPosition => Position;
            </code>
          </pre>
        </p>
        <video controls width="600" height="338">
          <source src="videos/NetBolt Networking.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="netboltrpcs" class="blog-entry">
        <a href="#netboltrpcs" class="title">NetBolt - RPCs</a>
        <div></div>
        <p>
          NetBolt features bi-directional Remote Procedure Calls (RPCs) as well with asynchronous responses.
          RPCs can exist as static methods or as instance methods on network entities and their components.
          RPCs can also be passed any number of parameters so long as they are a <a href="#netbolttypes" class="reference">networkable type</a>.
        </p>
        <pre>
          <code v-highlight class="csharp">
[Rpc.Server]
private void ServerRpc()
{
#if CLIENT
  this.CallRpc( nameof( ServerRpc ) );
#endif
#if SERVER
  Log.Info( "This will be executed on the server!" );
#endif
}
          </code>
        </pre>
        <p>
          Asynchronous responses can also be used to get one time networkable data from another realm.
        </p>
        <pre>
          <code v-highlight class="csharp">
[Rpc.Client]
private async ValueTask&lt;INetworkable&gt; ClientRpc()
{
#if SERVER
  var result = await this.CallRpcAsync( Owner, nameof( ClientRpc ) )
  Log.Info( result );
  return result;
#endif
#if CLIENT
  return new NetworkedString( "Hello from the client!" );
#endif
}
          </code>
        </pre>
      </div>

      <div id="netboltcomponents" class="blog-entry">
        <a href="#netboltcomponents" class="title">NetBolt - Entity Components</a>
        <div></div>
        <p>
          Components also have first-party support in NetBolt.
          They are implemented as ComplexNetworkables which can leverage networked properties, attributes, and RPCs.
          This allows you to create generic functionality that can be used across differing entities.
        </p>
        <pre>
          <code v-highlight class="csharp">
// SERVER
public class ColorComponent : EntityComponent
{
    public NetworkedVector3 Color { get; set; } = System.Numerics.Vector3.Zero;

    [ServerOnly, Rpc.Server]
    public void RotateColorRpc()
    {
        Color = new NetworkedVector3( Random.Shared.NextSingle() * 255, Random.Shared.NextSingle() * 255, Random.Shared.NextSingle() * 255 );
    }
}
        </code>
      </pre>
      <pre>
        <code v-highlight class="csharp">
// CLIENT
public class ColorComponent : EntityComponent
{
    public NetworkedVector3 Color { get; set; } = System.Numerics.Vector3.Zero;

    [ClientOnly]
    public void RotateColorRpc()
    {
        this.CallRpc( "RotateColorRpc" );
    }
}

// In NetworkEntity.UpdateClient
if ( Components.TryGetComponent&lt;ColorComponent&gt;( out var component ) )
{
    var color = component.Color;
    _player.RenderColor = Color.FromBytes( (int)color.X, (int)color.Y, (int)color.Z );

    if ( Input.Pressed( "flashlight" ) )
        component.RotateColorRpc();
}
          </code>
        </pre>
        <video controls width="600" height="338">
          <source src="videos/NetBolt Components.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="entitypools" class="blog-entry">
        <a href="#entitypools" class="title">Entity Pools</a>
        <h4>Made by Me</h4>
        <div></div>
        <img class="icon-img" src="projects/Entity Pools.png"/>
        <p>
          This is a small library available on <a href="https://asset.party/gooman/entity_pools" class="reference">asset.party</a> that provides pooling mechanisms for S&amp;box entities.
          It features an API that works like C#s <a href="https://learn.microsoft.com/en-us/dotnet/api/system.buffers.arraypool-1?view=net-7.0" class="reference">ArrayPool&lt;T&gt;</a> API.
          <br />
          <br />
          You can view the project <a href="https://github.com/peter-r-g/Sbox-EntityPools" class="reference">on GitHub here</a>.
        </p>
        <pre>
          <code v-highlight class="csharp">
// Get yourself an existing or new instance of MyEntity.
var rentedEntity = EntityPool&lt;MyEntity&gt;.Shared.Rent();

// Mess with your rented entity.

// Return the entity back to the pool for reuse.
EntityPool&lt;MyEntity&gt;.Shared.Return( rentedEntity );
          </code>
        </pre>
      </div>

      <div id="coroutines" class="blog-entry">
        <a href="#coroutines" class="title">Coroutines</a>
        <h4>Made by Me</h4>
        <div></div>
        <img class="icon-img" src="projects/Coroutines.png"/>
        <p>
          This is another library available on <a href="https://asset.party/gooman/coroutines" class="reference">asset.party</a> that provides a main thread coroutine library much like <a href="https://docs.unity3d.com/2023.2/Documentation/Manual/Coroutines.html" class="reference">Unity's API</a>.
          It provides a number of default objects to delay your coroutines as well as the ability to create your own specialized objects.
        </p>
        <pre>
          <code v-highlight class="csharp">
// Define your coroutine
IEnumerator&lt;ICoroutineStaller&gt; MyBehavior()
{
  Log.Info( "Hello!" );
  yield return new WaitForSeconds( 5 );
  Log.Info( "I waited 5 seconds" );
}

// Run your coroutine
Coroutine.Start( MyBehavior )
          </code>
        </pre>
        <p>
          The coroutine library also offers the ability to execute your future code within different contexts.
          The currently supported contexts are on server/client tick, client frame, and a clients input simulation.
          You can enter these contexts with specific objects for the context or you can pass a different context to objects like "WaitForSeconds."
        </p>
        <pre>
          <code v-highlight class="csharp">
IEnumerator&lt;ICoroutineStaller&gt; MyBehavior()
{
  Log.Info( "We are in tick" );
  yield return new WaitForSeconds( 1, ExecutionStrategy.Frame );
  Log.Info( "We are in frame" );
  var firstClient = Game.Clients.First();
  yield return new WaitForNextSimulate( firstClient );
  Log.Info( $"We are in {firstClient.Name}'s' input simulation" );
}
          </code>
        </pre>
        <p>
          You can view the project <a href="https://github.com/peter-r-g/Sbox-Coroutines" class="reference">on GitHub here</a>.
        </p>
      </div>

      <div id="iconbrowser" class="blog-entry">
        <a href="#iconbrowser" class="title">Icon Browser Tool</a>
        <h4>Made by Me</h4>
        <div></div>
        <img class="icon-img" src="projects/Icon Browser.png"/>
        <p>
          The icon browser is a tool that allows you to view all material design icons available in S&box tools.
          It features a number of things:
          <ul>
            <li>Shows all icons as defined in the Google material design font.</li>
            <li>Filter icons by name.</li>
            <li>Copy icon names in three formats.</li>
            <li>Dump all icons to a C# file that you can use in tool code in place of handmade strings.</li>
          </ul>
          <br />
          <br />
          You can view the project <a href="https://github.com/peter-r-g/Sbox-IconBrowser" class="reference">on GitHub here</a>.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Icon Browser.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="ptd" class="blog-entry">
        <a href="#ptd" class="title">Project Template Downloader Tool</a>
        <h4>Made by Me</h4>
        <div></div>
        <img class="icon-img" src="projects/Template Downloader.png"/>
        <p>
          The project template downloader is a tool that allows you to download S&amp;box templates from GitHub through their API.
          It features automatic searching of all S&amp;box template repositories, easy to install, update, and delete templates, and a quick shortcut from the Project Create dialog.
          <br />
          <br />
          You can view the project <a href="https://github.com/peter-r-g/Sbox-ProjectTemplateDownloader" class="reference">on GitHub here</a>.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Project Template Downloader.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>

      <div id="iag" class="blog-entry">
        <a href="#iag" class="title">Input Actions Generator Tool</a>
        <h4>Made by Me</h4>
        <div></div>
        <img class="icon-img" src="projects/Input Actions Generator.png"/>
        <p>
          This S&amp;box tool auto generates C# code for you based on the input settings of your gamemode project.
          It includes your input name, its category, and both the keyboard and gamepad default mapping.
          The struct that the data comes in implicitly returns the input name so it can be used seamlessly with the <a href="https://asset.party/api/Sandbox.Input" class="reference">Input</a> API.
          <br />
          <br />
          You can view the project <a href="https://github.com/peter-r-g/Sbox-InputActionsGenerator" class="reference">on GitHub here</a>.
        </p>
        <video controls width="600" height="338">
          <source src="videos/Input Actions Generator.mp4" type="video/mp4">
          Your browser does not support videos, sorry!
        </video>
      </div>
    </div>
  </main>
</template>

<style lang="scss" scoped>
@import "~assets/variables.scss";

h1.title,h3.subtitle {
  text-align: center;
}

div.header,div.blog-entry {
  margin: 10px 0px 50px 0px;
}

div.blog-entry {
  a {
    scroll-behavior: smooth;
  }

  a.title {
    font-size: 32px;
    font-weight: bold;
  }

  a.reference {
    font-weight: bold;
  }

  h3 {
    margin-top: 10px;
  }

  img.icon-img {
    display: block;
    max-width: 256px;
    max-height: 256px;
    margin: 5% auto;
  }

  video {
    display: block;
    margin: 5% auto;
  }

  code {
    max-height: 50vh;
  }
}
</style>
