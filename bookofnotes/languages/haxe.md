# Haxe

## Installation

1. Download jre >= 1.6 32bit
2. Install Haxedevelop (it is a 32bit app)
3. Download and install haxe from haxe.org
4. From a mirror/apache/flex/4.16.0/binaries download the apache flex zip and uncompress it
5. From Haxedevelop "Tools -> Install Software" install "Flash Player (SA)"
6. HaxeDevelop "Tools -> Program Settings -> AS3Context -> Installed Flex SDK -> Add", select the flex uncompressed path
7. HaxeDevelop "Tools -> Program Settings -> HaxeContext -> Installed Haxe SDK -> Add", select the haxe installed path
8. restart HaxeDevelop
9. cmd -> haxelib install openfl && haxelib run openfl setup
10. cmd -> haxelib install away3d && haxelib install away3d-samples
11. --- Should be ready to go ---


On HaxeDevelop, may need to install the debug flash player from the menu "Tools -> Install Software".
Then the debug flash player will be configured automatically on "Tools -> Program Settings -> FlashViewer".
Also, when compiling the applications, "Enable Debugger" should be "true" on "Project -> Properties -> Compiler Options"

If wants to run on flash and use the debugger (not needing the browser), edit project.xml:

```xml
	<setenv name="SWF_PLAYER" value="path/to/debug/player" />
```

Path to the debug player can be get from haxedevelop

## Away3d

* When using away3d on the browser, **DO USE `wmode=direct`**, it may work ok on firefox without it, but IE and Chrome will show black window of nothingness
* It works better on IE than Chrome... may need to check why.
