# findthemasks.com (used by getusppe.org)

This repo hosts the code for findthemasks.com, which is also used to power the v1 backend of "give ppe" features on getusppe.org

- Stats: <https://findthemasks.com/stats.html>

## New volunteer?

Join the slack! <https://join.slack.com/t/findthemasks/shared_invite/zt-czdjjznp-p8~9oKuXtV_gn7wEBZGGoA>

- new dev? please look at issues and comment on something to grab it!
    - Check out the [Getting Started](getting_started.md) doc
- new data moderator? Join the slack and come to #data!
- not either? The most useful contribution is identifying more drop off locations and plugging them into the form linked on the public website, so if you don't see an issue here that calls to you, please work on that! Advice on making calls is in [#131](https://github.com/r-pop/findthemasks/issues/131#issuecomment-602746963)

## Current setup

- The website reads from a google sheet, generates a json blob, which is used to generate static HTML.

## Reading our data to build your own frontend

- Our data file updates every five minutes and can be read from findthemasks.com/data.json.
- If you read the json directly, you need to ignore entries without an 'x' in the first field. Otherwise, you may publish info hospitals asked to have taken down. Don't do it!
- If this sounds like too much work, then please use our:

## Embeddable widget of donation sites

- We have produced an embeddable version of our map, data and filters, without the call to action that's at the top of findthemasks.com. This was designed for getusppe.org on March 22, but can be reused by anyone.
- You can view it here: <https://findthemasks.com/give.html>
- To embed into your site, use this html snippet:

```html
<iframe style="width: 100%; height: 800px; border: none;" src="https://findthemasks.com/give.html"></iframe>
```

- We also support state specific data views, hiding the map, and hiding the filters through query params:

```html
?state={CA/WA/NY/etc}
?hide-map={true/false}
?hide-filters={true/false}
?hide-list={true/false} (also hides filters)
?hide-search={true/false} (beta)
```

All boolean parameters default to false (unless they're in beta).

So, for state-specific pages you can now use something like:
<https://findthemasks.com/give.html?state=CA&hide-map=true&hide-filters=true>
This will return just the filtered list of donations sites in California.

**Beta features:**

Since beta features are disabled by default, you can enable them via:

```
?show-search=true
```

## Current Countries
* United States - us
* France - fr
* Canada - ca

## Current Locales
* English - en
* French - fr

## Adding Countries and Locales

We use a directory structure to view country-specific datasets.

For example, `/us/give.html` will filter the map to the United States and `/fr/give.html` will filter to France.

To view translated version of a country you can pass in a locale parameter. `/us/give.html?locale=fr-FR`
will show the map of the United States in French and `/fr/give.html?locale=en-US` will show the map of France in English.

To add a new country, you need to set a few variables.
1. Get the country code from https://www.iban.com/country-codes.
2. Add the country code and a link to the donation form to `donation-form-bounce.html`. The form should
include translations for all official languages in that country.
3. Add the translated strings for all official languages in that country in `i18n.js`. As a starting point,
it is OK to launch a new language using an international variant. e.g. you can launch Canada
with `en` translations and `fr` translations, they do not need to be localized to `en-CA` and `fr-CA`.
4. Update the list of languages and countries in `countries.js` and `locales.js` and ensure that
they propagate correctly to the language and country dropdowns. In `countries.js` you should also add
the name of the string for that country's administrative region. e.g. US = "State", CA "Province",
FR = "Department", copy for who they should direct large donations to, and copy for who they should
contact if there are no donation sites near them.

## Data inflow, storage & moderation

### Intake

Currently about PPE needs is contributed by members of the public through a Google Form.  We have at least one 
form per country; for CA and CH we have one per language.  Currently this includes:
* [AT](https://docs.google.com/forms/d/e/1FAIpQLSeUzUPg9KZAQbMRCYV4ZrOI2hJ3RJ0oqp73XIIBlZjC1shGSA/viewform)
* CA
  * [CA-en](https://docs.google.com/forms/d/e/1FAIpQLSf5JAiAikzMEEw86eyjoRMH5AFlwaMrOmjjlr3vGcL5RrJt9A/viewform)
  * [CA-fr](https://docs.google.com/forms/d/e/1FAIpQLSeM2Jt5zudVG9_IxCT0pXluTs4eHq7_p3X95klGCHSSSaDEFg/viewform)
* CH
  * [CH-de](https://docs.google.com/forms/d/e/1FAIpQLSefzkaIwCM2efWDsKffOy4YBAczz3Db0uWsdJobSETC1LYExw/viewform)
  * [CH-en](https://docs.google.com/forms/d/e/1FAIpQLSdeS0Pvd2u7njuhwt-EqaSAtXUmKUPsVA47XqjGP5U1NqX2rA/viewform)
  * [CH-fr](https://docs.google.com/forms/d/e/1FAIpQLSccTTMijFuFX_qimh9YMyHjInlsv7NITLfQ-LDj61aKVIV5hw/viewform)
  * [CH-it](https://docs.google.com/forms/d/e/1FAIpQLSf9zYPc1il7Tog-tiIniYrGytZyYIL4L2lrc51BHzFgZEQcTQ/viewform)
* [DE](https://docs.google.com/forms/d/e/1FAIpQLSdB7fIY9N0OtWwUvb3kyi3R82EQGqT__7Dczhjp5u-749g_1Q/viewform)
* [ES](https://docs.google.com/forms/d/e/1FAIpQLSeSfhRSGcSuxW02Ag8WtcWyrN8sNKmh14qd7UgkYXJrSEAGYg/viewform)
* FR
  * [FR-en](https://docs.google.com/forms/d/e/1FAIpQLSewYYshU3GWARzMhy-QnguHES43hTL4AkDrlwV6XxhXMiLK0w/viewform)
  * [FR-fr](https://docs.google.com/forms/d/e/1FAIpQLScys-wlfSEwCLNa5dnJIIR7LTdh7e3fpha7SL7A2jvJok8Tog/viewform)
* [PT](https://docs.google.com/forms/d/e/1FAIpQLScbBpYJj6WTlHhdMsFjHnBGzD_Ge_quqNv42iJiUhGst1GPrg/viewform)
* [USA](https://docs.google.com/forms/d/e/1FAIpQLSfgCpK5coPVFC6rJrE7ZhimiZuDoEaL6fo6gYqxsN_FIpJZhg/viewform)

### Storage

Currently the data about PPE needs is stored in Google Sheets spreadsheets (one per country). Data from the forms
(described above) automatically feeds into these sheets.

* [AT](https://docs.google.com/spreadsheets/d/19gKSyKmT4yU7F32R3lBM6p0rmMJXusX_uMYDq1CMTIo/edit#gid=1604454713)
* [CA](https://docs.google.com/spreadsheets/d/1STjEiAZVZncXMCUBkfLumRNk1PySLSmkvZuPqflQ1Yk/edit#gid=465803002)
* [CH](https://docs.google.com/spreadsheets/d/1mFbEzrWW8XLfrkAL0eCzGd1pCVNl6-QUxkoeubtdbPI/edit#gid=1220865648)
* [DE](https://docs.google.com/spreadsheets/d/1qiR4JRvPrbOwlPnEXCoUFWpfZtV9xadjBTCOhVy-dJM/edit#gid=567379797)
* [ES](https://docs.google.com/spreadsheets/d/1S3FO5gmXUvQdsGXjC0hBSUxBJZoaUzy1ctylTjUGOlM/edit#gid=1355538271)
* [FR](https://docs.google.com/spreadsheets/d/1YGWlGPOfJFEsUP6VTFCVohlsHxKMYA5HppatbLwNBVk/edit#gid=1291956074)
* [PT](https://docs.google.com/spreadsheets/d/1QnyjUUBT_P476dEl0WfQwVnW15Ie7ogty7DiOkMhHLo/edit#gid=2061172965)
* [US](https://docs.google.com/spreadsheets/d/1GwP7Ly6iaqgcms0T80QGCNW4y2gJ7tzVND2CktFqnXM/edit#gid=2027792507&fvid=115388765)

### Moderation

Moderation is done by volunteers in accordance with the guidance laid out in the findthemasks
[wiki](https://github.com/findthemasks/findthemasks/wiki/Data-Quality-Procedure).


## Directory structure

- `/public` - The client-side code for the website. Currently has some symlinks to legacy file locations.
- `/functions` - The cloud function used to generate data.json. Not needed for frontend work.

## Firebase

### Basic architecture
Firebase is used to pull data from our moderated datastore and then generate a data.json. There is
a production environment [findthemasks](https://console.firebase.google.com/project/findthemasks/overview)
and a dev environment [findthemasks-dev](https://console.firebase.google.com/project/findthemasks-dev/overview).

The setup uses cloud functions to provide http endpoints, cloud-storage to keep the generated results,
and the firebase realtime database (NOT firestore) to cache oauth tokens.

Adding an oauth token requires hitting the `/authgoogleapi?sheetid=longstring` on the
cloud-function endpoint and granting an OAuth token for a user that has access to the sheet.

### How to deploy
- Install the [firebase cli](https://firebase.google.com/docs/cli?hl=vi) for your platform.
- Do once
  - `firebase login`
  - `firebase use --add findthemasks-dev`
  - `firebase use --add findthemasks`
  - `cd functions; npm install`  # Note you need node v8 or higher. Look a [nvm](https://github.com/nvm-sh/nvm#installing-and-updating)
- Switch deployment envrionments `firebase use [findthemasks or findthemasks-dev]`
- Deploy the cloud function. `cd functions; npm run deploy`

### How to set config variables.
Secrets and configs not checked into github are specified via cloud function configs.

To set a config:
```
firebase functions:config:set findthemasks.geocode_key="some_client_id"
```

In code, this can be retrieved via:
```
functions.config().findthemasks.geocode_key
```

In get all configs:
```
firebase functions:config:get
```

The namespace can be anything. Add new configs to the `findthemasks` namespace.

### How to locally develop
Firebase comes with a local emulation environment that lets you live develop
against localhost. Since we are using firebase configs, first we have to snag
the configs from the environment. Do that with:

```
firebase functions:config:get > .runtimeconfig.json
```

Next generate a new Firebase Admin SDK private key here:
  https://console.firebase.google.com/project/findthemasks-dev/settings/serviceaccounts/adminsdk

And save it to `service_key.json`

Then start up the emulator. Note this will talk to the production firebase
database (likely okay as the firebase database is just storing oauth tokens).

```
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/service_key.json
firebase emulators:start --only functions
```

This will create localhost versions of everything. Cloud functions should be on
[http://localhost:5001](http://localhost:5001) and `console.log()` messages will
stream to the terminal.

There is also a "shell"

```firebase functions:shell```

that can be used, but running the emulator and hitting with a web browser
is often easier in our simple case.

## Scripts that edit the spreadsheet (Apps Script)
There are currently 2 scripts that automatically update the spreadsheet, and one
that backs it up:

* fillInGeocodes: fills in the lat/lng column based on the address in the "address" column.
  (Note that the "address" column is defined as the column that has "address" in row 2.)
  Uses Google Maps geocoding API.  Currently runs once/minute.
* createStandardAddress: fills in the "address" column based on the data in the "orig_address",
  "city", and "state" columns.  Uses Google Maps geocoding API. Currently runs once/minute.
* backupSheet: makes a timestamped copy of the sheet.  Currently runs once every 2 hours.

There are a few important things to know about these scripts:

* They are visible by navigating to tools > script editor from the Google Sheet.
* They can be run by anyone who has edit permission to the Google Sheet.
* Triggers (automation) can be set up by navigating to Edit > Edit current project's triggers.
* The dev-owner of each sheet should set up a trigger for each of the 3 scripts.  (US spreadsheet
  dev owner is @susanashlock's gmail).
* The scripts are run using quota of the user the user that runs them.
* Each Gmail user has a fixed amount of geocoding quota per day.  This quota is somewhere
  around 250 calls per day.  @susanashlock's account has 'special' quota.  We're not
  sure exactly what it is, but is sufficient to support ~1000 calls per day.


## Thanks

- The "Face With Medical Mask" favicon is used with thanks to
   [favicon.io](https://favicon.io/emoji-favicons/face-with-medical-mask/) which
   provides pre-generated favicon packages using
   [Twemoji](https://twemoji.twitter.com/). Twemoji graphics are licensed
   [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- [Feather icon collection](https://github.com/feathericons/feather), licensed under [MIT License](https://github.com/feathericons/feather/blob/master/LICENSE).
