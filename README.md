# ReGrid
A file storage system for RethinkDB inspired by GridFS

The [ReGrid spec](https://github.com/internalfx/regrid-spec) is being discussed on the RethinkDB slack channel. There **WILL** be breaking changes.

[![npm version](https://img.shields.io/npm/v/rethinkdb-regrid.svg)](https://www.npmjs.com/package/rethinkdb-regrid) [![license](https://img.shields.io/npm/l/rethinkdb-regrid.svg)](https://github.com/internalfx/regrid/blob/master/LICENSE)

---

Special thanks to [Arthur Andrew Medical](http://www.arthurandrew.com/) for sponsoring this project.

Arthur Andrew Medical manufactures products with ingredients that have extensive clinical research for safety and efficacy. We specialize in Enzymes, Probiotics and Antioxidants.

---

## Installation

Supports node v4.0+

```
npm install --save rethinkdb-regrid
```

## TL;DR

```javascript
var ReGrid = require('rethinkdb-regrid')

var dbfs = ReGrid({db: 'example'})

// prepDB creates tables and indexes if they don't exist, returns a promise.
dbfs.initBucket().then(function () {
  // We are now ready to read and write files

  // create read stream from file
  var fileStream = fs.createReadStream('./bigvid.mp4')
  var dbStream = dbfs.createWriteStream('/videos/bigvid.mp4')

  // Pipe it to a RethinkDBFS write stream
  fileStream.pipe(dbStream)

  dbStream.on('finish', function () {
    // File is now written to the database

    //Read the file and pipe it to a write stream to save the file back out to the file system.
    dbfs.createReadStreamByFilename('/videos/bigvid.mp4').pipe(fs.createWriteStream('./copy-of-bigvid.mp4'))
  })

})
```

## API Documentation

### `ReGrid([connectionOptions, bucketOptions])`

##### Arguments

###### `connectionOptions`

connectionOptions is passed directly to [rethinkdbdash](https://github.com/neumino/rethinkdbdash)

###### `bucketOptions`

| key | default | type | description |
|---|---|---|---|
| bucketName | `fs` | String | The name of the bucket. Table names are prefixed by this. |
| chunkSizeBytes | `1024 * 255` | Number | The default chunk size, in bytes. |
| concurrency | `10` | Number | When reading/writing a file, the number of concurrent queries in flight for a given stream. |

### Example

```javascript
var ReGrid = require('rethinkdb-regrid')

var bucket = ReGrid(connectionOptions, bucketOptions)
```
