![lvksh sunflake](./assets/banner.png)

[![MINIFIED SIZE](https://img.shields.io/bundlephobia/min/sunflake.svg)]()
[![COVERAGE](https://img.shields.io/badge/coverage-100%25-brightgreen.svg)]()
[![LANGUAGES](https://img.shields.io/github/languages/top/lvkdotsh/sunflake)]()
[![DEPENDENCIRES](https://img.shields.io/badge/dependencies-0-brightgreen.svg)]()
[![NPM](https://img.shields.io/npm/dt/sunflake)]()


Zero dependency, lightweight, snowflake generator.
Sunflake takes a 42 bit unix timestamp, 10 bits of machine id and 12 bits of sequence number. Sunflake generates id in string format (which easily can be casted into a 64 bit bigint in databases), as javascript is limited to 53 bit integer precision.

## The snowflake:
| 111111111111111111111111111111111111111111 | 1111111111 | 111111111111 |   |
|--------------------------------------------|------------|--------------|---|
| 64                                         | 22         | 12           | 0 |

| FIELD      | BITS | DESCRIPTION                                               | RETRIEVAL                    |
|------------|------|-----------------------------------------------------------|------------------------------|
| Timestamp  | 42   | Milliseconds since given Epoch                            | (snowflake >> 22) + epoch    |
| Machine Id | 10   |                                                           | (snowflake & 0x3E0000) >> 12 |
| Increment  | 12   | Increments for every id created<br>in the same timestamp. | snowflake & 0xFFF            |



* (snowflake >> 22) + epoch: We right shift with 22 (64 - 42). This grabs the 42 first bits which is the time since the epoch. We then add the epoch to it and we have a unix timestamp in milliseconds.
* (snowflake & 0x3E0000) >> 12: We start by offset `0x3E0000`, which in binary is `1111100000000000000000` which is 22 bits long. Therefore we start after the timestamp, then we take the values from there until the 12th bit which is the start of the increment.
* snowflake & 0xFFF: Grabs everything after the offset `0xFFF` which is `111111111111` in binary which is 12 bits long. We start at the 12th bit (start of increament).

## Installation

Using `npm`:

```sh
npm install sunflake
```

or if you prefer to use the `yarn` package manager:

```sh
yarn add sunflake
```

## Usage
```ts
import { generateSunflake } from "sunflake";

export const SnowflakeGen = generateSunflake({
    machineID: 1, // Machine id
    epoch: 1640988001000, // First second of 2022
});
```
Now you can use that function easily
```ts
const id1 = SnowflakeGen(); 
const id2 = SnowflakeGen();
```

## Contributors

[![](https://contrib.rocks/image?repo=lvkdotsh/sunflake)](https://github.com/lvkdotsh/sunflake/graphs/contributors)

## LICENSE

This package is licensed under the [GNU Lesser General Public License](https://www.gnu.org/licenses/lgpl-3.0).

Hexadecimal to decimal convertion within this package is sourced from [hex2dec](http://www.danvk.org/hex2dec.html), which is licensed under the [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0.html) license.
