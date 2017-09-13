# PICO8 HTML Template

**Good touch controls + standalone detection + offline-cache for your PICO8 games!**


## Live Demo: [PICO8 demo game!](https://headjump.github.io/pico8_html_template/)

[<img src="https://github.com/headjump/pico8_html_template/blob/gh-pages/html-demo.jpg?raw=true">](https://headjump.github.io/pico8_html_template/)

Open it on your smartphone, it works fine :)


## About and why

In my opinion [PICO8](https://www.lexaloffle.com/pico-8.php) is the perfect fit for mobile devices - with it's small resolution, input restrictions and good performance. The square screen leaves enough space for touch controls.

The existing HTML-export however, by the time of writing, doesn't come with touch controls. Also, the touch controls provided on the PICO8 site itself never really worked for me.


## File structure

The template file is a single HTML file with inline styles, javascripts and svg-images.

This is so you don't need additional tools to build the code or link a lot of files. I tried my best to provide a good code structure and comments - so it should be easy to follow what is going on.

Override game.js and icon.png with your own files and set your title and description in the index.html file.



## Different button layouts

Don't be confused, the template contains different touch-control layouts. Just use the one that fits your game best and remove the others.

But you can also switch between different button-sets from within your PICO8 game while playing.

To set the layout from the PICO8 game, simply set the first GPIO:

```
function html_buttons(buttonset_number) poke(0x5f80,buttonset_number) end
```

This may sound like overkill but allows you things like giving players a layout with (←, →, X, O) during platforming sections and (↑, ↓, X) in the item menu.

If you don't need this, simply set your preferred buttonset and never change it afterwards (look through the HTMl for `changeButtonset(...);`) or remove everything you don't need from the HTML markup.


## Tipps

### What button-layouts work best?

If your game only uses a single action button, there is no need to display both. Why confuse the user? This is one of the advantages of touch controls - you are not tied to the keys on a keyboard or the buttons on a real controller.

If you only need ← and → (e.g. in a simple sidescroller) it is best to only use the left-right-axis for your touch-controls.

When you need all directions (←, →, ↑, ↓), you should try to optimize the touch-detection for your games needs. The HTML template uses thresholds to decide whether left,right,up and down are pressed. There are different thresholds for x- and y-axis. If your game uses all directions "equally" (e.g. for a game where your control your character from a top-down perspective) the thresholds for x and y should be similar. But if your game uses one axis primarily and the other axis "sometimes" (e.g in a sidescroller where you press down while jumping to stomp), you can set the threshold to detect up/down touches only when you are close to the button boundaries (look for `x_range` and `y_range` in the code)


### During development

If your game is still under heavy development and you frequently update it, you should remove the link to the manifest-file in the html-tag until you are "done". Otherwise you have to update the manifest each time you want to test your game.

To check the game locally in your browser while you are still developing, it is usually enough to append a (changing) parameter to the url to force the browser to load your changes (and not load them from cache). E.g `file:///C:/Users/.../pico8_html_template/index.html?1` to `?2` and so on.


## iOS Tipps/Problems

Current iOS unfortunately reloads the page when used as webapp everytime you switch back to the the webapp from another app (while multitasking).

This means, your PICO8 game restarts everytime you switch back to the app. The only workaround at the moment is to save and restore the players progress within the game. At minimum you should save the current level number and accumulated score and restore it on gamestart (or give the player the option to continue the game).
