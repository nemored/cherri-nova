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

## CI/CD

GitHub Actions validates and publishes the Nova extension with the Nova CLI:

- `.github/workflows/nova-ci.yml` validates the bundle on pushes to `main` and pull requests, then uploads the `.novaextension` bundle as an artifact.
- `.github/workflows/nova-publish.yml` validates and publishes the bundle to the Nova Extension Library on `v*` tags or manual dispatch.

Both workflows run on GitHub-hosted `macos-latest` runners and install Nova with Homebrew:

```yaml
brew install --cask nova
```

The publish workflow loads the Nova credentials from 1Password with `1password/load-secrets-action`, signs in non-interactively, verifies that login with `nova extension whoami`, and then runs `nova extension publish Cherri.novaextension`.

Configure these GitHub Actions settings before publishing:

- Secret `OP_SERVICE_ACCOUNT_TOKEN`: a 1Password service account token with access to the Nova credential item.
- Variable `NOVA_EMAIL_1PASSWORD_REF`: the 1Password secret reference for the Nova account email, such as `op://app-cicd/nova-extension-library/email`.
- Variable `NOVA_PASSWORD_1PASSWORD_REF`: the 1Password secret reference for the Nova account password, such as `op://app-cicd/nova-extension-library/password`.
