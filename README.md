# Shared Actions

This repo contains github actions that can be shared across the org.
The original impetus was for syncing actions to keep the mirrors updated,
but I'm sure that more uses will crop up soon.

## Actions:

### Syncing / mirroring actions:

- These actions will automatically sync an upstream repo of the same name once a day.
- The actual code of the upstream repo will be visible as branches and the plumbing branch should be set to default, with the sync action and documentation on how to use it.
- Branches and tags are all mirrored
- Backup branches are created with dates each time the sync runs. This makes sure that we still have a ref to commits even if upstream forcibly deletes things.

The two actions do things a little bit differently.

- `/.github/workflows/mirror.yml`
  - Designed to be used in a template repo that clones and mirrors a repo all on its own based on its name.
  - To use it, simply create a new repository with the appropriate name. eg, `overlookinfra/foo` will mirror `puppetlabs/foo`.
  - This one doesn't work well because github doesn't like it when actions create/modify workflows. There's a permission to allow this, but I couldn't get it to work properly.
  - Because of ☝️, workflow files are deleted with each sync, which makes the repo get out of sync easily.
- `/.github/workflows/sync.yml`
  - This one is far simpler. It relies on the user to have manually cloned each repo and set up the plumbing branch.
  - It basically just does a `git fetch` and a `git push` and creates the backup branches.
