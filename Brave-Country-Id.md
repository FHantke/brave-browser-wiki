## Checking which `countryid_at_install` is being used

There's two ways of retrieving the `countryid_at_install` value within Brave.

#### `Method #1` (a bit more complicated)

You'll need to look at the `Preferences` file which is located in the following directories:

* macOS --> `~/Library/Application\ Support/BraveSoftware/Brave-Browser/Default`
* Windows --> `C:\Users\[user name]\AppData\Local\BraveSoftware\Brave-Browser\User Data\Default`
* Linux --> `~/.config/BraveSoftware/Brave-Browser/Default`

#### `Method #2` (easiest/fastest method)

You'll simply need to type in `brave://prefs-internals` into the URL and search for `countryid_at_install`.

## Converting `countryid_at_install` into Country Code

Once you've retrieved `countryid_at_install` using one of the methods mentioned above, the next step will be to convert the code into a usable country code using the following method. We'll go through US and Canada as examples:

#### `US Example`

The `countryid_at_install` for US is `21843`. You'll need to convert `21843` into binary. Example:

* `21843` = `0101010101010011:`

Once you've converted `countryid_at_install` into binary, you'll need to split into 8 bit chunks. Example:

* `01010101` = `85` = U (in ASCII)
* `01010011` = `83` = S (in ASCII)

#### `Canada Example:`

The `countryid_at_install` for Canada is `17217`. You'll need to convert `17217` into binary. Example:

* `17217` = `0100001101000001`

Once you've converted `countryid_at_install` into binary, you'll need to split into 8 bit chunks. Example:

* `01000011` = `67` = C (in ASCII)
* `01000001` = `65` = A (in ASCII)

## Lookup Table:

| countryid_at_install | Country |
| ---------------------| ------- |
| 21843                | US      | 
| 17217                | CA      |
| 18766                | IN      |