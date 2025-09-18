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
- **width** (optional): Window width in pixels (default: 500)
- **height** (optional): Window height in pixels (default: full screen height)

#### Functionality
- Calculates positioning for the bottom-right corner of the screen
- Opens a new window with `redirector.html` at the calculated position
- Passes through any URL parameters to the redirector
- Closes itself immediately after opening the new window

#### Usage Example
```
opener.html?url=https://example.com&width=600&height=400
```

### redirector.html

The `redirector.html` file positions itself to the right edge of the screen and redirects to a target URL. It's designed to work seamlessly with DMO's step-by-step commands.

#### Parameters
- **url** (required): The target URL to redirect to. Must be a valid HTTP/HTTPS URL
- **width** (optional): Window width in pixels (default: 600)
- **height** (optional): Window height in pixels (default: full screen height)

#### Functionality
- Positions the window to the right edge of the screen
- Validates that the target URL is a valid HTTP/HTTPS URL for security
- Redirects to the target URL after a brief delay (100ms)
- Maintains the positioned window layout during the redirect

#### Usage Example
```
redirector.html?url=https://example.com&width=800
```

#### Security Note
The redirector validates URLs using a regex pattern to ensure only HTTP and HTTPS protocols are allowed, preventing potential security issues from javascript: or other protocol schemes.

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
