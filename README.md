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
