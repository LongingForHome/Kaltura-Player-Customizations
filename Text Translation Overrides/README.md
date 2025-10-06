# Text Translation Overrides
Ever wanted to change what a certain text field on the Kaltura player says?  If so, then you're in the right place.  In this guide, we'll go over how to do just that.

## Reference
There are two ways to achieve these overrides.  
1. The first is via static config on the player in the the player Studio in the KMC.
  To access this, you'll go the Studio in the KMC and choose the 'Advanced Settings' at the bottom of the 'Player Settings' tab: ![Screenshot of the Player Settings tab for a player in the KMC player Studio with an arrow highlighting the link to the Advanced Settings](/resources/player-studio.png)  Once you're in the Advanced Settings, look for the `config->ui` element as that's where we'll be editing the config to include our desired translations: ![Screenshot of the Advanced Settings JSON editor for a player in the KMC player Studio, with the config > ui element highlighted](/resources/player-studio-advanced.png).
2. The other is if you want to control these more dynamically using the player setup config when embedding the Kaltura player.
  For this option, you just need to add the `ui` element in the JSON being supplied to the `KalturaPlayer.setup()` method: ![Screenshot of a Kaltura player embed code with a red box highlighting an added ui element in the JSON setup](/resources/player-embedCode-setup.png) 

We'll provide examples of both options as we go along...but for context, let's lay a little groundwork first.

The core player, along with many of the available plugins, defines many of the textual elements via a 'translations' JSON file.  There are multiple language versions of some of these files, so just make sure that there is a supported language translation for the component elements you are trying to override.  The player manages these 'translation' files via the `ui` configuration.  We'll use that `ui` config as the mechanism for overriding any desired translations.

## Examples
With that out of the way, let's jump into trying a few things.

### KISS - start with the basics
Before we address the scenario with plugins, let's just start with the core player elements.  Remember that the `ui` configuration is controlling all the translations, including the core elements.  You can find those core `ui` text translations in the [*playkit-js-ui* component's translations](https://github.com/kaltura/playkit-js-ui/tree/master/translations).  You'll notice that each translation follows the same path: language->component/plugin->text:override.  So, let's just take a basic text field from the player...how about the 'Advanced captions settings' text: ![Screenshot of the bottom section of a Kaltura player with the Settings > Advanced captions settings option showing](/resources/player-advancedCaptionsSettings.png)
Now, let's say that we want that to say something different, like maybe 'Customize captions display'.  We can go about overriding the default with our own 'translation' using the translations for the `ui` config.  As mentioned above, there are two paths for this, so let's look at both:
1. Static JSON player config
  If you look at the default translation file, you'll find two places where the text 'Advanced captions settings' exists.  Once in the 'captions' element (which is what you see when you click on the Settings icon in the player) and once in the 'cvaa' element (which is the modal overlay you see in the player once you click on the 'Advanced captions settings' option).  So, we'll want to make sure we change both for consistency.  So now we'll need to create the nested JSON for both elements and include the translation item along with our override text that we wish to use.  ![Screenshot of a Kaltura player in Advanced Setting tab of the Studio in KMC.  Shows the edited JSON config with boxes and arrows highlighting the text overrides and their resulting effect on a player](/resources/player-studio-advanced-translations-customizeCaptionsDisplay-withModal.png)
  
2. Dynamic JSON player setup in embed code
  Using the same logic from above, you can specify the desired translation overrides for the 'captions' and 'cvaa' elements as part of the JSON argument to the `KalturaPlayer.setup()`: ![Screenshot of embed code with the added translations for the 'captions' and 'cvaa' elements.  Red boxes highlight each element and point to the resulting rendering of the overrides on the Kaltura player](/resources/player-embedCode-setup-translations-customizeCaptionsDisplay-withModal.png). 
  Code sample of this config:
  ```html
  <div id="kaltura_player_835573579" style="width: 1280px;height: 720px"></div>
  <script type="text/javascript" src="https://cdnapisec.kaltura.com/p/5954112/embedPlaykitJs/uiconf_id/57372623"></script>
  <script type="text/javascript">
      try {
          var kalturaPlayer = KalturaPlayer.setup({
          targetId: "kaltura_player_835573579",
          provider: {
              partnerId: 5954112,
              uiConfId: 57372623
          },
          ui: {
              translations: {
                  en: {
                      captions: {
                          advanced_captions_settings: "Customize captions display"
                      },
                      cvaa: {
                          title: "Customize captions display"
                      }
                  }
              }
          }
          });
          kalturaPlayer.loadMedia({entryId: '1_ous6w2fq'});
      } catch (e) {
          console.error(e.message)
      }
  </script>
  ```

⚠️: These examples provide the overrides for English texts (notated by the 'en' child element of the 'translations' parent).  Best practice would be to also provide the same overrides for each language you are supporting in your player configuration by adding the additional language objects in the translations parent.

### Adding some complexity with overriding texts from plugins

