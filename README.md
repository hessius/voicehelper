<img src="https://voicehelper.io/logo/logo.svg" height=150>

# Voice Helper

1. [Voice Helper](#voice-helper)
   1. [What is This?](#what-is-this)
   2. [Why?](#why)
   3. [How it works](#how-it-works)
   4. [How to use with DMO](#how-to-use-with-dmo)
   5. [Parameters](#parameters)
      1. [Predefined domains](#predefined-domains)
   6. [Examples](#examples)
   7. [Other info](#other-info)

## What is This?

This is a service that acts as an intermediate page to facilitate search through step by step commands executed through Dragon Medical One (DMO).

## Why?

When executing step by step commands that go beyond the EHR, specifically to interact with websites, commands are prone to breaking due to layout changes. This is due to the engine navigating the UI using a fixed set of instructions, e.g. tabbing through a set of elements to reach a search where text can be entered. If the design of the webpage changes, an ad is inserted or a cookie noticed introduced the number of tab button presses needed to hit the correct field changes, and thus the script fails. Even when without layout changes the focus jumping incurred by repeating tab presses is visible to the user and can be jarring. Overall this equals very bad UX.

_But this service fixes that._

This service "invisibly" and near instantaneously converts provided parameters to a complete search url and redirects the user to the results page - thus enabling the user to search across a multitude of websites by voice and unlocking some of the power of the step by step commands that are available in DMO.

## How it works

By design this service contains no UI, this is in order to be basically invisible to the end user - it should "just work".
The service takes arguments from two fronts:

**a)** in the form of URL search params e.g. domain.com?foo=bar

**b)** from pasted text

To use the service, when accessing this page you need to to provide a url parameter specifying a site that either matches a predefined search engine (see below) or is a valid url. If accessed with a valid url parameter the service can receive pasted text. The input from the URL parameters and the pasted text is combined to generate a complete search URL, to which the user is redirected. **While the process might sound complex this happens (almost) instantaneously and is (almost) invisible to the user.**

An additional feature to note is the ability to translate text strings so that e.g. a Swedish string can be converted into English to enable searching an English language resource.

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
| Bing             | b     | bing            |             | General purpose search engine                                                       | Any      |
| FASS             | fass  |                 |             | Pharmaceutical information database                                                 | Swedish  |
| Google           | g     | google          |             | General purpose search engine                                                       | Any      |
| ICD.nu           | icd   |                 |             | Coding search engine, covers diagnostic as well procedure codes                     | Swedish  |
| Internetmedicin  | im    | internetmedicin |             | Swedish equivalent of Uptodate                                                      | Swedish  |
| Janusinfo        | ji    | janusinfo       |             | Swedish National collection of emergency medicine resources and pharmaceutical info | Swedish  |
| Läkemedelsboken  | lmb   | lakemedelsboken |             | Pharmaceutical guidelines                                                           | Swedish  |
| MD Calc          | mdc   | mdcalc          |             | Clinical calculators                                                                | English  |
| Medscape         | ms    | medscape        |             | Clinical reference (searches drugs & diseases database)                             | English  |
| Praktisk medicin | pm    | praktiskmedicin |             | Large collection of medical information for primary care                            | Swedish  |
| Uptodate         | u2d   | uptodate        | utd         | Large collection of medical information                                             | English  |
| Vårdhandboken    | vhb   | vardhandboken   |             | Practical instructions re: medical procedures                                       | Swedish  |

## Examples

Here are two examples of valid url parameters both querying Google:

1. https://voicehelper.io/?site=g
2. https://voicehelper.io/?site=https://www.google.com/search?q=

Here are two examples of valid url parameters querying MD Calc, one ready for an English query (no translation needed, thus no language parameter passed) and one ready for a query in German:

1. https://voicehelper.io/?site=mdc
2. https://voicehelper.io/?site=mdc&lang=de

## Other info

- This tool was developed by [Jesper Hessius](https://github.com/hessius).
- If you have any other resources you want added reach out through [first name].[surname]@nuance.com.

<link rel="stylesheet" href="/air.min.css" />
<script>
      if (location.protocol !== 'https:' && location.hostname !== 'localhost') {
        location.replace(
          `https:${location.href.substring(location.protocol.length)}`
        )
      }
</script>
