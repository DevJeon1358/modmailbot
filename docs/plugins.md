# 🧩 Plugins
The bot supports loading external plugins.

## Specifying plugins to load
For each plugin file you'd like to load, add the file path to the [`plugins` option](configuration.md#plugins).
The path is relative to the bot's folder.
Plugins are automatically loaded on bot startup.

## Creating a plugin
Plugins are simply `.js` files that export a function that gets called when the plugin is loaded.

For details about the function arguments, see [Plugin API](#plugin-api) below.

### Example plugin
This example adds a command `!mycommand` that replies with `"Reply from my custom plugin!"` when the command is used inside a modmail inbox thread channel.
```js
module.exports = function({ bot, knex, config, commands }) {
  commands.addInboxThreadCommand('mycommand', [], (msg, args, thread) => {
    thread.replyToUser(msg.member, 'Reply from my custom plugin!');
  });
}
```

(Note the use of [object destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Unpacking_fields_from_objects_passed_as_function_parameter) in the function parameters)

### Example of a custom attachment storage type
This example adds a custom type for the `attachmentStorage` option called `"original"` that simply returns the original attachment URL without rehosting it in any way.
```js
module.exports = function({ attachments }) {
  attachments.addStorageType('original', attachment => {
    return { url: attachment.url };
  });
};
```
To use this custom attachment storage type, you would set the `attachmentStorage` config option to `"original"`.

### Plugin API
The first and only argument to the plugin function is an object with the following properties:

| Property | Description |
| -------- | ----------- |
| `bot` | [Eris Client instance](https://abal.moe/Eris/docs/Client) |
| `knex` | [Knex database object](https://knexjs.org/#Builder) |
| `config` | The loaded config |
| `commands` | An object with functions to add and manage commands |
| `attachments` | An object with functions to save attachments and manage attachment storage types |
| `hooks` | An object with functions to add *hooks* that are called at specific times, e.g. before a new thread is created |
| `formats` | An object with functions that allow you to replace the default functions used for formatting messages and logs |

See the auto-generated [Plugin API](plugin-api.md) page for details.

## Plugin API stability
Bot releases may contain changes to the plugin API. Make sure to check the [CHANGELOG](../CHANGELOG.md) before upgrading!

Please send any feature suggestions to the [issue tracker](https://github.com/Dragory/modmailbot/issues)!
