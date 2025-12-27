# 🗺️ Mapper

Do not use AutoMapper (reflection-based) which is much too slow and consumes far too much memory just to map an object.

## 🔄 Alternative

[https://mapperly.riok.app/](https://mapperly.riok.app/)

## [📈 Performance](https://mapperly.riok.app/docs/intro/#performance)

|Method|Mean|Error|StdDev|Gen 0|Allocated|
|---|---|---|---|---|---|
|AgileMapper|1,523.8 ns|3.90 ns|3.25 ns|1.5106|3,160 B|
|TinyMapper|4,094.3 ns|3.90 ns|3.05 ns|1.0300|2,160 B|
|ExpressMapper|2,595.8 ns|5.49 ns|5.14 ns|2.3422|4,904 B|
|AutoMapper|1,203.9 ns|2.30 ns|2.15 ns|0.9098|1,904 B|
|ManualMapping|529.6 ns|0.52 ns|0.44 ns|0.5541|1,160 B|
|Mapster|562.1 ns|1.14 ns|0.89 ns|0.9098|1,904 B|
|Mapperly|338.5 ns|0.95 ns|0.84 ns|0.4396|920 B|

## 📝 Notes :

- [🌏 Mapperly](Mapperly.md)