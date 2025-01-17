# aws-lambda-brotli-unpacker ![](https://img.shields.io/badge/code_style-prettier-ff69b4.svg)

> Unpacks large Lambda binaries to /tmp, such as Tesseract, LibreOffice, Google Chrome, etc

Inspired by [chrome-aws-lambda](https://github.com/alixaxel/chrome-aws-lambda)

## Why fork original project?

Node 10.16 introduced native brotli support. Original project targeted >=8.10 Node environments.

## Install

```
$ yarn add @hypercharge/aws-lambda-brotli-unpacker
```
or
```
$ npm install @hypercharge/aws-lambda-brotli-unpacker
```

## Usage

> Q: Why do I need this package?

A: It helps if you want to deploy pre-compiled software to use in Lambda. See list of projects where it is used above.

> Q: Why bother?

A: Lambda environment has limited software installed. This package helps ship large binaries compiled for Lambda
which unpack to `/tmp` folder when Lambda starts.

> Q: Why `/tmp`?

A: Lambda has [500 MB of storage](https://docs.aws.amazon.com/lambda/latest/dg/running-lambda-code.html) in `/tmp`

> Q: Why brotli?

A: This compression algorithm is known for great speed/size ration. Perfect for scarce Lambda resources.

## Example

```js
const {unpack} = require('@hypercharge/aws-lambda-brotli-unpacker');
const {execSync} = require('child_process');

const inputPath = path.join(__dirname, '..', 'bin', 'tt.tar.br'); // for example, tesseract
const outputPath = '/tmp/tesseract/tesseract';

module.exports.handler = async event => {
  await unpack({inputPath, outputPath});

  execSync(`${outputPath} -l eng image.png`);
};
```
