# Customization

PxWeb comes prebuilt with configuration for tables in English, a set of fonts
colors and icons. This guide help you customize PxWeb to your needs.

You can:

- Configure available languages and set a default fallback.
- Add a new language or adjust existing translations.
- Change colors (design tokens).
- Adjust corner radii of UI elements.
- Replace fonts.
- Change logo and favicon
- Change topic icons.
- Limit table sizes.
- Control how languages appear in URLs.
- Hide variables from the variable filter.
- Customize start page text and footer links (examples included below).
- Define characters used for missing values.

Below you will find a set of practical **How to** sections.

---

## Folder structure

When you unzip the official PxWeb 2.0 release you get a structure like:

```sh
root
├── index.html               # (1)!
├── web.config               # (2)!
├── assets                   # (3)!
├── config
│   └── config.js            # (4)!
├── content                  # (5)!
│   ├── ar
│   │   └── content.json
│   ├── en
│   │   └── content.json
│   ├── no
│   │   └── content.json
│   └── sv
│       └── content.json
├── fonts                    
│   ├── PxWeb-font-400.ttf
│   ├── PxWeb-font-500.ttf
│   └── PxWeb-font-700.ttf
├── icons
│   ├── topicIconMap.json    # (6)!
│   └── topic
│       ├── icon1.svg
│       ├── icon2.svg
│       ├── ...
│       └── small/
│           ├── icon1.svg
│           ├── icon2.svg
│           └── ...
├── images
│   ├── favicon.svg
│   ├── favicon-darkmode.svg
│   └── logo.svg
├── locales                  # (7)!
│   ├── ar
│   │   └── translation.json
│   ├── en
│   │   └── translation.json
│   ├── no
│   │   └── translation.json
│   └── sv
│       └── translation.json
└── theme
    └── variables.css

```

1. `index.html` is the page that bootstraps PxWeb.
2. `web.config` is a starting point if you host on Microsoft IIS.  
3. `assets` contains the compiled JavaScript/CSS for PxWeb. You normally do not
    edit anything inside this folder.
4. `config.js` the main config file for PxWeb.
5. `content` a folder containg content customizations e.g. links in the footer.
6. `topicIconMap.json` is a file where mapping between tables `SUBJECT-CODE` and
    icons are made.
7. `locales` a folder where translated text are placed.

---

## How to’s

### Change the API endpoint

PxWeb retrieves table metadata and data from a PxWeb API 2.0 instance.  
Open `config/config.js` and set `apiUrl`:

```js hl_lines="11"
  window.PxWeb2Config = {
    language: {
      supportedLanguages: [
        { shorthand: 'en', languageName: 'English' }
      ],
      defaultLanguage: 'en',
      fallbackLanguage: 'en',
      showDefaultLanguageInPath: true,
    },
    baseApplicationPath: '/',
    apiUrl: "https://your.api.server/PxWeb/api/v2",
    maxDataCells: 100000,
    specialCharacters: ['.', '..', ':', '-', '...', '*'],
    variableFilterExclusionList: {
      en: [
        'observations',
        'year',
        'quarter',
        'month',
        'every other year',
        'every fifth year',
      ]
    },
    homePage: {
      en: '', 
    },
  };
```

Omit a trailing slash, one is added automatically.

### Run PxWeb under a subpath

If you serve PxWeb from e.g. `https://www.mysite.org/pxweb` (without an upstream
reverse proxy):

1. Set `baseApplicationPath` in `config/config.js`:

   ```js
   baseApplicationPath: "/pxweb/"
   ```

2. In `index.html`, adjust the `<base>` tag:

   ```html
   <base href="/pxweb/">
   ```

3. Ensure the web server rewrites (if needed) static file requests correctly to
   that subpath.

### Limit how large tables can be viewed

Set `maxDataCells` in `config/config.js`. Example:

```js
maxDataCells: 500000
```

!!! note "API limitation"
    The API has its own limit. Do not set the UI limit higher than the API’s
    backend limit to avoid inconsistent behavior. Usually they should match.

### Change the default characters for missing values

Edit the `specialCharacters` array in `config/config.js`:

```js
specialCharacters: ['.', '..', ':', '-', '...', '*']
```

These are displayed when a data point is missing or confidential.

### Exclude variables from the variable filter

To hide certain variables from appearing in the initial variable filter, add
their (localized) names under `variableFilterExclutionList` per language in `config/config.js`:

```js
variableFilterExclutionList: {
  en: ["Contents"],
  sv: ["Innehåll"]
}
```

Repeat for every supported language.

### Configure site languages

In `config/config.js`:

```js hl_lines="2 3 4 5 6 7 8 9 10"
  window.PxWeb2Config = {
    language: {
      supportedLanguages: [
        { shorthand: 'en', languageName: 'English' },
        { shorthand: 'sv', languageName: 'Svenska' }
      ],
      defaultLanguage: 'sv',
      fallbackLanguage: 'en',
      showDefaultLanguageInPath: true,
    },
    baseApplicationPath: '/',
    apiUrl: "https://your.api.server/PxWeb/api/v2",
    maxDataCells: 100000,
    specialCharacters: ['.', '..', ':', '-', '...', '*'],
    variableFilterExclusionList: {
      en: [
        'observations',
        'year',
        'quarter',
        'month',
        'every other year',
        'every fifth year',
      ]
    },
    homePage: {
      en: '', 
    },
  };
```

- `supportedLanguages`: The active languages (must match those available in the API).
- `defaultLanguage`: The primary language.
- `fallbackLanguage`: Used if a translation key is missing for the active language.
- `showDefaultLanguageInPath`: If `true`, URLs include the default language code.

### Add a completely new language

1. Create a folder under `locales/` using the ISO 639‑1 two-letter code (e.g. `fr`).
2. Copy `locales/en/translation.json` into `locales/fr/translation.json`.
3. Translate the values (keep keys intact).
4. Add `"fr"` to `supportedLanguages`.
5. (Optional) Add localized content under `content/fr/content.json` if your
   content system uses it.

### Change fonts

Replace the font files in the `fonts` directory. Keep file names.

Make sure you have the proper license to self-host fonts.

### Change logo and favicon
The svg **must** include viewbox and width/height attributes for it to be rendered correctly.

To change the logo/favicon in PxWeb replace svgs in the image folder. The names must be the same.

For image replace `images/logo.svg` with your own logo.
Replace `images/favicon.ico` / `images/favicon-darkmode.svg` with your own favicon.

### Change logo URL
By default you will come to the PxWeb start page when clicking the logo. However, it is possible to configure this. You can configure a new logo URL per language by editing `homePage` in `config/config.js`:

```js
  homePage: {
    en: 'https://www.mypxweb.com',     // Set to your English homepage URL
    sv: 'https://www.mypxweb.com/sv/', // Set to your Swedish homepage URL
  },
```

### Adjust or fix text / translations

Edit the relevant `locales/<lang>/translation.json`.  
Keys reflect where in the UI a string is used. Only modify values.  

??? tip "Example snippet"
    ```json
    {
      "common": {
        "title": "PxWeb 2.0",
        "header": {
          "logo": "PxWeb 2.0",
          "logo_alt": "To the front page",
          "language_selector": "Språk/Language"
        },
      }
    }  
    ```

### Change date format in translation file

Edit the relevant `locales/<lang>/translation.json` file.  
We have two date formats defined under `date` in translation files - `simple_date` and `simple_date_with_time`. 

Change the format options as needed. **PxWeb2** uses `Intl.DateTimeFormat` for date formatting. See documentation for options here: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat

Here is an examples configuration used for english and norwegian:

??? tip "english date formats"
    ```json
    "date": {
      "simple_date": "{{value, datetime}}",
      "simple_date_with_time": "{{value, datetime(year: 'numeric'; month: 'numeric'; day: 'numeric'; hour: 'numeric'; minute: 'numeric')}}"
    }
    ``` 

Example of how `simple_date_with_time` for english is displayed in PxWeb2 UI:
`2/21/2025, 8:00 AM`

??? tip "norwegian date formats"
    ```json
    "date": {
      "simple_date": "{{value, datetime(day: '2-digit'; month: '2-digit'; year: 'numeric')}}",
      "simple_date_with_time": "{{value, datetime(year: 'numeric'; month: '2-digit'; day: '2-digit'; hour: 'numeric'; minute: 'numeric')}}"
    }
    ```

Example of how `simple_date_with_time` for norwegian is displayed with two digit configuration in PxWeb2 UI:
`21.02.2025, 08:00`

### Override separators in number formatting per language
Edit the relevant `locales/<lang>/translation.json` file.
We have defined multiple number formatting rules under `number` in translation files. Here are some expamples of number formats that are used:

??? tip "number formats"
    ```json
    "number": {
        "simple_number": "{{value, pxNumber}}",
        "simple_number_with_zero_decimal": "{{value, pxNumber(minimumFractionDigits: 0; maximumFractionDigits: 0;)}}",
        "simple_number_with_one_decimal": "{{value, pxNumber(minimumFractionDigits: 1; maximumFractionDigits: 1;)}}",
        "simple_number_with_two_decimals": "{{value, pxNumber(minimumFractionDigits: 2; maximumFractionDigits: 2;)}}",
      }
    ```

- `simple_number`: Default number formatting.
- `simple_number_with_zero_decimal`: Number formatting with no decimal places.
- `simple_number_with_one_decimal`: Number formatting with one decimal place.
- `simple_number_with_two_decimals`: Number formatting with two decimal places.

The group and decimal separators are determined by the language locale. However, if you want to customize the number formatting further, you can add additional options to the `pxNumber` formatter.

To override decimal add `decimalSeparator` to the `number_format` object. Likewise, to override group separator add `thousandSeparator` to the `number_format` object.

If you need to add space as a group separator, you add the value `nbsp` for non-breaking space or `nnbsp` for narrow non-breaking space, e.g. `thousandSeparator: 'nbsp'`.

Here is an example of how to override both decimal and group separators for simple_number_with_two_decimals. Use `,` as decimal separator and non-breaking space as group separator:
```json
"number": {
    "simple_number_with_two_decimals": "{{value, pxNumber(minimumFractionDigits: 2; maximumFractionDigits: 2; decimalSeparator: ','; thousandSeparator: 'nbsp';)}}",
  }
```

### Hide or show the default language in the URL

Toggle:
```js hl_lines="8"
  window.PxWeb2Config = {
    language: {
      supportedLanguages: [
        { shorthand: 'en', languageName: 'English' }
      ],
      defaultLanguage: 'en',
      fallbackLanguage: 'en',
      showDefaultLanguageInPath: true, // or false
    },
    baseApplicationPath: '/',
    apiUrl: "https://your.api.server/PxWeb/api/v2",
    maxDataCells: 100000,
    specialCharacters: ['.', '..', ':', '-', '...', '*'],
    variableFilterExclusionList: {
      en: [
        'observations',
        'year',
        'quarter',
        'month',
        'every other year',
        'every fifth year',
      ]
    },
    homePage: {
      en: '', 
    },
  };
```

in the `language` section of `config/config.js`.

### Change topic icons on the start page

Each table can have an associated icon. Mapping is done through the table’s
`SOBJECT-CODE` in:

```bash
/icons/topicIconMap.json
```

Add mapping between subject code and icon file like:

```json
{
  "ED": "education.svg",
  "HE": "health.svg"
}
```

If you whant to user your own icons add them in both:

- `/icons/topic/` (SVG 36×36)
- `/icons/topic/small/` (SVG 28×28)

### Change corner rounding (border radii)

Edit these CSS variables in `theme/variables.css`:

- `--px-border-radius-xxsmall`
- `--px-border-radius-xsmall`
- `--px-border-radius-small`
- `--px-border-radius-medium`
- `--px-border-radius-large`
- `--px-border-radius-xlarge`

Do not alter `--px-border-radius-none` or `--px-border-radius-full` unless you
know the side effects (they are used for logical extremes).

### Change the text and related links on the startpage and in the footer

The `content.json` file defines customizable text and links that appear in
**PxWeb’s user interface** This file allows you to configure localized UI
content: you can add or change the **breadcrumb navigation** on the start page,
add a **detailsSection** after the ingress on the start page, or update the
**footer** content for the application.

```sh
root/content
├── ar
│   └── content.json
├── en
│   └── content.json
├── no
│   └── content.json
└── sv
    └── content.json
```

- Each language folder contains its own `content.json`.
- The active language determines which file is loaded at runtime.
- This makes it possible to have different breadcrumbs, footer links, or details
  text depending on the selected language.

**Structure overview:**

```json
{
  "startPage": {
    "breadCrumb": {
      "enabled": false,
      "items": []
    },
    "detailsSection": {
      "enabled": true,
      "detailHeader": "More about PxWeb",
      "detailContent": [
                {
          "textBlock": {
            "header": "When to use this section",
            "text": "This is an optional section that can be used for content that may be useful for some users, but is not essential for everyone. Key information that all users need to see should always appear in the lead paragraph."
          }
        },
        {
          "links": {
            "header": "Useful resources",
            "items": [
              {
                "text": "Example link 1",
                "url": "#"
              },
              {
                "text": "Example link 2 (open in new tab)",
                "url": "#",
                "openInNewTab": true
              }
            ]
          }
        }
      ]
    }
  },
  "footer": {
    "columns": [
      {
        "header": "This is a header",
          "links": [
            {
              "text": "Link text",
              "url": "https://your.link.here",
              "external": true
            },
            { "text": "Contact us", "url": "mailto:mail@example.com" }
          ]
      }
    ]
  }
}
```

- **startPage.breadCrumb**
  Optional breadcrumb navigation on start page. Enabled with the `enabled` flag.
  Each item contains a `label` and an `href`.
- **startPage.detailsSection** – Optional section below the page ingress. When
  `enabled` is `true`, the application renders the **`DetailsSection` component**.
  This section can contain multiple entries, and each entry may be either a
  `textBlock` (with `header` and `text`) or a `links` block (with `header` and a
  list of `items`). Links described in `items` have `text`, `url`, and an optional `openInNewTab` boolean. If `openInNewTab` is `true`, the link opens in a new browser tab.

- **footer**
  One or more footer columns with `header` and list of `links`. If links have `external` set to `true`, they automatically will have the icon for external links and will open in a new tab. See example above.

  This setup allows administrators to adjust localized content (text and links)
  for each language without modifying the application code.

### Change the colors (Design Tokens)

PxWeb’s visual styling is driven by CSS custom properties (“design tokens”).
Understanding token layers helps you customize safely.

Token categories:

1. Primitive (Base) Tokens: Raw color values (brand, accent, neutrals, semantic
   palettes). Example: `--px-color-brand-400`.
2. Semantic (System) Tokens: Contextual meaning-based tokens mapping to 
   primitives. Example: `--px-color-surface-default`, `--px-color-text-action`.
3. Component / Alias Tokens (if introduced later): Optional intermediary 
   re-mappings for specific components.

Principles:

- You normally change only primitive tokens (`--px-color-brand-*`,
  `--px-color-accent-*`, `--px-color-neutral-*`, and semantic palette roots like
  `--px-color-info-100`).
- Semantic tokens derive usability (contrast, hierarchy) from primitives. Avoid
  editing semantic tokens unless you fully control downstream impacts.
- Keep sufficient contrast (WCAG AA: 4.5:1 for normal text, 3:1 for large / UI elements).
- Maintain directional progression (e.g. `-50` lighter → `-900` darkest).

Location:
All tokens reside in `theme/variables.css`.

Typical edit workflow:

1. Inventory current brand identity (primary, secondary, accent).
2. Decide which existing scale (brand, accent, neutral) maps to your brand palette.
3. Replace primitive hex values:

   ```css
   :root {
     --px-color-brand-50:  #F5FAFF;
     --px-color-brand-100: #E3F2FE;
     --px-color-brand-200: #C6E4FD;
     --px-color-brand-400: #5BB4F6;
     --px-color-brand-600: #1D78C7;
     --px-color-brand-800: #0F3F63;
     --px-color-brand-900: #072638;
   }
   ```

4. Rebuild or reload the page; verify legibility in buttons, table headers,
   focus outlines.
5. Test states: hover (`--px-color-surface-action-hover`), active, focus
   outlines, inverted surfaces (dark backgrounds).
6. Use a tooling aid (e.g. [Leonardo](https://leonardocolor.io/theme.html)) to
   generate balanced scales if starting from one or two seed colors.
7. Run automated accessibility checks (e.g. Lighthouse, axe) to validate contrasts.

Do NOT:

- Randomly change semantic tokens like `--px-color-text-default` unless you
  intentionally override the model.
- Mix light/dark semantics arbitrarily; ensure base background tokens stay coherent.

Primitive token groups you can safely adjust:

- `--px-color-brand-*`
- `--px-color-accent-*`
- `--px-color-neutral-*`

- Semantic palettes for status:

- `--px-color-info-*`
- `--px-color-success-*`
- `--px-color-warning-*`
- `--px-color-danger-*`

Avoid altering:

- Focus ring semantic tokens unless verifying accessibility (`--px-color-border-focus-outline`).
- Action text vs. action background pairings (they are tuned for contrast).

Reference mapping (primitive → semantic usage) is documented below to understand
impact of changing a primitive.

#### Color mappings

Below is the mapping of primitive tokens to semantic tokens that consume them:

- `--px-color-base-00`
    - `--px-color-background-default`
    - `--px-color-surface-default`
    - `--px-color-border-on-inverted`
    - `--px-color-text-on-inverted`
    - `--px-color-text-on-action`
    - `--px-color-icon-on-inverted`
    - `--px-color-icon-on-action`
- `--px-color-brand-alpha-800`
    - `--px-color-surface-scrim`
- `--px-color-brand-50`
    - `--px-color-background-subtle`
    - `--px-color-surface-subtle`
- `--px-color-brand-100`
    - `--px-color-surface-moderate`
    - `--px-color-surface-action-subtle-hover`
- `--px-color-brand-200`
    - `--px-color-surface-search-string-highlight`
    - `--px-color-surface-action-subtle-active`
    - `--px-color-border-subtle`
    - `--px-color-border-divider-subtle`
- `--px-color-brand-400`
    - `--px-color-border-moderate`
    - `--px-color-border-divider`
    - `--px-color-border-hover`
- `--px-color-brand-600`
    - `--px-color-text-subtle`
- `--px-color-brand-800`
    - `--px-color-surface-inverted`
    - `--px-color-surface-action`
    - `--px-color-border-action`
    - `--px-color-border-selected`
    - `--px-color-text-action`
    - `--px-color-icon-action`
- `--px-color-brand-900`
    - `--px-color-surface-action-hover`
    - `--px-color-surface-action-active`
    - `--px-color-border-default`
    - `--px-color-border-focus-boxshadow`
    - `--px-color-text-default`
    - `--px-color-text-on-action-subtle`
    - `--px-color-icon-default`
    - `--px-color-icon-on-action-subtle`
- `--px-color-accent-300`
    - `--px-color-border-focus-outline-on-inverted`
- `--px-color-accent-500`
    - `--px-color-border-focus-outline`
- `--px-color-neutral-100`
    - `--px-color-surface-neutral-subtle`
- `--px-color-neutral-200`
    - `--px-color-surface-neutral-moderate`
- `--px-color-info-100`
    - `--px-color-surface-info-subtle`
- `--px-color-info-200`
    - `--px-color-surface-info-moderate`
- `--px-color-info-600`
    - `--px-color-border-info`
    - `--px-color-icon-info`
- `--px-color-success-100`
    - `--px-color-surface-success-subtle`
- `--px-color-success-200`
    - `--px-color-surface-success-moderate`
- `--px-color-success-600`
    - `--px-color-border-success`
    - `--px-color-icon-success`
- `--px-color-warning-100`
    - `--px-color-surface-warning-subtle`
- `--px-color-warning-200`
    - `--px-color-surface-warning-moderate`
- `--px-color-warning-600`
    - `--px-color-border-warning`
    - `--px-color-icon-warning`
- `--px-color-danger-100`
    - `--px-color-surface-error-subtle`
- `--px-color-danger-200`
    - `--px-color-surface-error-moderate`
- `--px-color-danger-600`
    - `--px-color-border-error`
    - `--px-color-icon-error`

---

## Troubleshooting

| Issue | Possible Cause | Fix |
|-------|----------------|-----|
| Language not switching | Missing folder or translation key | Verify `locales/<lang>/translation.json` exists and is valid JSON |
| Icons not showing | File name mismatch | Ensure mapping file matches actual SVG names |
| Colors look inconsistent | Edited semantic tokens directly | Revert semantic tokens; adjust primitives only |
| Table too large error | API limit lower than UI limit | Align `maxDataCells` with API config |
| Broken layout after font change | Font weights not matching | Provide correct weights or adjust CSS font-weight usage |

---

## Checklist After Customization

- [ ] All languages load without console errors.
- [ ] Color contrast passes automated tests.
- [ ] Focus outline visible on interactive elements.
- [ ] Missing value characters render distinctly.
- [ ] Icons display in both normal and small variants.
- [ ] No 404s for font or icon assets.
- [ ] Table size limit behaves as expected.

---

## Summary

Customize primarily through:

- `config/config.js` (behavior, language, API settings)
- `locales/<lang>/translation.json` (text)
- `theme/variables.css` (tokens: colors, radii, possibly fonts)
- `icons/` (topic icons)
- `index.html` (base path, structural HTML, optional static footer/start content)

Make incremental changes and test after each step for accessibility and consistency.

---
