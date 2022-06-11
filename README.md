# DMO Helper

## What is This?

This is a service that acts as an intermediate page to facilitate search through step by step commands executed through Dragon Medical One (DMO).

## Why?

When executing step by step commands that go beyond the EHR, specifically to interact with websites, commands are prone to breaking due to layout changes. This is due to the engine navigating the UI using a fixed set of instructions, e.g. tabbing through a set of elements to reach a search where text can be entered. If the design of the webpage changes, an ad is inserted or a cookie noticed introduced the number of tab button presses needed to hit the correct field changes, and thus the script fails. Even when without layout changes the focus jumping incurred by repeating tab presses is visible to the user and can be jarring. Overall this equals very bad UX. But this service fixes that.

## How it works

By design this service contains no UI, this is in order to be basically invisible to the end user - it should "just work".
The service takes arguments from two fronts:
a) in the form of URL search params e.g. domain.com?foo=bar
b) from pasted text

When accessing this page with a url param (see below for options) specifying a site that either matches a predefined search engine or is a url the service can receive pasted text. The input from the URL parameters and the pasted text is combined to generate a complete search URL, to which the user is redirected. While the process might sound complex this happens (almost) instantaneously and is (almost) invisible to the user.

An additional feature to note is the ability to translate text strings so that e.g. a Swedish string can be converted into English to enable searching an English language resource.

## How to use with DMO

1. Create a new step by step command "search X" (X being the database)
2. Add the commands:
   a. Keyboard shortcut: Ctrl-C
   b. Run program: Web browser of choice (e.g. Edge) with the argument https://hessius.github.io/dmohelper/?site=[INSERT SITE HERE]&lang=[INSERT LANGUANGE HERE (OPTIONAL)]
   c. Wait: e.g. 3000 ms to let the browser load, the website is minimal enough to load nearly instantaneously
   d. Keyboard shortcut: Ctrl-V
3. Invoke the shortcut by saying "Select [TEXT]" and then "Search X"

## Parameters

### Sites

| Name             | Param | Param alt       | Param alt 2 | Description                                                                         | Language |
| ---------------- | ----- | --------------- | ----------- | ----------------------------------------------------------------------------------- | -------- |
| Bing             | b     | bing            |             | General purpose search engine                                                       | Any      |
| FASS             | fass  |                 |             | Pharmaceutical information database                                                 | Swedish  |
| Google           | g     | google          |             | General purpose search engine                                                       | Any      |
| ICD.nu           | icd   |                 |             | Coding search engine, covers diagnostic as well procedure codes                     | Swedish  |
| Internetmedicin  | im    | internetmedicin |             | Swedish equivalent of Uptodate                                                      | Swedish  |
| Janusinfo        | ji    | janusinfo       |             | Swedish National collection of emergency medicine resources and pharmaceutical info | Swedish  |
| Läkemedelsboken  | lmb   | lakemedelsboken |             | Pharmaceutical guidelines                                                           | Swedish  |
| Praktisk medicin | pm    | praktiskmedicin |             | Large collection of medical information for primary care                            | Swedish  |
| Uptodate         | u2d   | utd             | uptodate    | Large collection of medical information                                             | English  |
