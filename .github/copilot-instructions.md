# VoiceHelper

VoiceHelper is a static web application that provides voice-enabled search capabilities for Dragon Medical One (DMO). It acts as an intermediate service that accepts URL parameters and pasted text to redirect users to search results on medical databases and other websites.

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Bootstrap and run the repository:
- `cd /home/runner/work/voicehelper/voicehelper` - Navigate to repository root
- `python3 -m http.server 8080` - Start local web server on port 8080. Takes <1 second to start.
- Access the application at `http://localhost:8080`

### Core Application Files:
- `index.html` - Main application entry point with domain logic and search functionality
- `UI.html` - Domain selector interface showing available search engines organized by language
- `docs.html` - Complete documentation page (same content as README.md but styled)
- `README.md` - Project documentation and usage instructions
- Static assets: `air.css`, `air.min.css` - Custom CSS framework for styling

### No Build System:
- This is a pure HTML/CSS/JavaScript static website
- NO package.json, NO npm install, NO build process required
- NO test suite - validation must be done manually
- NO linting tools - code style is maintained manually

## Validation

### ALWAYS manually validate any changes by running the application scenarios below:

#### Test Scenario 1: Basic Homepage (No Parameters)
- Navigate to `http://localhost:8080/`
- Expected: Gray blank page with title "Voice Helper Ready"
- Console should show: "site param is empty"

#### Test Scenario 2: Valid Site Parameter
- Navigate to `http://localhost:8080/?site=g`
- Expected: Same gray blank page, ready to accept pasted input
- Console should NOT show errors about missing site param

#### Test Scenario 3: Domain Selector UI
- Navigate to `http://localhost:8080/UI.html`
- Expected: Clean interface with search textbox and domain buttons organized by language (Swedish, English, General)
- Click any domain button (e.g., "Google") - should populate the textbox and focus it
- Type search term and press Enter - should show alert dialog with the search term

#### Test Scenario 4: Documentation Page
- Navigate to `http://localhost:8080/docs.html`
- Expected: Styled documentation page with table of contents, domain table, examples
- All internal links should work for navigation

#### Test Scenario 5: Redirect Functionality (Advanced)
- Navigate to `http://localhost:8080/?site=g`
- Simulate paste event with JavaScript in browser console: `document.dispatchEvent(new ClipboardEvent('paste', {clipboardData: new DataTransfer()}));`
- Expected: Should trigger search logic (may not fully work due to clipboard security restrictions in testing environment)

### Always take screenshots when making UI changes to verify visual changes work correctly.

## Common Tasks

### Repository Structure:
```
.
├── .github/
│   └── copilot-instructions.md  # This file
├── .gitignore                   # Only excludes .vscode/
├── CNAME                        # GitHub Pages domain configuration
├── LICENSE                      # GNU AGPL 3.0 license
├── README.md                    # Project documentation
├── README.html                  # Generated HTML version of README  
├── index.html                   # Main application entry point
├── UI.html                      # Domain selector interface
├── docs.html                    # Documentation page
├── air.css                      # CSS framework source
├── air.min.css                  # Minified CSS framework
├── air.min.css.map             # CSS source map
├── opener.html                  # Utility page
├── redirector.html              # Utility page
└── logo/                        # Brand assets
    ├── 2x/                      # 2x resolution PNGs
    ├── 4x/                      # 4x resolution PNGs  
    └── SVG/                     # Vector logos
```

### Key Architecture Concepts:
- **Domain List**: Predefined search engines defined in JavaScript object in `index.html` (lines ~245-400)
- **URL Parameters**: `site` (required) and `lang` (optional) control search behavior
- **Paste Handling**: Main functionality relies on clipboard paste events to trigger searches
- **Translation**: Optional language translation feature for cross-language searches
- **Invisible UI**: By design, the main interface is intentionally minimal/blank to be "invisible"

### Adding New Search Domains:
- Edit the `domainList` object in `index.html`
- Structure: `paramname: { url: 'search_url', name: 'Display Name', lang: 'language_code' }`
- Also update the domain list in `UI.html` if you want it to appear in the selector
- Test by accessing `http://localhost:8080/?site=newparam`

### File Relationships:
- `README.md` content is duplicated in `docs.html` as styled HTML
- `index.html` and `UI.html` share similar domain definitions but different presentation
- `air.css` provides the minimal styling framework used across pages

## Deployment

This application is deployed to GitHub Pages and accessible at `voicehelper.io`. The `CNAME` file configures the custom domain.

## No CI/CD Pipeline

There are no automated tests, builds, or deployment pipelines. Changes are deployed by pushing to the main branch, which automatically updates GitHub Pages.

## Key Limitations

- **External Dependencies**: Application loads Google Fonts and external resources that may be blocked in testing environments
- **Clipboard Security**: Paste functionality requires user interaction and may not work in automated testing
- **No Error Handling**: Minimal error handling in the JavaScript code
- **No Validation**: No input sanitization or validation beyond basic JavaScript checks

## Always Remember

- Test every change manually using the validation scenarios above
- Take screenshots of UI changes for documentation
- This is a production application used by medical professionals - ensure changes don't break core functionality
- The "invisible" user experience is intentional - users should not see complex UI during normal operation