# Text Translation Overrides

Ever wanted to change what certain text fields say on the **Kaltura Player**? You’re in the right place. This guide shows how to override player text using either the **KMC Player Studio** or your **embed setup config**.

---

## Reference

There are **two main ways** to apply translation overrides:

1. **Static config via Player Studio (KMC)**  
   Go to **Player Studio → Player Settings → Advanced Settings**.  
   In the JSON editor, locate the `config -> ui` section — this is where you’ll add translation overrides.  
   ![Player Studio Advanced Settings Screenshot](/resources/player-studio.png)  
   ![Advanced Settings JSON Screenshot](/resources/player-studio-advanced.png)

2. **Dynamic config via Embed Setup**  
   Add the `ui` element with your translation overrides to the JSON passed into `KalturaPlayer.setup()`.  
   ![Embed Setup JSON Screenshot](/resources/player-embedCode-setup.png)

---

The player and its plugins define many UI strings through translation JSON files. These files exist in multiple languages, and overrides are handled through the same `ui.translations` configuration. We’ll use that to customize any text you’d like.

---

## Examples

### Start Simple: Core Player Elements

Let’s begin with the core player UI (no plugins yet).  
You can browse the core translation files in [`playkit-js-ui/translations`](https://github.com/kaltura/playkit-js-ui/tree/master/translations).  
Translation keys follow this structure:  
`language → component/plugin → text_key: "translation"`  

For example, suppose you want to change **“Advanced captions settings”** to **“Customize captions display”**:  
![Advanced Captions Settings Screenshot](/resources/player-advancedCaptionsSettings.png)

You’ll find this text under both the `captions` and `cvaa` components — so we’ll override both for consistency.

---

### 1. Static Config in Player Studio

Add the nested translation JSON in the `ui.translations` section of your player’s **Advanced Settings**:  
![Static JSON Override Screenshot](/resources/player-studio-advanced-translations-customizeCaptionsDisplay-withModal.png)

---

### 2. Dynamic Config in Embed Code

You can also define the same overrides when embedding the player:

```html
<div id="kaltura_player_835573579" style="width: 1280px; height: 720px"></div>
<script src="https://cdnapisec.kaltura.com/p/5954112/embedPlaykitJs/uiconf_id/57372623"></script>
<script>
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
    kalturaPlayer.loadMedia({ entryId: '1_ous6w2fq' });
  } catch (e) {
    console.error(e.message);
  }
</script>
```

![Embed Code Override Screenshot](/resources/player-embedCode-setup-translations-customizeCaptionsDisplay-withModal.png)

> ⚠️ **Note:** These examples use English (`"en"`). For multilingual players, add equivalent overrides for each supported language.

---

### Next Up: Plugin-Specific Text Overrides  
(Coming up next — we’ll look at how to apply translation overrides for plugins.)
