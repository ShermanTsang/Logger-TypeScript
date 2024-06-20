# Logger

`Logger` is a lightweight logging library that allows custom log styles and tags. It provides some predefined log types
and supports the dynamic creation of new log types.

![GitHub tag (latest by date)](https://img.shields.io/github/v/tag/ShermanTsang/Logger-TypeScript?label=version)

![Build Status](https://github.com/ShermanTsang/Logger-TypeScript/actions/workflows/npm-publish.yml/badge.svg)

![npm](https://img.shields.io/npm/dt/@shermant/logger)

# Features

- Multiple predefined log types (info, warn, error, debug, success, failure, plain)
- Custom log type styles
- Log tags
- Log prepend and append dividers
- Displaying log time
- Dynamic creation of custom log types

# Installation

Install using any package manager: npm, pnpm, yarn, or bun:

```bash
npm install @shermant/logger
pnpm install @shermant/logger
yarn add @shermant/logger
bun add @shermant/logger
```

# Concept

## Log Type

This lib provides seven predefined log types: `info`, `warn`, `error`, `debug`, `success`, `failure`, and `plain`.

You can use them directly to log messages, and every log type has a corresponding style.

## Style

This package uses the chalk package to style log messages. Any style provided by chalk can be used to customize log
message styles. Use the style method to customize the log message style. The style method takes an array of two strings:
the background color and the text color.

Therefore, you can use the `style` method to customize the log message style. The `style` method takes an array of two
strings: the background color and the text color.

Below is the type definition of the `Style` type

```typescript
import {ChalkInstance} from "chalk";

namespace LoggerType {
    // ignored content...
    type Style = keyof ChalkInstance;
    type Styles = Style[]
    // ignored content...
}
```

# API & Usages

The basic usage involves using the logger proxy to log messages. Predefined log types are wrapped in static getters,
allowing direct use.

## `Logger.[TypeName]` static getter

The basic usage of this package is to use the `logger` proxy to log messages.

This package wraps preset log types in static getters, and you can use them directly.

Each of them will return a `Logger` instance with the corresponding log type, you can continue to use chain calling .

```typescript
import {logger} from '@shermant/logger';

logger.info.tag('info title').message('This is an info message').print();
logger.warn.tag('warn title').message('This is a warning message').print();
logger.error.tag('error title').message('This is an error message').print();
logger.debug.tag('debug title').message('This is a debug message').print();
logger.success.tag('success title').message('This is a success message').print();
logger.failure.tag('failure title').message('This is a failure message').print();
logger.plain.tag('plain title').message('This is a plain message').print();
```

## `Logger` class

All functions are provided by the `Logger` class, but you don't need to create an instance of the `Logger` class
white `new` EcmaScript syntax.

```typescript
import {Logger} from '@shermant/logger';
```

## `logger` proxy

You are recommend to use the `logger` proxy to access the `Logger` class.

With the `logger` proxy, you can use the `Logger` class without creating an instance.

```typescript
import {logger} from '@shermant/logger';
```

## `logger.createLogger` function

You can also use the `createLogger` function to predefined a new logger instance with a custom log type.

To implement this, you need to pass a generic type to the `createLogger` function. The generic type is an object that
contains the new log type name and the `LoggerType.CreateCustomType` type.

```typescript
const logger = createLogger<{
    newType: LoggerType.CreateCustomType
}>()

logger
    .newType(['bgGreenBright', 'underline'])
    .tag('custom logger')
    .message('test adding custom logger type via type function')
    .print()
```

Next time, use `newType` with the `type` function.

```typescript
logger
    .type('newType')
    .tag('custom logger')
    .message('The next time you can use `newType` with `type` function')
    .print()
```

To use newType directly, cast it to the Logger type. Note that casting to unknown first will disable type prompting and
checking.

However, you need to cast it to `unknown` type first that will make you lose the type prompting and checking service
with `Logger` class.

This is not recommend code style

```typescript
const newType = logger.newType as unknown as Logger
newType
    .tag('custom logger')
    .message('Also, you can use `newType` directly, but not recommend')
    .print()
```

## `print()` method

Use print() to output the log message to the console. You can pass a boolean value to control whether the log message is
output.

## `type()` method

`type` is a static function that allows you to create a new log type.

### Specify log type

Use `type(string, [styles])` to specify the log type, or you can create custom log type.

```typescript
// You can use the `type` method to specify the log type
logger.type('anyType').tag('type').message('This is an info message').print();
```

### Override Preset Style

You can override the preset style by using the `style` method. The `style` method takes an array of two strings: the
background color and the text color.

```typescript

import {logger} from '@shermant/logger';

logger.type('info', ['bgRed', 'white']).tag('info title').message('This is a red info message').print();
```

### Add custom log types

When you use the `type` method to create a new log type, you need to pass two arguments: the type `name` and
the `style`.

```typescript
import {logger} from '@shermant/logger';

// The instance will automatically register a type named `myCustomType`
logger.type('myCustomType', ['bgRed', 'white'])

// Then, you can use `myCustomType` to log messages
logger.type('myCustomType').tag('custom').message('This is a custom message').print();
```

## `tag()` method

Use `tag(string)` to add a tag to the log message, and the tag will be displayed in the log message.

In the future, you can use `tag` attribute to filter log messages.

## `message()` method

Use `message(string)` to add a message to the log message.

### Emphasize key information

With `[[key infomation]]}` syntax, you can emphasize key information in the log message.

```typescript
import {logger} from '@shermant/logger';

logger.info.tag('info title').message('This is an info message with [[key information]]').print();
```

Then, the message `formatter` will use `chalk.yellow.underline` style to decorate the key information.

Later, you are able to customize the style and the matched sign of the key information.

## `prependDivider()` & `appendDivider()` method

Use `prependDivider()` to add a divider before the log message.

Use `appendDivider()` to add a divider after the log message.

Both of them receive three optional arguments: `char`, `length`, and `styles`.

The default value of `char` is `-`, the default value of `length` is `80`, and the default value of `styles` is
`['bgWhite', 'black']`.

```typescript
import {logger} from '@shermant/logger';

logger.info.tag('info title').prependDivider().message('This is an info message').print();
logger.info.tag('info title').appendDivider().message('This is an info message').print();
```

## `time()` method

If you want to display the log time, you can use the `time()` method.

```typescript
import {logger} from '@shermant/logger';

logger.info.tag('info title').time().message('This is an info message').print();
```
