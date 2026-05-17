# Cherri for Nova

This repository contains a Nova extension for the [Cherri Siri Shortcuts programming language](https://cherrilang.org).

The extension package lives in `Cherri.novaextension`.

## Extension Contents

- `extension.json`: Nova extension manifest
- `README.md`: Nova Extension Library details text
- `CHANGELOG.md`: Nova Extension Library change log
- `LICENSE.md`: Nova Extension Library license text
- `extension.png` and `extension@2x.png`: Nova Extension Library icons
- `Syntaxes/Cherri.xml`: Cherri syntax definition for `.cherri` files

## Grammar

The Nova grammar is maintained directly in `Cherri.novaextension/Syntaxes/Cherri.xml`.

For a scope-by-scope breakdown of the grammar, see `docs/nova-grammar.md`.

## File Type Icons

Nova does not expose a VS Code-style syntax contribution for assigning a custom icon file to a language extension in `extension.json` or syntax XML.

Per Nova's Images documentation, file type icons are requested by image name using the `__filetype.` prefix. If this extension later adds sidebar UI for Cherri files, use `__filetype.cherri` for those sidebar item icons. Nova will then ask the application or Finder for the best icon associated with `.cherri` files.
