# dweb-storage

DWeb specific storage provider for [DWebFs](https://github.com/distributedweb/dwebfs)

```
npm install dweb-storage
```

## Usage

``` js
var storage = require('dweb-storage')

// files are stored in ./my-dataset
// metadata (hashes and stuff) are stored in ./my-dataset/.dweb
// secret keys are stored in ~/.dweb/secret_keys/<discovery-key>
var archive = dwebfs(storage('my-dataset'))
```

### Custom storage provider

You can require this module in your own storage provider in order to override certain behaviors for some files while still using the default dweb storage methods for other files. Here's an example of overriding only the secret key storage and nothing else:

```js
const defaultStorage = require('dweb-storage')
const alternativeSecretStorage = require('your-own-custom-random-access-file-module')

module.exports = function keychainStorage() {
  const storage = defaultStorage(...arguments)
  return {
    metadata: function(file, opts) {
      if (file === 'secret_key') alternativeSecretStorage(file)
      return storage.metadata(...arguments)
    },
    content: function(file, opts) {
      return storage.content(...arguments)
    }
  }
}
```

### Options

- `secretDir` - folder to store secret keys in (default is users home dir)
- `prefix` - subfolder to put dweb SLEEP files in (default is `.dweb/`)

### Secret Keys

By default secret keys are stored in the users home directory via [dweb-secret-storage](https://github.com/joehand/dweb-secret-storage). To change the directory, pass it as an option:

```js
var storage = require('dweb-storage')

var archive = dwebfs(storage('my-dataset', {secretDir: '/secret_keys'})
```

## License

MIT
