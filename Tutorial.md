# Tutorial on how to use node-tree-sitter
## Prerequisite
Make sure your `node.js` and `npm` is using the latest stable release.  
If not, error might occur during the process.

## How to get started?
```sh
# 1. Create a folder named XXX
mkdir XXX

# 2. Go into that folder
cd XXX

# 3. Initialize as an npm package
npm init
```

## Summary of what to be done
1. What to install?
2. How to define the grammar?
3. How to generate parser from the grammar?
4. How to re-generate parser from the grammar?
5. How to import the generated parser?
6. How to use the generated parser?

<hr>

<hr>

## 1. What to install?
```sh
# 1. Tree-sitter cli
#   To allow you to generate parser based on tree-sitter grammar
npm install --save tree-sitter-cli

# 2. Node tree sitter
#   Since the generated parser is in C we need to install `node tree sitter`
#   To allow you to import and use the generated parser in JS
npm install --save tree-sitter
```
Refer
- https://github.com/tree-sitter/node-tree-sitter
- https://github.com/tree-sitter/tree-sitter-cli

<hr>

<hr>

## 2. How to define the grammar?
First, create a file named `grammar.js` (NOTE: the file name must be exactly `grammar.js`, any typo would result in error later)
```
touch grammar.js
```

Then, define the grammar using the [Tree Sitter Grammar DSL](http://tree-sitter.github.io/tree-sitter/creating-parsers).  A working example can be found at [Tree Sitter Javascript Grammar](https://github.com/tree-sitter/tree-sitter-javascript/blob/master/grammar.js)


<hr>

<hr>

## 3. How to generate parser from grammar.js?
Before that, you need to install `node-gyp`.
```sh
sudo npm install -g node-gyp

node-gyp configure # Configure the platform (Linux/Windows/Mac, etc.)
```
*Note: After you run `node-gyp` configure, the `build` folder will appear, so please `gitignore` it.*

For further info refer https://github.com/nodejs/node-gyp.  

<hr>

Then, generate the `binding.gyp` file using `tree-sitter-cli`.
```
./node_modules/tree-sitter-cli/cli.js generate
```
*Note: After this command, a file named `binding.gyp` and a folder named `src` will appear.*
<hr>

Lastly, generate the Node.js binding from `binding.gyp` using `node-gyp`.  
```
node-gyp build
```

<hr>

<hr>

## 4. How to re-generate parser from the grammar?
Same as Step 3, but you will need to remove some file/folder first.
```
rm binding.gyp
rm -rf src
```
Note that the `src` folder is generated by `tree-sitter-cli`, so please make sure you did not create a folder named as `src` to store other scripts.

<hr>

<hr>

## 5. How to import the generated parser?
Create a Javascript file, then type in the following line.
```js
const language = require(`./build/Release/tree_sitter_${LANGUAGE}_binding.node`);
const Parser = require('tree-sitter');
const parser = new Parser();
parser.setLanguage(language);
```
The `LANGUAGE` variable depends on the `name` property defined in `grammar.js`.

<hr>

<hr>

## 6. How to use the generated parser?
Refer this [documentation](https://github.com/tree-sitter/node-tree-sitter).

<hr>

<hr>

## The end.

<hr>