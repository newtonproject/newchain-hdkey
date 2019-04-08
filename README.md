hdkey
=====

[![NPM Package](https://img.shields.io/npm/v/hdkey.svg?style=flat-square)](https://www.npmjs.org/package/hdkey)
[![build status](https://secure.travis-ci.org/cryptocoinjs/hdkey.svg)](http://travis-ci.org/cryptocoinjs/hdkey)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com/)

A JavaScript component for [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)(hierarchical deterministic keys).


Installation
------------

    npm i --save newchain-hdkey


Usage
-----

**example:**

```js
var HDKey = require('newchain-hdkey')
var seed = 'a0c42a9c3ac6abf2ba6a9946ae83af18f51bf1c9fa7dacc4c92513cc4dd015834341c775dcd4c0fac73547c5662d81a9e9361a0aac604a73a321bd9103bce8af'
var hdkey = HDKey.fromMasterSeed(new Buffer(seed, 'hex'))
console.log(hdkey.privateExtendedKey)
// => 'xprv9s21ZrQH143K2SKJK9EYRW3Vsg8tWVHRS54hAJasj1eGsQXeWDHLeuu5hpLHRbeKedDJM4Wj9wHHMmuhPF8dQ3bzyup6R7qmMQ1i1FtzNEW'
console.log(hdkey.publicExtendedKey)
// => 'xpub661MyMwAqRbcEvPmRAmYndzERhyNux1GoHzHxgzVHMBFkCro3kbbCiDZZ8uSmJawXrycScK9jM5TVupCPdmqxuvWqeBnK6np76yn2b7GVkE'
```


### `HDKey.fromMasterSeed(seedBuffer[, versions])`

Creates an `hdkey` object from a master seed buffer. Accepts an optional `versions` object.

```js
var seed = 'a0c42a9c3ac6abf2ba6a9946ae83af18f51bf1c9fa7dacc4c92513cc4dd015834341c775dcd4c0fac73547c5662d81a9e9361a0aac604a73a321bd9103bce8af'
var hdkey = HDKey.fromMasterSeed(new Buffer(seed, 'hex'))
```

### `HDKey.fromExtendedKey(extendedKey[, versions])`

Creates an `hdkey` object from a `xprv` or `xpub` extended key string. Accepts an optional `versions` object.

```js
var key = 'xprvA2nrNbFZABcdryreWet9Ea4LvTJcGsqrMzxHx98MMrotbir7yrKCEXw7nadnHM8Dq38EGfSh6dqA9QWTyefMLEcBYJUuekgW4BYPJcr9E7j'
var hdkey = HDKey.fromExtendedKey(key)
```

**or**

```js
var key = 'xpub6FnCn6nSzZAw5Tw7cgR9bi15UV96gLZhjDstkXXxvCLsUXBGXPdSnLFbdpq8p9HmGsApME5hQTZ3emM2rnY5agb9rXpVGyy3bdW6EEgAtqt'
var hdkey = HDKey.fromExtendedKey(key)
```

### `HDKey.fromJSON(obj)`

Creates an `hdkey` object from an object created via `hdkey.toJSON()`.

---

### `hdkey.derive(path)`

Derives the `hdkey` at `path` from the current `hdkey`.

```js
var seed = 'fffcf9f6f3f0edeae7e4e1dedbd8d5d2cfccc9c6c3c0bdbab7b4b1aeaba8a5a29f9c999693908d8a8784817e7b7875726f6c696663605d5a5754514e4b484542'
var hdkey = HDKey.fromMasterSeed(Buffer.from(seed, 'hex'))
var childkey = hdkey.derive("m/0/2147483647'/1")

console.log(childkey.privateExtendedKey)
// -> "xprv9xvaETDxuFq5cyuJHUC3S7bD7bPGR7uafuQb8Lvm8UqVUV1hCnSyrcZ2MYrEPKHP1A5fM14fN1XFqMXgvfWDvZhVayzf6A62mHYnqrRQywB"
console.log(childkey.publicExtendedKey)
// -> "xpub6BuvdxkrjdPNqTymPVj3oFXwfdDkpadS38LBvjLNgpNUMHLqkKmEQQsWCo7nk3NXCCxZKP5kAscqRq6YYzcq3LJ9gAt34wMhH9VFPgAm4BB"
```

### `hdkey.sign(hash)`

Signs the buffer `hash` with the private key using `secp256r1` and returns the signature as a buffer.

### `hdkey.verify(hash, signature)`

Verifies that the `signature` is valid for `hash` and the `hdkey`'s public key using `secp256r1`. Returns `true` for valid, `false` for invalid. Throws if the `hash` or `signature` is the wrong length.

### `hdkey.wipePrivateData()`

Wipes all record of the private key from the `hdkey` instance. After calling this method, the instance will behave as if it was created via `HDKey.fromExtendedKey(xpub)`.

### `hdkey.toJSON()`

Serializes the `hdkey` to an object that can be `JSON.stringify()`ed.

```js
var seed = 'fffcf9f6f3f0edeae7e4e1dedbd8d5d2cfccc9c6c3c0bdbab7b4b1aeaba8a5a29f9c999693908d8a8784817e7b7875726f6c696663605d5a5754514e4b484542'
var hdkey = HDKey.fromMasterSeed(Buffer.from(seed, 'hex'))

console.log(hdkey.toJSON())
// -> {
//      xpriv: 'xprv9s21ZrQH143K31xYSDQpPDxsXRTUcvj2iNHm5NUtrGiGG5e2DtALGdso3pGz6ssrdK4PFmM8NSpSBHNqPqm55Qn3LqFtT2emdEXVYsCzC2U',
//      xpub: 'xpub661MyMwAqRbcFW31YEwpkMuc5THy2PSt5bDMsktWQcFF8syAmRUapSCGu88PRW8Roww5CfAtDP9kVhL3q5wFNzWFtkmiZYMz2EdR2EzWVu1'
//    }
```

### `hdkey.privateKey`

Getter/Setter of the `hdkey`'s private key, stored as a buffer.

### `hdkey.publicKey`

Getter/Setter of the `hdkey`'s public key, stored as a buffer.

### `hdkey.privateExtendedKey`

Getter/Setter of the `hdkey`'s `xprv`, stored as a string.

### `hdkey.publicExtendedKey`

Getter/Setter of the `hdkey`'s `xpub`, stored as a string.

References
----------
- https://github.com/bitcoinjs/bitcoinjs-lib/blob/master/src/hdnode.js
- http://bip32.org/
- http://blog.richardkiss.com/?p=313
- https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki
- http://bitcoinmagazine.com/8396/deterministic-wallets-advantages-flaw/


License
-------

MIT
