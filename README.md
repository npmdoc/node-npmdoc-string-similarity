# api documentation for  [string-similarity (v1.1.0)](https://github.com/aceakash/string-similarity#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-string-similarity.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-string-similarity) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-string-similarity.svg)](https://travis-ci.org/npmdoc/node-npmdoc-string-similarity)
#### Finds degree of similarity between strings, based on Dice's Coefficient, which is mostly better than Levenshtein distance.

[![NPM](https://nodei.co/npm/string-similarity.png?downloads=true)](https://www.npmjs.com/package/string-similarity)

[![apidoc](https://npmdoc.github.io/node-npmdoc-string-similarity/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-string-similarity_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-string-similarity/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-string-similarity/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-string-similarity/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Akash Kurdekar",
        "email": "npm@kurdekar.com",
        "url": "http://untilfalse.com/"
    },
    "bugs": {
        "url": "https://github.com/aceakash/string-similarity/issues"
    },
    "dependencies": {
        "lodash": "^4.13.1"
    },
    "description": "Finds degree of similarity between strings, based on Dice's Coefficient, which is mostly better than Levenshtein distance.",
    "devDependencies": {
        "gulp": "^3.9.1",
        "gulp-jasmine": "^2.3.0",
        "gulp-watch": "^4.3.6"
    },
    "directories": {},
    "dist": {
        "shasum": "3c66498858a465ec7c40c7d81739bbd995904914",
        "tarball": "https://registry.npmjs.org/string-similarity/-/string-similarity-1.1.0.tgz"
    },
    "gitHead": "8352ccafbfdbccebc87946a6a99b06d85f34d15e",
    "homepage": "https://github.com/aceakash/string-similarity#readme",
    "keywords": [
        "strings",
        "similar",
        "difference",
        "similarity",
        "compare",
        "comparison",
        "degree",
        "match",
        "matching",
        "dice",
        "levenshtein"
    ],
    "license": "ISC",
    "main": "compare-strings.js",
    "maintainers": [
        {
            "name": "aceakash",
            "email": "npm@kurdekar.com"
        }
    ],
    "name": "string-similarity",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/aceakash/string-similarity.git"
    },
    "scripts": {
        "test": "gulp test"
    },
    "version": "1.1.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module string-similarity](#apidoc.module.string-similarity)
1.  [function <span class="apidocSignatureSpan">string-similarity.</span>compareTwoStrings (str1, str2)](#apidoc.element.string-similarity.compareTwoStrings)
1.  [function <span class="apidocSignatureSpan">string-similarity.</span>findBestMatch (mainString, targetStrings)](#apidoc.element.string-similarity.findBestMatch)



# <a name="apidoc.module.string-similarity"></a>[module string-similarity](#apidoc.module.string-similarity)

#### <a name="apidoc.element.string-similarity.compareTwoStrings"></a>[function <span class="apidocSignatureSpan">string-similarity.</span>compareTwoStrings (str1, str2)](#apidoc.element.string-similarity.compareTwoStrings)
- description and source-code
```javascript
function compareTwoStrings(str1, str2) {
  var pairs1 = wordLetterPairs(str1.toUpperCase());
  var pairs2 = wordLetterPairs(str2.toUpperCase());
  var intersection = 0;
  var union = pairs1.length + pairs2.length;

  _forEach(pairs1, function (pair1) {
    for(var i = 0; i < pairs2.length; i++) {
      var pair2 = pairs2[i];
      if (pair1 === pair2) {
        intersection++;
        pairs2.splice(i, 1);
        break;
      }
    }
  });

  return (2.0 * intersection) / union;

  // private functions ---------------------------
  function letterPairs(str) {
    var numPairs = str.length - 1;
    var pairs = [];
    for(var i = 0; i < numPairs; i++) {
      pairs[i] = str.substring(i, i + 2);
    }
    return pairs;
  }

  function wordLetterPairs(str) {
    return _flattenDeep(_map(str.split(' '), letterPairs));
  }
}
```
- example usage
```shell
...
'''

In your code:

'''javascript
var stringSimilarity = require('string-similarity');

var similarity = stringSimilarity.compareTwoStrings('healed', 'sealed');

var matches = stringSimilarity.findBestMatch('healed', ['edward', 'sealed', 'theatre']);
'''
## API

Requiring the module gives an object with two methods:
...
```

#### <a name="apidoc.element.string-similarity.findBestMatch"></a>[function <span class="apidocSignatureSpan">string-similarity.</span>findBestMatch (mainString, targetStrings)](#apidoc.element.string-similarity.findBestMatch)
- description and source-code
```javascript
function findBestMatch(mainString, targetStrings) {
  if (!areArgsValid(mainString, targetStrings)) {
    throw new Error('Bad arguments: First argument should be a string, second should be an array of strings');
  }
  var ratings = _map(targetStrings, function (targetString) {
    return {
      target: targetString,
      rating: compareTwoStrings(mainString, targetString)
    };
  });

  return {
    ratings: ratings,
    bestMatch: _maxBy(ratings, 'rating')
  };

  // private functions ---------------------------
  function areArgsValid(mainString, targetStrings) {
    var mainStringIsAString = (typeof mainString === 'string');

    var targetStringsIsAnArrayOfStrings = Array.isArray(targetStrings) &&
      targetStrings.length > 0 &&
      _every(targetStrings, function (targetString) {
        return (typeof targetString === 'string');
      });

    return mainStringIsAString && targetStringsIsAnArrayOfStrings;
  }
}
```
- example usage
```shell
...
In your code:

'''javascript
var stringSimilarity = require('string-similarity');

var similarity = stringSimilarity.compareTwoStrings('healed', 'sealed');

var matches = stringSimilarity.findBestMatch('healed', ['edward', 'sealed', 'theatre']);
'''
## API

Requiring the module gives an object with two methods:

### compareTwoStrings(string1, string2)
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
