# Dice Roller
Dice Roller is an extensive dice roller app for all your dice pool needs built entirely in Obsidian using the [Meta Bind](https://github.com/mProjectsCode/obsidian-meta-bind-plugin) and [Dataview](https://github.com/blacksmithgu/obsidian-dataview) plugins. Kept simple and lightweight by operating on YAML frontmatter. It currently supports basic dice notation (`AdX`) along with a 4-function calculator and the ability to save roll history. 

![screenshot](screenshot.png)

Coming soon:
- roll options (keep/drop highest/lowest rolls, re-roll until target, exploding dice, penetration dice, etc.)
- ability to add labels to rolls
- roll history statistics
- a more comprehensive calculator
- quality of life improvements

Submit any issues/feedback [here](https://github.com/MalihaTarafdar/obsidian-dice-roller/issues). 

---
## Installation
1. Download `Dice Roller.md` from [Releases](https://github.com/MalihaTarafdar/obsidian-dice-roller/releases) and import it into your Obsidian vault. 
2. Install and configure the required [dependencies](#dependencies) as instructed below. 
3. Optional: you may want to set "Properties in document" to "Hidden" (Settings → Editor → "Properties in document") due to length of the frontmatter block (especially as roll history increases)

### Dependencies
1. Install and enable the [Meta Bind](https://github.com/mProjectsCode/obsidian-meta-bind-plugin),  [Dataview](https://github.com/blacksmithgu/obsidian-dataview), and [JS Engine](https://github.com/mProjectsCode/obsidian-js-engine-plugin) plugins from the Obsidian Plugin Store. Store links are provided below. 
	- Meta Bind: [obsidian://show-plugin?id=obsidian-meta-bind-plugin](obsidian://show-plugin?id=obsidian-meta-bind-plugin)
	- Dataview: [obsidian://show-plugin?id=dataview](obsidian://show-plugin?id=dataview)
	- JS Engine: [obsidian://show-plugin?id=js-engine](obsidian://show-plugin?id=js-engine)
2. Meta Bind configuration: Settings → Meta Bind → toggle on "Enable JavaScript"
3. Dataview configuration: Settings → Dataview → toggle on "Enable JavaScript queries"
4. Optional: `Dice Roller.md` uses the `LiDices` icon if the [Iconize](https://github.com/FlorianWoelki/obsidian-iconize) plugin is installed

---
## Documentation
### The Roller
The input field accepts basic dice notation in the form of `AdX` where `A` is the number of dice to roll and `X` is the number of sides of the dice. Currently `A` and `X` are unconstrained positive integers although it is not possible to have dice with an exceedingly large amount of sides in actuality. 

The input field may also be used as a 4-function calculator (add, subtract, multiply, divide as `+`, `-`, `*`, `/` respectively). Any whitespace is ignored. 

We do not yet support any special dice pool mechanics. 

#### Examples
- `1d20 + 3`
- `2 * 4d6 - 1`
- `3 + 4 + 2 - 1`

### Roll History
After rolling, you may add your roll to the Roll History table by clicking the `Add` button. Click `Clear` to remove all entries in the Roll History table. We do not yet support the removal of individual entries or any sorting/filtering options. 

---
## License
This project is licensed under the terms of the [MIT License](LICENSE). 
This project does not include, but requires the installation of the following plugins:
- [Meta Bind](https://github.com/mProjectsCode/obsidian-meta-bind-plugin) (which is distributed under the GPLv3 License)
- [Dataview](https://github.com/blacksmithgu/obsidian-dataview) (which is distributed under the MIT License)
- [JS Engine](https://github.com/mProjectsCode/obsidian-js-engine-plugin) (which is distributed under the GPLv3 License)
