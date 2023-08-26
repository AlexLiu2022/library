---
{"dg-publish":true,"permalink":"/tree/nodejs/","tags":["CS/web","CS/programming-languages "],"created":"2023-05-30T01:38:41.385+08:00","updated":"2023-08-27T03:03:13.513+08:00"}
---


# Introduction 

## running js outside a browser

enter `node` in terminal, which enables the JS REPL (read and eval loop)
use `.exit` or `ctrl + d` to exit it

See all the global variables by hitting tab

\_is  your previous result

# Basic

## Use modules

```js
const fs = require('fs');
```

## Read and write files

```js
const fs = require('fs');

const textIn = fs.readFileSync('./txt/input.txt','utf-8'// encoding);
console.log(textIn);

const textOut = `This is what we know about avocado: ${textIn}.\nCreated on ${Date.now()}`;
fs.writeFileSync('./txt/output.txt',textOut//data);
console.log('File Written!');
```

## asynchronous

nodejs is single-threaded  

sync code is blocking code

use callbacks to implement non-blocking I/O model

```js
const fs = require('fs');


// blocking synchronous way
// const textIn = fs.readFileSync('./txt/input.txt','utf-8');
// console.log(textIn);

// const textOut = `This is what we know about avocado: ${textIn}.\nCreated on ${Date.now()}`;
// fs.writeFileSync('./txt/output.txt',textOut);
// console.log('File Written!'); 

// non-blocking asynchronous way
fs.readFile('./txt/start.txt','utf-8',(err,data1)=>{
  if(err) return console.log('ERROR! 🤡 ');

  fs.readFile(`./txt/${data1}.txt`,'utf-8',(err,data2)=>{
    console.log(data2);
    fs.readFile('./txt/append.txt','utf-8',(err,data3)=>{
      console.log(data3 );

      fs.writeFile('./txt/final.txt',`${data2}\n${data3}`,'utf-8', err =>{
        console.log('Your file has been written 😀');
      })
    })
  })  
})

console.log('Will read file!');
```

## simple web server 
```js
const http = require('http');

const server =  http.createServer((req,res) => {
  // console.log(req);
  res.end('Hello from the server');
});

server.listen(8000,'127.0.0.1', () => {
  console.log('Listening to requests on port 8000');
})
```

## routing

implement  different actions for different urls 

```js
const http = require('http');
const url = require('url');

const server =  http.createServer((req,res) => {
  const pathName = req.url;
  if (pathName === '/' || pathName === '/overview') {
    res.end('This is the Overview');
  } else if (pathName === '/product'){
    res.end('This is the Product')
  } else {
    res.writeHead(404, {
      'Content-type': 'text/html',
      'my-own-header': 'hello-world'
    });
    res.end('<h1>Page not found</h1>');
  }
});

server.listen(8000,'127.0.0.1', () => {
  console.log('Listening to requests on port 8000');
})
```
