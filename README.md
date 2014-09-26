npm-examples
============


read zip file
```

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

JSONStream
```
SONStream = require('JSONStream')
stream = fs.createReadStream(file, {encoding: "ascii"}).pipe(JSONStream.parse())

   stream.on "data", (data) ->
       console.log 2222, data
   
   #read over  
   stream.on "root", (root) ->
       console.log root

   stream.on "error", (error) ->
       console.log "---->", error
```
