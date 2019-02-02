# imgui_markdown

## Markdown for Dear ImGui

A permissively licensed markdown single-header library for [Dear ImGui](https://github.com/ocornut/imgui).

imgui_markdown currently supports the following markdown functionality:

  * Wrapped text
  * Headers H1, H2, H3
  * Indented text, multi levels
  * Unordered lists and sub-lists
  * Links

![imgui_markdown demo live editing](https://github.com/juliettef/Media/blob/master/imgui_markdown_demo_live_editing.gif)

## Syntax

### Wrapping
Text wraps automatically. To add a new line, use 'Return'.
### Headers
```
# H1
## H2
### H3
```
### Lists
#### Indents
On a new line, at the start of the line, add two spaces per indent.
```
Normal text
··Indent level 1
····Indent level 2
······Indent level 3
Normal text
```
#### Unordered lists
On a new line, at the start of the line, add two spaces, an asterisks and a space. For nested lists, add two additional spaces in front of the asterisk per list level increment.
```
Normal text
··*·Unordered List level 1
····*·Unordered List level 2
······*·Unordered List level 3
······*·Unordered List level 3
··*·Unordered List level 1
Normal text
```
### Links
```
[link description](https://...)
```

![Example use of imgui_markdown with icon fonts](https://github.com/juliettef/Media/blob/master/imgui_markdown_icon_font.jpg)

## Example use on Windows with links opening in a browser

```Cpp

#include "ImGui.h"                // https://github.com/ocornut/imgui
#include "imgui_markdown.h"       // https://github.com/juliettef/imgui_markdown
#include "IconsFontAwesome5.h"    // https://github.com/juliettef/IconFontCppHeaders

// Following includes for Windows LinkCallback
#define WIN32_LEAN_AND_MEAN
#include <Windows.h>
#include "Shellapi.h"
#include <string>

// You can make your own Markdown function with your prefered string container and markdown config.
static ImGui::MarkdownConfig mdConfig{ LinkCallback, ICON_FA_LINK, { { NULL, true }, { NULL, true }, { NULL, false } } };

void LinkCallback( MarkdownLinkCallbackData data_ )
{
    std::string url( data_.link, data_.linkLength );
    ShellExecuteA( NULL, "open", url.c_str(), NULL, NULL, SW_SHOWNORMAL );
}

void LoadFonts( float fontSize_ = 12.0f )
{
    ImGuiIO& io = ImGui::GetIO();
    io.Fonts->Clear();
    // Base font
    io.Fonts->AddFontFromFileTTF( "myfont.ttf", fontSize_ );
    // Bold headings H2 and H3
    mdConfig.headingFormats[ 1 ].font = io.Fonts->AddFontFromFileTTF( "myfont-bold.ttf", fontSize_ );
    mdConfig.headingFormats[ 2 ].font = mdConfig.headingFormats[ 1 ].font;
    // bold heading H1
    float fontSizeH1 = fontSize_ * 1.1f;
    mdConfig.headingFormats[ 0 ].font = io.Fonts->AddFontFromFileTTF( "myfont-bold.ttf", fontSizeH1 );
}

void Markdown( const std::string& markdown_ )
{
    // fonts for, respectively, headings H1, H2, H3 and beyond
    ImGui::Markdown( markdown_.c_str(), markdown_.length(), mdConfig );
}

void MarkdownExample()
{
    const std::string markdownText = u8R"(
# H1 Header: Text and Links
You can add [links like this one to enkisoftware](https://www.enkisoftware.com/) and lines will wrap well.
## H2 Header: indented text.
  This text has an indent (two leading spaces).
    This one has two.
### H3 Header: Lists
  * Unordered lists
    * Lists can be indented with two extra spaces.
  * Lists can have [links like this one to Avoyd](https://www.avoyd.com/)
)";
    Markdown( markdownText );
}
```

## Projects using imgui_markdown

### [Avoyd](https://www.avoyd.com)
Avoyd is an abstract 6 degrees of freedom voxel game. The game and the voxel editor's help and tutorials use imgui_markdown with Dear ImGui.

![imgui_markdown used in Avoyd](https://github.com/juliettef/Media/blob/master/imgui_markdown_Avoyd_about_OSS.png)

## Credits

Design and implementation - [Doug Binks](http://www.enkisoftware.com/about.html#doug) - [@dougbinks](https://github.com/dougbinks)  
Implementation and maintenance - [Juliette Foucaut](http://www.enkisoftware.com/about.html#juliette) - [@juliettef](https://github.com/juliettef)  
Thanks to [Omar Cornut for Dear ImGui](https://github.com/ocornut/imgui).

## License (zlib)

Copyright (c) 2019 Juliette Foucaut and Doug Binks

This software is provided 'as-is', without any express or implied
warranty. In no event will the authors be held liable for any damages
arising from the use of this software.

Permission is granted to anyone to use this software for any purpose,
including commercial applications, and to alter it and redistribute it
freely, subject to the following restrictions:

1. The origin of this software must not be misrepresented; you must not
   claim that you wrote the original software. If you use this software
   in a product, an acknowledgment in the product documentation would be
   appreciated but is not required.
2. Altered source versions must be plainly marked as such, and must not be
   misrepresented as being the original software.
3. This notice may not be removed or altered from any source distribution.
