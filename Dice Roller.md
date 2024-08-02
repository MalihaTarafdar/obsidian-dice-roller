---
icon: LiDices
datetime: 1722627907620
input: 1d20
arg: 1d20
output: 15:15
history: '["1722622854883:d20:12:12",object Object]'
---
An extensive dice roller app built entirely in Obsidian using the [Meta Bind](https://github.com/mProjectsCode/obsidian-meta-bind-plugin) and [Dataview](https://github.com/blacksmithgu/obsidian-dataview) plugins. See [[README#Documentation|Documentation]] for help. Submit any issues/feedback [here](https://github.com/MalihaTarafdar/obsidian-dice-roller/issues). 

`INPUT[text:input]` `BUTTON[roll-button]`
```meta-bind-button
label: Roll Dice
icon: dices
hidden: true
class: ''
tooltip: ''
id: roll-button
style: primary
actions:
  - type: updateMetadata
    bindTarget: arg
    evaluate: true
    value: getMetadata('input').replace(/\s/g, '')
  - type: updateMetadata
    bindTarget: datetime
    evaluate: true
    value: Date.now()
  - type: sleep
    ms: 250
  - type: command
    command: dataview:dataview-rebuild-current-view
```
```meta-bind-js-view
{arg} as arg
save to {output}
hidden
---
const arg = context.bound.arg;

// input validation
if (!/^(\d*d\d+|\d+)(?:(\+|-|\*|\/)(\d*d\d+|\d+))*$/.test(arg)) {
	return 'INVALID INPUT';
}

// roll dice
let rolls = [];
let roll = function(e) {
	// rollArgs[0]: roll count, rollArgs[1]: num sides
	const rollArgs = e.split('d');
	const rollCount = rollArgs[0] !== '' ? rollArgs[0] : 1;
	let sum = 0;

	for (let i = 0; i < rollCount; i++) {
		let r = Math.floor(Math.random() * rollArgs[1]) + 1;
		sum += r;
		rolls.push(r);
	}

	return sum;
}

// evaluate expression
let evaluate = function(e) {
	const ai = e.lastIndexOf('+');
	const si = e.lastIndexOf('-');
	const mi = e.lastIndexOf('*');
	const di = e.lastIndexOf('/');
	if ((ai != -1 && si == -1) || ai > si) {
		return evaluate(e.substring(0, ai)) + evaluate(e.substring(ai + 1));
	} else if ((ai == -1 && si != -1) || ai < si) {
		return evaluate(e.substring(0, si)) - evaluate(e.substring(si + 1));
	} else if ((mi != -1 && di == -1) || mi > di) {
		return evaluate(e.substring(0, mi)) * evaluate(e.substring(mi + 1));
	} else if ((mi == -1 && di != -1) || mi < di) {
		return Math.floor(evaluate(e.substring(0, di)) / evaluate(e.substring(di + 1)));
	} else if (/^\d*d\d+$/.test(e)) {
		return roll(e);
	} else if (/^\d+$/.test(e)) {
		return parseInt(e, 10);
	} else {
		return 'INVALID INPUT';
	}
};

const result = evaluate(arg);
return `${rolls.join(' ')}:${result}`;
```

```dataviewjs
// render output
if (dv.current().output === 'INVALID INPUT') {
	dv.span('`INVALID INPUT`');
} else {
	const output = dv.current().output.split(':');
	dv.span(`Input: \`${dv.current().arg.replace(/(\+|-|\*|\/)/g, ' $1 ')}\`<br/>`);
	dv.span(`Rolls: \`${output[0] !== '' ? output[0].replace(/ /g, ', ') : 'none'}\`<br/>`);
	dv.span(`Result: \`${output[1]}\``);
}
```

---
### Roll History

`BUTTON[add-history-button]` `BUTTON[clear-history-button]`

```meta-bind-button
label: 'Add'
icon: plus
hidden: true
class: ''
tooltip: 'Add to History'
id: 'add-history-button'
style: primary
actions:
  - type: updateMetadata
    bindTarget: history
    evaluate: true
    value: getMetadata('history').toString().charAt(0) + '"' + getMetadata('datetime') + ':' + getMetadata('arg') + ':' + getMetadata('output') + '",' + getMetadata('history').toString().substring(1)
  - type: sleep
    ms: 250
  - type: command
    command: dataview:dataview-rebuild-current-view
```
```meta-bind-button
label: 'Clear'
icon: trash-2
hidden: true
class: ''
tooltip: 'Clear History'
id: 'clear-history-button'
style: destructive
actions:
  - type: updateMetadata
    bindTarget: history
    evaluate: true
    value: '{}'
  - type: sleep
    ms: 250
  - type: command
    command: dataview:dataview-rebuild-current-view
```
```dataviewjs
// render roll history
if (dv.current().history.toString() === '[object Object]') {
	dv.paragraph('No entries. Click `Add` after rolling to add to roll history. ');
} else {
	let historyStr = dv.current().history.replace(/,object Object/g, '');
	let history = dv.array(JSON.parse(historyStr).map(s => s.split(':')));
	let table = dv.markdownTable(
		['Roll Number', 'Date & Time', 'Input', 'Rolls', 'Result'],
		history.map((r, i) => [
			`\`${history.length - i}\``,
			`\`${new Date(parseInt(r[0])).toLocaleString('en-US', {dateStyle: 'long', timeStyle: 'medium'})}\``,
			`\`${r[1].replace(/(\+|-|\*|\/)/g, ' $1 ')}\``,
			`\`${r[2] !== '' ? r[2].replace(/ /g, ', ') : 'none'}\``,
			`\`${r[3]}\``
		])
	);
	dv.paragraph(table);
}
```
