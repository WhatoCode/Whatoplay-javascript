# Whatoplay Javascript Style Guide

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

_A mostly reasonable approach to JavasScript guided by AirBnb_

> **Note**: this guide assumes you are using [Babel](https://babeljs.io/).

# Table of Contents

# Types

# Trivia

##### Pros and cons of `async` / `await` or generators

The polyfill for these contructs generates very large code.
This may change once all our supported browsers support ES6 natively.

Using async/await vs promise

```
function sample () {
	return new Promise((resolve, reject) => {
    	setTimeOut(() => {
          console.log('Hello world');
          resolve(true);
        }, 4000)
    })
}

sample().then(result => console.log(result)); // using promise

(async () => {
  const result = await sample();

  return result;
})(); // using async await with IIFE
```

Code transpiled from babel

```
"use strict";

function asyncGeneratorStep(gen, resolve, reject, _next, _throw, key, arg) { try { var info = gen[key](arg); var value = info.value; } catch (error) { reject(error); return; } if (info.done) { resolve(value); } else { Promise.resolve(value).then(_next, _throw); } }

function _asyncToGenerator(fn) { return function () { var self = this, args = arguments; return new Promise(function (resolve, reject) { var gen = fn.apply(self, args); function _next(value) { asyncGeneratorStep(gen, resolve, reject, _next, _throw, "next", value); } function _throw(err) { asyncGeneratorStep(gen, resolve, reject, _next, _throw, "throw", err); } _next(undefined); }); }; }

function sample() {
  return new Promise((resolve, reject) => {
    setTimeOut(() => {
      console.log('Hello world');
      resolve(true);
    }, 4000);
  });
}

sample().then(result => console.log(result)); // using promise

_asyncToGenerator(function* () {
  const result = yield sample();
  return result;
})(); // using async await with IIFE
```