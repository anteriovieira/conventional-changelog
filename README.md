## Conventional Changelog Preset

Custom preset for conventional-changelog 

### Install

`yarn add -D @anteriovieira/conventional-changelog`

### Usage

```js
// changelog.js
const execa = require('execa')
const cc = require('conventional-changelog')
const custom = require('@anteriovieira/conventional-changelog')

const gen = module.exports = version => {
  const fileStream = require('fs').createWriteStream(`CHANGELOG.md`)

  cc({
    config: custom,
    pkg: {
      transform (pkg) {
        pkg.version = `v${version}`
        return pkg
      }
    }
  }).pipe(fileStream).on('close', async () => {
    delete process.env.PREFIX
    await execa('git', ['add', '-A'], { stdio: 'inherit' })
    await execa('git', ['commit', '-m', `chore: ${version} changelog`], { stdio: 'inherit' })
  })
}

gen(require('./package.json').version)
```

`$ node changelog.js`
