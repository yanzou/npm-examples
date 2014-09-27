npm-examples
============


##read zip file
```js

stream = fs.createReadStream(SCENARIO_TAR_FILE, {
    flags: 'r',
    #encoding: 'utf-8',
    fd: null,
    bufferSize: 1
  }).pipe(zlib.createUnzip())

stream.addListener('data', (char) ->
    console.log(char)
    stream.pause()
    if char == '\n'
        #do whatever you want to do with the line here.
        #....
        #When done set the line back to empty, and resume the strem
        console.log "------>", line
        count++
        line = ''
        stream.resume()
    else
        #add the new char to the current line
        line += char
        stream.resume()
)
stream.addListener('error', ->
    console.log arguments
)
```

##JSONStream
```js
JSONStream = require('JSONStream')
stream = fs.createReadStream(file, {encoding: "ascii"}).pipe(JSONStream.parse())

   stream.on "data", (data) ->
       console.log 2222, data
   
   #read over  
   stream.on "root", (root) ->
       console.log root

   stream.on "error", (error) ->
       console.log "---->", error
```


tar exctract in stream
```js
tar = require('tar-stream')
extract = tar.extract()
fs.createReadStream(SCENARIO_TAR_FILE).pipe(zlib.createUnzip()).pipe(extract)
    .on 'entry', (header, stream, callback) ->
        console.log "header -->", header.name, header.size, header.type
        #console.log "stream -->", stream
        #if hearder.type == "directory"
        stream.resume();
        stream.on 'end', ->
            console.log "this entry end"
    .on 'error', ->
        console.log "error", arguments
    .on 'finish', ->
        console.log "finished"
```

##extractTarball
```js
tarball = require('tarball-extract')
tarball.extractTarball('sss.tgz', '/tmp/test/', (err) ->
    if (err)
        console.log(err)
    else
        console.log("oj")
)
```

##read files in directory and sub-directories
```js
recursive = require('recursive-readdir')
//["*.js", "*.pcap*"] is exclude files, pretty wire...
recursive(OFFLINE_DATA_PATH, ["*.js", "*.pcap*"], (err, files) ->
```
