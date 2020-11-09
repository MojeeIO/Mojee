# Welcome to Mojee :zany_face:

## What the is Mojee?

Mojee is a lightning :zap: fast Emoji library for .NET & TypeScript (.ts is coming soon).

![Mojee - A lighting fast Emoji libraries](https://user-images.githubusercontent.com/62210/97496776-14ea7880-192f-11eb-9c49-68c7260ff76d.png)

Mojee's big party trick is the ultra high-performance parse and replacement of emoji `:short-codes:` within a block of text. Pass a string of text to Mojee, and Mojee will convert any emoji `:short-codes:` found with actual emoji character instances. Here's a basic Mojee `Replace` sample:

```csharp
// Replace all shortcodes with an emoji
var result = Mojee.Replace("Hello, world :smile:");

Console.WriteLine(result);
```

The console output from the above sample would be:

```
Hello, world 😄
```

Mojee is also amazing at Emoji search. Search for any term and Mojee will hand back a collection of emoji results. The following sample demonstrates a basic emoji search using Mojee.

```csharp
// Search for emojis
Mojee.Search("smile").ToList()
     .ForEach(emoji => Console.Write(emoji));
```

The console output from the above sample would be:

```
😄 😸 😃 😺 😅
```

## Getting Started

Within seconds, Mojee can be added to any .NET Core project using NuGet.

```sh
# Install using dotnet CLI
dotnet add package Mojee

# or,

# Instal using NuGet CLI
Install-Package Mojee
```

# API Docs

## Mojee Class

### `Replace` static method

The static method `Mojee.Replace` will replace all emoji shortcodes in a string with unicode emoji characters.

```csharp
string Mojee.Replace(string text)
```

Argument | Type | Description
-- | -- | --
`text` | `string` | The input text to perform the replacement on.

Returns a new string with all matched `:short-codes:` replaced.

#### Example

```csharp
var result = Mojee.Replace("Hello, world :smile:");

Console.WriteLine(result);
```

The `Replace` method also has a handy overload where an `EmojiMatch` evaluator can be passed. Each matched `:short-code:` found by Mojee is passed to the `evaluator` for custom processing.

```csharp
string Mojee.Replace(string text, Func<EmojiMatch, string> evaluator)
```

Argument | Type | Description
-- | -- | --
`text` | `string` | The input text to perform the replacement on.
`evaluator` | `Func<EmojiMatch, string>` | The function evaluating a replacement string based on the matched Emoji. Returning `null` keeps the original capture as is.

Returns a new string with all matched `:short-codes:` replaced with the result from the `evaluator`.

A basic use-case for the `evaluator` would be logging each emoji match. In the following sample we keep a simple count of the emoji shortcode matches that occur within a string.

#### Example

```csharp
static void Main(string[] args)
{
    var count = 0;
    
    var result = Mojee.Replace(":+1: Hello, world :smile:", match => 
    {
        count++;

        // Any custom processing of the
        // emoji match can happen here.

        // Return the Emoji character
        return match.Emoji.Character;
    });

    Console.WriteLine($"Count: {count}");
    Console.WriteLine(result);

    Console.ReadKey();
}
```

The console output from the above sample would be:

```
Count: 2
👍 Hello, world 😄
```

### `Search` static method

Search for emojis. 

Search by any Shortcode substring (`"cat"`), or by an exact Emoji Shortcode (`":smile:"`), or if a Shortcode startsWith (`":smi"`).

```csharp
IEnumerable<Emoji> Mojee.Search(string query)
```

Argument | Type | Description
-- | -- | --
`query` | `string` | Any substring, exact Emoji Shortcode, or a Shortcode starting with.

Returns the matched Emoji instances as a `IEnumerable<Emoji>`. 

#### Example

```csharp
var result = Mojee.Search("smile");

result.ToList().ForEach(emoji => 
{
    Console.WriteLine($"{emoji}: {emoji.Shortcode}");
});
```

The console output from the above sample would be:

```
😄: :smile:
😃: :smiley:
😅: :sweat_smile:
😋: :yum:
😁: :grin:
😊: :blush:
☺️: :relaxed:
😆: :laughing:
😍: :heart_eyes:
😈: :smiling_imp:
😻: :heart_eyes_cat:
```

## EmojiMatch Class

Property | Type | Description
-- | -- | --
`Emoji` | `Emoji` | The captured Emoji. `readonly`
`Index` | `int` | The position in the original string where the first character of the captured Emoji is found. `readonly`
`Length` | `int` | Gets the length of the Emoji capture. `readonly`
`Type` | `MatchType` | The match type - specifies what Emoji identifier was captured. `readonly`

## MatchType Enum

Property | Description
-- | --
`Shortcode` | An Emoji was captured by Shortcode

## Emoji Class

Represents a single Emoji instance.

Property | Type | Description
-- | -- | --
`Id` | `string` | A unique identifier based on the unicode code point value.
`Shortcode` | `string` | The unique short name for this Emoji (e.g. `:smile:`).
`Aliases` | `IReadOnlyList<string>` | A list of Shortcode alias names.
`Character` | `string` | The unicode code point value for this Emoji.
`UnicodeChars` | `char[]` | Characters of the Character string.
`Emoticon` | `string?` | The emoticon string pattern for this Emoji.
`Description` | `string?` | A basic description of this Emoji or related keywords.
`Tags` | `IReadOnlyList<string>` | A list of tags related to this Emoji.
`Order` | `int` | The order of this Emoji within the collection of Emojis.
`Category` | `Category` | The Emoji Category.

### `Get` static method

#### Example

```csharp
var emoji = Emoji.Get("smile");
Console.Write($"{emoji.Shortcode}: {emoji}");
// smile: 😄
```

### `GetAll` static method

Gets the list of all supported Emojis.

```csharp
IReadOnlyList<Emoji> Emoji.GetAll()
```

#### Example

```csharp
// Get the first 5 emojis
var emojis = Emoji.GetAll()
    .Take(5)
    .ToList();

// Iterate through the list of emojis
emojis.ForEach(emoji => 
{
    // Write the emojis to the Console
    Console.WriteLine(emoji);
});
```

The console output from the above sample would be:

```
😀
😃
😄
😁
😆
```

### `TryGet` static method

Tries to get an Emoji object based on the supplied shortcode. See also `Emoji.Get(string)`.

```csharp
bool Emoji.TryGet(string shortcode, out Emoji emoji)
```

Argument | Type | Description
-- | -- | --
`shortcode` | `string` | A Shortcode. Colons are optional (both values are valid: `":smile:"` and `"smile"`).
`out` | `Emoji` | The found Emoji or null.

#### Example

```csharp
var matched = Emoji.TryGet("smile", out var emoji);

Console.WriteLine($"Found something? {matched}");
Console.WriteLine(emoji);
```

The console output from the above sample would be:

```
Found something? True
Emoji: 😄
```

## Licensing

Mojee is free to use in both open-source and commercial applications, although with support for a limited set of emojis unless a license key is provided. 

The most 50 most popular emojis are supported for free, which covers approximately 80% of the emojis commonly used today.

[Purchasing](https://mojee.io/pricing) a **Pro** or **Team** license key will unlock the full power of Mojee with support for all 1743 emojis.
