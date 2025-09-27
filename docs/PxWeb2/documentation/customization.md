# Customization

PxWeb comes prebuilt with configuration for tables in English, a set of fonts
colors and icons. This guide help you customize PxWeb to your needs.

- Change which language that should be available and which one is the default language
  if a language is not specifyed.
- Add a new language or change translations of an language.
- Change colors on the diffrent elements.
- Change the coner radius on diffrent elements.
- Change the fonts.
- Change icons.
- And more...

Bellow follows a list of **How to** on how to acoplich diffrent customizations.

## Folder structure

When you unzip the zip-file of the official PxWeb 2.0 release. It contains the following
directory structure. It is good to understand it to be able to know where you
have to do your modification.

```sh
root
├── index.html 
├── web.config # (1)!
├── assets # (2)!
├── config
│   └── config.js
├── content
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
│   ├── topicIconMap.json
│   └── topic
│       ├── icon1.svg
│       ├── icon2.svg
│       ├── ...
│       └── small/
│           ├── icon1.svg
│           ├── icon2.svg
│           └── ...
├── images
│   └── logo.svg
├── locales
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

1. This is a `web.config` that ships with PxWeb tha you can use as a starting point
   if you are serving PxWeb from an Microsoft Internet Information Server (IIS).
2. This folder contains all the javascript and css that make up most of the logic
   of PxWeb. You should not have to edit anything in this folder.

## How to´s

### How to change the API-endpoint

PxWeb get all the data that it needs from an PxWebAPI 2.0 instance. To show your
data in PxWeb you will have the set the `apiUrl` in `config/config.js` to the
base url of your API site. Make user to omit the last backslash it will be added
automatically.

### How to run PxWeb under a subpath

Lets say you have a web server that is bound to the domain `www.mysite.org` and you
whant to have PxWeb served in a subpath `pxweb` like `www.mysite.org\pxweb` and
that you do not have a proxy infron of it instead you will solve it on the web
server. You will then have to set the `baseApplicationPath` in `config/config.js`
and also set the `href` attribute for the `base` element in `index.html` to be

### How to set the limit of how big tables you can view in PxWeb

You can limit how big tables you are able to view in PxWeb by setting
`maxDataCells` in `config/config.js` file to the number of cell (data points)
that can be displayed.
!!! note
    Notice that the API endpoint also has a separat setting for this. You should
    not set the value larger than the API because that can lead to strange
    behaivour. The reccomended thing is to set it to be the same as for the API.

### How to change the default characters for missing values

There are a set of characters that are displayed in the table when data is missing.
You can set the characters in the array called `specialCharacters` in `config/config.js`.

### How to exclude some variables from showing up in the variables filter

In some cases you have variables that you do not whant to be included in the variables
filter on the first page. Then you can add the to the `variableFilterExclutionList`
section in `config/config.js`. There is one sub section for each language by the
[two letter](https://en.wikipedia.org/wiki/ISO_639-1) language code.
!!! note
    Notice that you have to add the variable name. If your site have multiple
    languages then you should add it for each language.

### How to set the language(s) of my site

You can configure your site to support one or more language. This is don in the
`language.supportedLanguages` section in `config/config.js`. Just add another language
to the list. To set the main or default language of your site set the `language.defaultLanguage`
to the [two letter language code](https://en.wikipedia.org/wiki/ISO_639-1) of the
language you want to have as main language.

!!! tip
    Use the same set of language as you have configured for the API.

!!! tip
    The `fallbackLanguage` is used by the localization system if no translation
    exist for a particular text for a specific language the text for the
    `fallbackLanguage` is used.

### How to a a completly new language

If you want to add a completly new language to PxWeb, you can do so easely by creating
a subfolder to `locales` with the name set to the [two letter  language code](https://en.wikipedia.org/wiki/ISO_639-1)
Then take a copy of the file `locales/en/translate.json` and place it in your
newly create folder. Then change the texts inside the copy.

### How to change the fonts

If you ned to change the fonts simply replace the fints in the folder `fonts` with
your font.

### How to change the text

If text is not correctly translated or you just whnat to change it to make it
better. You can do so by change it in the `translation.json` file found under
the `locales` folder and in the sub folder of the specific language. There is
a subfolder for each [two letter  language code](https://en.wikipedia.org/wiki/ISO_639-1).
E.g. to change the english texts you should do that in the `locales/en/translate.json`.
The `translation.json` file has a structure that makes it easy to see where the
text is displayed in PxWeb.

### How to hide or show the default language in the URL

You can show or hide the default language in the url by setting the
`showDefaultLanguageInPath` to either `true` or `false` in `config/config.js`

### How to change the topic icons on the startpage

The startpage displays a list of tables.  Each table can be associated with an
icon. The mapping between a table and the which icon to use is done by the tables
`SOBJECT-CODE` and the name of the icon file. 
This mapping is defined in `/public/icon/topic/topicIconMap.json`

If you want to add new icons, they must be placed in both of the following folders:

- `/public/icon/topic/` — SVG file with dimensions **36x36**
- `/public/icon/topic/small/` — SVG file with dimensions **28x28**

> If you need to change an existing topic ID or add a new mapping to a new SVG
  icon, this must also be done in the `/public/icon/topic/topicIconMap.json` file.

### How to changes the corner rounding of the visual elements

The are a couple of css variables in `theme/variables.css` that you can change to
your likings.

The following variables can be changed.

- `--px-border-radius-xxsmall`
- `--px-border-radius-xsmall`
- `--px-border-radius-small`
- `--px-border-radius-medium`
- `--px-border-radius-large`
- `--px-border-radius-xlarge`

!!! tip
    Do not change the`--px-border-radius-none` and `--px-border-radius-full` in
    `theme/variables.css` if you are uncertain in what you are doing.

### How to change the text and related links on the start page

TODO

### How to change the links in the footer

TODO

### How to change the colors

PxWeb´s desing system builds on design token. This guide is not the right place
to explain that concept in deepth but basiclay there are different type of tokens
in our case primitive tokens and system token. The primitive are for instance a
brand color. While the system token are more descriptive e.g. surface subtle color.
The system token build upon the primitive tokens.

The colors are defined in `theme/variables.css` as css variables. The once you
would want to change starts with:

- `--px-color-brand-`
- `--px-color-accent-`
- `--px-color-neutral-`

You should not change the system tokens.

[Leonardo](https://leonardocolor.io/theme.html#)

#### Colors mappings

Bellow is a list of primitive color tokens and a mapping to the system token that
uses it. For the name of the system token you can almost see exactly where it is
used.

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
