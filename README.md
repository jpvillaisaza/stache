# Stache

[![License BSD3](https://img.shields.io/badge/license-BSD3-brightgreen.svg)](http://opensource.org/licenses/BSD-3-Clause)
[![Hackage](https://img.shields.io/hackage/v/stache.svg?style=flat)](https://hackage.haskell.org/package/stache)
[![Stackage Nightly](http://stackage.org/package/stache/badge/nightly)](http://stackage.org/nightly/package/stache)
[![Stackage LTS](http://stackage.org/package/stache/badge/lts)](http://stackage.org/lts/package/stache)
![CI](https://github.com/stackbuilders/stache/workflows/CI/badge.svg?branch=master)

This is a Haskell implementation of Mustache templates. The implementation
conforms to the version 1.1.3 of the official [Mustache
specification](https://github.com/mustache/spec). It is extremely simple and
straightforward to use with minimal but complete API—three functions to
compile templates (from directory, from file, and from lazy text) and one to
render them.

The implementation uses the Megaparsec parsing library to parse the
templates which results in superior quality of error messages.

For rendering you only need to create Aeson's `Value` where you put the data
to interpolate. Since the library re-uses Aeson's instances and most data
types in the Haskell ecosystem are instances of classes like
`Data.Aeson.ToJSON`, the whole process is very simple for the end user.

Template Haskell helpers for compilation of templates at compile time are
available in the `Text.Mustache.Compile.TH` module. The helpers currently
work only with GHC 8 and later.

One feature that is not currently supported is lambdas. The feature is
marked as optional in the spec and can be emulated via processing of parsed
template representation. The decision to drop lambdas is intentional, for
the sake of simplicity and better integration with Aeson.

## Quick start

Here is an example of basic usage:

```haskell
{-# LANGUAGE OverloadedStrings #-}

module Main (main) where

import Data.Aeson
import Data.Text
import Text.Megaparsec
import Text.Mustache
import qualified Data.Text.Lazy.IO as TIO

main :: IO ()
main = do
  let res = compileMustacheText "foo"
        "Hi, {{name}}! You have:\n{{#things}}\n  * {{.}}\n{{/things}}\n"
  case res of
    Left bundle -> putStrLn (errorBundlePretty bundle)
    Right template -> TIO.putStr $ renderMustache template $ object
      [ "name"   .= ("John" :: Text)
      , "things" .= ["pen" :: Text, "candle", "egg"]
      ]
```

If I run the program, it prints the following:

```
Hi, John! You have:
  * pen
  * candle
  * egg
```

For more information about Mustache templates the following links may be
helpful:

* The official Mustache site: https://mustache.github.io/
* The manual: https://mustache.github.io/mustache.5.html
* The specification: https://github.com/mustache/spec
* Stack Builders Stache tutorial: https://www.stackbuilders.com/tutorials/haskell/mustache-templates/

## Contribution

Issues, bugs, and questions may be reported in [the GitHub issue tracker for
this project](https://github.com/stackbuilders/stache/issues).

Pull requests are also welcome.

## License

Copyright © 2016–present Stack Builders

Distributed under BSD 3 clause license.
