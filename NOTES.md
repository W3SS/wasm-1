Notes
=====

* [Design](https://github.com/WebAssembly/design)
* [Semantics](https://github.com/WebAssembly/design/blob/master/AstSemantics.md)
* [Reference Implementation](https://github.com/WebAssembly/spec/tree/64822f7137e26c0b101ecba9cb1cd93d416c2c74/ml-proto)
* [Binary Encoding](http://webassembly.org/docs/binary-encoding/)
* [ilwasm](https://github.com/WebAssembly/ilwasm)
* [binaryren](https://github.com/WebAssembly/binaryen)
* [wassembler](https://github.com/ncbray/wassembler)
* [demo](https://ncbray.github.io/wassembler/)
* [backend prototype](https://github.com/ncbray/wassembler/blob/master/v8/backend.js)
* [WasmExplorer](https://mbebenita.github.io/WasmExplorer/)
* [Javascript API](http://webassembly.org/docs/js/)
* [Spec](https://webassembly.github.io/spec/index.html)

**Binary Protocol**

* [wasm.h](https://github.com/WebAssembly/binaryen/blob/master/src/wasm.h)
* [wasm-binary.h](https://github.com/WebAssembly/binaryen/blob/master/src/wasm-binary.h)

**Chrome**

* [v8 wasm](https://github.com/v8/v8/tree/master/src/wasm)

As of Chrome 56 the flag `#enable-webassembly` can be enabled to add support for
WebAssembly.

**Firefox**

* [BaldrMonkey: land initial wasm compilation and testing functions](https://bugzilla.mozilla.org/show_bug.cgi?id=1234985)

#### Goals

* Parse WebAssembly textual AST
* Read binary format
* Emit binary format
* Pretty print textual AST
* Verify soundness of code
* Interpreter
* High level code generation API  for retargeting
* Translation to [LLVM IR backend](http://reviews.llvm.org/D10569)

### Build with `stack` (recommended)

```bash
$ git clone https://github.com/sdiehl/wasm
$ cd wasm
$ stack install
```

```bash
$ wasm fac.wasm
```

### Build with `cabal`

```bash
$ git clone https://github.com/sdiehl/wasm
$ cd wasm

$ cabal sandbox init
$ cabal install
```

### How to Run Test Suite

```bash
$ stack test
```

### How to Run inside GHCi


```bash
$ stack ghci wasm
```

#### Building v8

In Chrome:

```javascript
WebAssembly.compile(new Uint8Array(
 `00 61 73 6d  0d 00 00 00  01 09 02 60  00 00 60 01
7f 01 7f 03  03 02 00 01  05 04 01 00  80 01 07 07
01 03 66 6f  6f 00 01 08  01 00 0a 3a  02 02 00 0b
35 01 01 7f  20 00 41 04  6c 21 01 03  40 01 01 01
0b 03 7f 41  01 0b 20 01  41 e4 00 6c  41 cd 02 20
01 1b 21 01  41 00 20 01  36 02 00 41  00 21 01 41
00 28 02 00  0f 0b 0b 0e  01 00 41 00  0b 08 00 00
00 00 2c 00  00 00`.split(/[\s\r\n]+/g).map(v => parseInt(v, 16))
)).then(mod => {
 let m = new WebAssembly.Instance(mod)
 console.log('foo(1) =>', m.exports.foo(1))
 console.log('foo(2) =>', m.exports.foo(2))
 console.log('foo(3) =>', m.exports.foo(3))
})
```

In V8:

```bash
$ git clone git@github.com:v8/v8.git
$ make native wasm=on

$ out/native/v8 --expose-wasm
V8 version 4.9.0 (candidate)
d8> buffer = readbuffer('test.bin');
[object ArrayBufferr
d8> module = WASM.instantiateModule(buffer, {});
{memory: [object ArrayBuffer], test: function test() { [native code] }}
d8> module.test()
3
```

