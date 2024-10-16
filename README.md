# vorp_lang
Dynamic global and per user localization system. !! WILL BE PART OF THE VORP_LIB !!

## Features 
- Live update of translation tables without the need to restart the resource - *Experimental*
- Users can use a language selection command/UI(in the works) to select from a list of available translations and set it as their prefered language. This setting will be loaded when the user logins and loads the translations for the user prefered language
- Fallback and Backup Languaga system: when a translation key does not exist for a certain language it will default to show the Fallback/Backup language text for that particular key, ensuring you dont run in errors if somone forgets to add a key to the translation table.
- Ability to override certain translation terms by passing the language though as an aditional argument.

### Example: 
```lua
local en = {
  menu.title = "Hello"
}

local pt = {
  menu.title = "Ola"
}

lang.loadLanguage("en", en)
lang.loadLanguage("pt", pt)


lang.setLangauge("en") -- en set as the default language

print(lang.getKeyString("menu.title")) -- uses the defined language
-- output: Hello

-- as long as the language table is loaded its able to be used
print(lang.getKeyString("menu.title", "pt"))  -- gets override with the pt language table
-- output: Ola
```
# Translation Resource API Documentation

## Safeguards

### `lang.setProtectedMode(bool)`
Enabled by default, makes protected calls for catching runtime errors.

**Parameters:**
- `bool` (Boolean): Enable with `true`, disable with `false`.

**Returns:**
- Nothing

### `lang.setIsolatedMode(bool)`
Enabled by default, isolates the environment when loading a string from an external Lua file. Can be disabled for dynamic and/or procedural string generation.

**Parameters:**
- `bool` (Boolean): Enable with `true`, disable with `false`.

**Returns:**
- Nothing

### `lang.setSanitizeOutput(bool)`
Enabled by default, filters out any type not in the `sanitizeTypes` whitelist when calling `getString`.

**Parameters:**
- `bool` (Boolean): Enable with `true`, disable with `false`.

**Returns:**
- Nothing

### `lang.setSanitizeOutputFilters(filterList)`
Sets the types `getString` is allowed to output when `setSanitizeOutput` is enabled. By default, they are `string`, `number`, and `table`.

**Parameters:**
- `filterList` (Table): A table containing strings that represent a type like `string`, `function`, `number`, `nil`, `table`, or `userdata`. When given the literal `nil`, it will assume the default config which is `{"string", "table", "number"}`.

**Returns:**
- Nothing

## Library Settings

### `lang.setLanguage(name)`
Sets the current language to use.

**Parameters:**
- `name` (String): The current language chosen.

**Returns:**
- Nothing

### `lang.setPrintErrors(bool)`
Enabled by default. When in protected mode, prints errors that are being caught.

**Parameters:**
- `bool` (Boolean): Enable with `true`, disable with `false`.

**Returns:**
- Nothing

## Resource Manipulation

### `lang.loadLanguage(name, data)`
Loads a table containing the strings into memory.

**Parameters:**
- `name` (String): The name that will be the key where the data will be saved.
- `data` (String or Table):
  - `String`: Path to the file to look for loading the table.
  - `Table`: Table containing the strings already loaded.

**Returns:**
- Nothing

### `lang.getString(stringKey, languageKey)`
Fetches a string from the translations.

**Parameters:**
- `stringKey` (String): String identification key. The key can be nested in other keys using the `.` separator, e.g., `menu.main.title`.
- `languageKey` (String): When set, this will override the current language only for this call to the specified one. If not found, it will just ignore it.

**Returns:**
- `String`: When used correctly, this will always return a string. It may return a table when the key points to said table but also may point to a function or undefined types if `setSanitizeOutput` is set to `false`.

### `lang.getLocalizationData()`
Gets all loaded translations.

**Parameters:**
- Nothing

**Returns:**
- `Table`: Contains the strings loaded. Although not encouraged, this can be manipulated directly.

### `lang.getCurrentLanguageData()`
Gets the data of the current language.

**Parameters:**
- Nothing

**Returns:**
- `Table`: Returns what is effectively `translations[activeLanguage]`.

### `lang.getBackupLanguageData()`
Gets the data of the fallback languages.

**Parameters:**
- Nothing

**Returns:**
- `Table`: This is a fallback table where a key will be searched when `currentLanguage` has no defined such key.

### `lang.getLanguage()`
Gets the current language that this library tries to look for when asked for a key on `getString`.

**Parameters:**
- Nothing

**Returns:**
- `String`: The current language that is set.
