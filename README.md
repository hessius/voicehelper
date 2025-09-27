<link rel="icon" href="https://voicehelper.io/logo/SVG/logo.svg" type="image/svg+xml">

<img src="https://voicehelper.io/logo/SVG/logo.svg">

# VoiceHelper

1. [VoiceHelper](#voicehelper)
   1. [What is VoiceHelper?](#what-is-voicehelper)
   2. [Why?](#why)
   3. [How it works](#how-it-works)
   4. [How to use with DMO](#how-to-use-with-dmo)
   5. [Parameters](#parameters)
      1. [Predefined domains](#predefined-domains)
   6. [How to add predefined domains](#how-to-add-predefined-domains)
   7. [Utility Files](#utility-files)
   8. [Examples](#examples)
   9. [Logo](#logo)
   10. [Contact / Issues](#contact--issues)
   11. [License](#license)
   12. [Acknowledgements](#acknowledgements)

## What is VoiceHelper?

VoiceHelper is a service that adds search as a skill to Dragon Medical One (DMO). It achieves this by acting as an intermediate page to facilitate search through step by step commands executed through DMO.

## Why?

When executing step by step commands from DMO that go beyond the EHR, specifically to interact with websites, commands are prone to breaking due to layout changes. This is due to the engine navigating the UI using a fixed set of instructions, e.g. tabbing through a set of elements to reach a search field / text box where text can be entered. If the design of the webpage changes, e.g. because an ad is inserted or a cookie notice introduced, the number of tab button presses needed to hit the correct field changes, and thus the step by step command fails. Even when without layout changes the focus jumping incurred by repeating tab presses is visible to the user, visually jarring and slow. Overall this equals very bad UX.

_But this service fixes that._

VoiceHelper "invisibly" and near instantaneously converts provided parameters to a complete search url and redirects the user to the results page - thus enabling quick and easy step by step commands that let the user search across a multitude of websites by voice. In short, VoiceHelper unlocks some of the power of the step by step commands that are available in DMO. Essentially a community contributed skill for DMO.

## How it works

By design this service contains no visible UI in its initial state, this is in order to be basically invisible to the end user - searches appear to go directly from DMO to the domain that is being searched.
The service takes arguments from two fronts:

**a)** in the form of URL search params e.g. domain.com?foo=bar

**b)** from pasted text

To use the service, when accessing this page you need to to provide a url parameter specifying a site that either matches a predefined search engine (see below) or is a valid url. If accessed with a valid url parameter the service can receive pasted text. The input from the URL parameters and the pasted text is combined to generate a complete search URL, to which the user is redirected. Each new line of text will generate a new query, e.g. enabling the user to search a list of medications. **While the process might sound complex this happens (almost) instantaneously and is (almost) invisible to the user.**

An additional feature to note is the ability to translate text strings so that e.g. a Swedish string can be converted into English to enable searching an English language resource.

If a domain has been provided but no search text is provided on paste - VoiceHelper shows a search box where the user can enter search text and sets focus to it so that the user can use DMO to directly search the specified site.

## How to use with DMO

1. Create a new step by step command "search X" (X being the database)
2. Add the commands:
   - Keyboard shortcut: Ctrl-C
   - Run program: https://voicehelper.io/?site=[INSERT SITE HERE]&lang=[INSERT LANGUANGE HERE (OPTIONAL)]
   - Wait: e.g. 3000 ms to let the browser load, the website is minimal enough to load nearly instantaneously
   - Keyboard shortcut: Ctrl-V
3. Invoke the shortcut by saying "Search X", if text has been selected, the service searches the database of choice for that term, if needed with added translation. If no text has been selected the user is presented a search box.

## Parameters

Currently two parameters are available

1. Site (Required)
   Accepts either one of the predefined domains (see below) or a full url, please note that the url needed is the full search URL to which a query can be appended. e.g. "https://www.bing.com/search?q="
2. Lang (optional)
   Accepts any two letter language code ([ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)), e.g. "en" for English and "sv" for Swedish. Is used to enable automatic translation of query for searches in a resource in another language than the source.

### Predefined domains

| Name             | Param | Param alt       | Param alt 2 | Description                                                                         | Language |
| ---------------- | ----- | --------------- | ----------- | ----------------------------------------------------------------------------------- | -------- |
| AICD.nu          | aicd  | icd             |             | AI powered coding search engine, covers diagnostic as well procedure codes          | Swedish  |
| Bing             | b     | bing            |             | General purpose search engine                                                       | Any      |
| BDPM             | bdpm  |                 |             | Pharmaceutical information database, N.B. search performed through google           | French   |
| FASS             | fass  |                 |             | Pharmaceutical information database                                                 | Swedish  |
| Giftinformation  | gi    | giftinfo        |             | Swedish national toxicology database                                                | Swedish  |
| Google           | g     | google          |             | General purpose search engine                                                       | Any      |
| HAS              | has   |                 |             | Collection of databases with practice guidelines, pharmaceutical info etc           | French   |
| Internetmedicin  | im    | internetmedicin |             | Swedish equivalent of Uptodate                                                      | Swedish  |
| Janusinfo        | ji    | janusinfo       |             | Swedish National collection of emergency medicine resources and pharmaceutical info | Swedish  |
| Läkemedelsboken  | lmb   | lakemedelsboken |             | Pharmaceutical guidelines                                                           | Swedish  |
| MD Calc          | mdc   | mdcalc          |             | Clinical calculators                                                                | English  |
| Medscape         | ms    | medscape        |             | Clinical reference (searches drugs & diseases database)                             | English  |
| Praktisk medicin | pm    | praktiskmedicin |             | Large collection of medical information for primary care                            | Swedish  |
| Svensk MESH      | mesh  |                 |             | Medical dictionary with Swedish and English translations                            | Swedish  |
| Uptodate         | u2d   | uptodate        | utd         | Large collection of medical information                                             | English  |
| Vårdhandboken    | vhb   | vardhandboken   |             | Practical instructions re: medical procedures                                       | Swedish  |

## How to add predefined domains

Adding predefined domains is easy, just add the domain to the list in the index.html fiole and the service will automatically add it to available domains.
The structure of the predefined domains is as follows:

```js
[param name]: {
    url: [search url],
    name: [display name],
    lang: [language code],
    filter: [optional: regex to define a filter, e.g. for Giftinformation this filters out everything after the first two words to enable searching only for drugs]
},
```

## Utility Files

VoiceHelper includes two utility HTML files that provide window positioning functionality for Dragon Medical One (DMO) integration:

### opener.html

The `opener.html` file is a utility that opens a positioned window containing `redirector.html`. It accepts URL parameters to control window positioning and passes them through to the redirector.

#### Parameters
- **url** (optional): The target URL to redirect to
- **url2, url3, etc.** (optional): Additional URLs for multi-option selection
- **title** (optional): Display title for the first URL
- **title2, title3, etc.** (optional): Display titles for additional URLs
- **image** (optional): URL to an image to display in the selection interface
- **hideurl** (optional): Set to "true" to hide URL display in the selection interface
- **width** (optional): Window width in pixels (default: 500)
- **height** (optional): Window height in pixels (default: full screen height)

#### Functionality
- Calculates positioning for the bottom-right corner of the screen
- Opens a new window with `redirector.html` at the calculated position
- Supports multiple URLs for creating selection interfaces
- Passes through all URL parameters to the redirector
- Closes itself immediately after opening the new window
- Provides fallback UI with user-friendly options if window closing fails

#### Usage Examples

**Single URL redirect:**
```
opener.html?url=https://example.com&width=600&height=400
```

**Multiple URL selection with titles:**
```
opener.html?url=https://site1.com&url2=https://site2.com&title=First%20Site&title2=Second%20Site
```

**Multiple URLs with image and hidden URLs:**
```
opener.html?url=https://site1.com&url2=https://site2.com&title=Option%201&title2=Option%202&image=https://example.com/logo.png&hideurl=true
```

#### Error Handling
When the window cannot be closed automatically (due to browser security restrictions), opener.html provides a user-friendly interface with options to:
- Try closing again
- Navigate back to the previous page
- Manual instructions for closing the tab

### redirector.html

The `redirector.html` file positions itself to the right edge of the screen and either redirects to a target URL or presents a selection interface for multiple URLs. It's designed to work seamlessly with DMO's step-by-step commands and supports voice commands for option selection.

#### Parameters
- **url** (required for single redirect): The target URL to redirect to. Must be a valid HTTP/HTTPS URL
- **url2, url3, etc.** (optional): Additional URLs for multi-option selection
- **title** (optional): Display title for the first URL option
- **title2, title3, etc.** (optional): Display titles for additional URL options
- **image** (optional): URL to an image to display above the selection options
- **hideurl** (optional): Set to "true" to hide URL display in selection interface
- **width** (optional): Window width in pixels (default: 600)
- **height** (optional): Window height in pixels (default: full screen height)

#### Single URL Functionality
When only one URL is provided:
- Positions the window to the right edge of the screen
- Validates that the target URL is a valid HTTP/HTTPS URL for security
- Redirects to the target URL after a brief delay (100ms)
- Maintains the positioned window layout during the redirect

#### Multiple URL Selection Functionality
When multiple URLs are provided:
- Creates a numbered selection interface with clickable options
- Displays titles for each option (if provided)
- Shows or hides URLs based on the `hideurl` parameter
- Displays an optional image at the top of the selection interface
- Supports voice command selection in both Swedish and English
- Supports direct keyboard number selection (1-9)

#### Voice Command Support
The redirector recognizes various voice command patterns:
- **Direct numbers**: "4", "5" (any digit)
- **Swedish commands**: "välj 3" (select 3)
- **English commands**: "select 2", "number 4", "choose 1"
- **Number words**: "two", "three", "fyra", "fem" (supports 1-13 in both languages)

#### Keyboard Navigation
- Press number keys (1-9) to directly select options
- Options are highlighted on hover for visual feedback

#### Usage Examples

**Single URL redirect:**
```
redirector.html?url=https://example.com&width=800
```

**Multiple URL selection with titles:**
```
redirector.html?url=https://site1.com&url2=https://site2.com&title=Primary%20Site&title2=Secondary%20Site
```

**Multiple URLs with image and hidden URLs:**
```
redirector.html?url=https://medical1.com&url2=https://medical2.com&url3=https://medical3.com&title=Database%20A&title2=Database%20B&title3=Database%20C&image=https://example.com/medical-logo.png&hideurl=true
```

#### Security Note
The redirector validates all URLs using a regex pattern to ensure only HTTP and HTTPS protocols are allowed, preventing potential security issues from javascript: or other protocol schemes.

#### Integration with Dragon Medical One
The multiple URL selection feature is particularly useful for DMO integration where users can:
1. Say "opener medical databases" to open the selection interface
2. Say "select 2" or "välj 3" to choose from multiple medical databases
3. Have immediate access to their chosen resource without manual navigation

## Step-by-Step Command Examples for DMO

This section provides detailed instructions for creating Dragon Medical One step-by-step commands that utilize VoiceHelper's opener and redirector utilities. These examples serve as templates and inspiration for building your own voice-enabled workflows.

### Example 1: Single Website Redirect Command

**Command Name:** "Open UpToDate"

**Purpose:** Quickly open UpToDate medical database

**Step-by-step instructions:**
1. Create a new step-by-step command "Open UpToDate"
2. Add the commands:
   - **Run program:** `https://voicehelper.io/opener.html?url=https://www.uptodate.com`

**Usage:** Say "Open UpToDate" and the medical database opens immediately.

### Example 2: Multiple Database Selection Command

**Command Name:** "Medical Databases"

**Purpose:** Present a voice-controlled selection menu of multiple medical databases

**Step-by-step instructions:**
1. Create a new step-by-step command "Medical Databases"
2. Add the commands:
   - **Run program:** `https://voicehelper.io/opener.html?url=https://www.uptodate.com&url2=https://www.medscape.com&url3=https://emedicine.medscape.com&title=UpToDate&title2=Medscape&title3=eMedicine&hideurl=true`

**Usage:** 
- Say "Medical Databases" to open the selection interface
- Say "select 1", "two", or "choose three" to pick your desired database
- The selected database opens automatically

### Example 3: Search Command with Opener Integration

**Command Name:** "Search Medical Database"

**Purpose:** Select text, choose from multiple databases, and search immediately

**Step-by-step instructions:**
1. Create a new step-by-step command "Search Medical Database"
2. Add the commands:
   - **Keyboard shortcut:** Ctrl-C (copy selected text)
   - **Run program:** `https://voicehelper.io/opener.html?url=https://voicehelper.io/?site=u2d&url2=https://voicehelper.io/?site=ms&url3=https://voicehelper.io/?site=mdc&title=UpToDate&title2=Medscape&title3=MD%20Calc&hideurl=true&image=https://voicehelper.io/logo/SVG/logo.svg`

**Usage:**
- Select text in any application
- Say "Search Medical Database"
- Say "select 1" to search UpToDate, "two" for Medscape, or "three" for MD Calc
- The selected database opens with search results for your selected text

### Example 4: Direct Redirector Command

**Command Name:** "Quick PubMed"

**Purpose:** Directly redirect to PubMed search page positioned on the right side of screen

**Step-by-step instructions:**
1. Create a new step-by-step command "Quick PubMed"
2. Add the commands:
   - **Run program:** `https://voicehelper.io/redirector.html?url=https://pubmed.ncbi.nlm.nih.gov&width=800&height=600`

**Usage:** Say "Quick PubMed" and PubMed opens in an 800x600 window positioned on the right edge of your screen.

### Example 5: Advanced Multi-Language Search Command

**Command Name:** "International Medical Search"

**Purpose:** Copy text and search across international medical databases with language options

**Step-by-step instructions:**
1. Create a new step-by-step command "International Medical Search"
2. Add the commands:
   - **Keyboard shortcut:** Ctrl-C (copy selected text)
   - **Run program:** `https://voicehelper.io/opener.html?url=https://voicehelper.io/?site=u2d&lang=en&url2=https://voicehelper.io/?site=has&lang=fr&url3=https://voicehelper.io/?site=im&lang=sv&title=UpToDate%20(English)&title2=HAS%20(French)&title3=Internetmedicin%20(Swedish)&hideurl=true`

**Usage:**
- Select medical text in any language
- Say "International Medical Search"
- Say "one" for English UpToDate, "two" for French HAS, or "three" for Swedish Internetmedicin
- Text is automatically translated and searched in the selected database

### Tips for Creating Effective Commands

1. **URL Encoding:** When including special characters in titles or URLs, use URL encoding:
   - Space: `%20`
   - Ampersand: `%26`
   - Question mark: `%3F`

2. **Voice Commands:** Test voice commands with both English and Swedish phrases:
   - English: "select", "choose", "number", "one", "two", "three"
   - Swedish: "välj", "ett", "två", "tre"

3. **Window Positioning:** Use width and height parameters to position windows optimally:
   - Small reference window: `width=400&height=600`
   - Standard window: `width=800&height=600`
   - Full height: omit height parameter

4. **Error Handling:** If commands fail:
   - Check URL encoding
   - Verify all URLs are accessible
   - Test with simpler commands first

## Examples

Here are two examples of valid url parameters both querying Google:

1. https://voicehelper.io/?site=g
2. https://voicehelper.io/?site=https://www.google.com/search?q=

Here are two examples of valid url parameters querying MD Calc, one ready for an English query (no translation needed, thus no language parameter passed) and one ready for a query in German:

1. https://voicehelper.io/?site=mdc
2. https://voicehelper.io/?site=mdc&lang=de

## Logo

Logotypes are available under the same license as the rest of the software. The logo is a grayscale glyph that can be tinted to match the context where it is used. There are five different versions of the logo provided.
| <img src="https://voicehelper.io/logo/SVG/logo.svg" height=75> | <img src="https://voicehelper.io/logo/SVG/logo_blue.svg" height=75> | <img src="https://voicehelper.io/logo/SVG/logo_red.svg" height=75> | <img src="https://voicehelper.io/logo/SVG/logo_green.svg" height=75> | <img src="https://voicehelper.io/logo/SVG/logo_yellow.svg" height=75> |
|--------------------------------------------------|-------------------------------------------------------|------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------|
| <center>[**svg**](https://voicehelper.io/logo/SVG/logo.svg)</center> | <center>[**svg**](https://voicehelper.io/logo/SVG/logo_blue.svg)</center> | <center>[**svg**](https://voicehelper.io/logo/SVG/logo_red.svg)</center> | <center>[**svg**](https://voicehelper.io/logo/SVG/logo_green.svg)</center> | <center>[**svg**](https://voicehelper.io/logo/SVG/logo_yellow.svg)</center> |
| <center>[**2x**](https://voicehelper.io/logo/2x/logo@2x.png)</center> | <center>[**2x**](https://voicehelper.io/logo/2x/logo_blue@2x.png)</center> | <center>[**2x**](https://voicehelper.io/logo/2x/logo_red@2x.png)</center> | <center>[**2x**](https://voicehelper.io/logo/2x/logo_green@2x.png)</center> | <center>[**2x**](https://voicehelper.io/logo/2x/logo_yellow@2x.png)</center> |
| <center>[**4x**](https://voicehelper.io/logo/4x/logo@4x.png)</center> | <center>[**4x**](https://voicehelper.io/logo/4x/logo_blue@4x.png)</center> | <center>[**4x**](https://voicehelper.io/logo/4x/logo_red@4x.png)</center> | <center>[**4x**](https://voicehelper.io/logo/4x/logo_green@4x.png) </center>| <center>[**4x**](https://voicehelper.io/logo/4x/logo_yellow@4x.png)</center> |

## Contact / Issues

- This tool was developed by [Jesper Hessius](https://github.com/hessius).
- If you have any other resources you want added reach out through [surname]@me.com or submit an issue on the [github repository](https://github.com/hessius/voicehelper/issues).

## License

The source code is available on [Github](https://github.com/hessius/voicehelper).
This software is distributed freely under the GNU Affero General Public License v3.0 (GNU AGPL 3.0).
Full license statement is available [here](https://www.gnu.org/licenses/agpl-3.0.html) and in the file `LICENSE` included in the root of the repository.
This license permits reuse in any form as long as the original author and source are credited.

## Acknowledgements

The logotype used for this project is a remix of glyphs from [The Noun Project](https://thenounproject.com) by authors [Chintuza](https://thenounproject.com/chintuza/) and [Symbolon](https://thenounproject.com/symbolon/).

<link rel="stylesheet" href="/air.min.css" />
<script>
      if (location.protocol !== 'https:' && location.hostname !== 'localhost') {
        location.replace(
          `https:${location.href.substring(location.protocol.length)}`
        )
      }
</script>
∏
````
