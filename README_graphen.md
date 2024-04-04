# Graph

(literarur 1)[https://engineering.fb.com/2015/09/14/core-infra/graphql-a-data-query-language/]
(literatur 2)[https://de.wikipedia.org/wiki/Graphersetzungssystem]
()[https://de.wikipedia.org/wiki/Generative_Programmierung]
()[https://www.youtube.com/watch?v=hd6QgeJW-Rc]
    
    Czarnecki, Krzysztof, Ulrich W. Eisenecker: Generative Programming: Methods, Tools, and Applications. Addison-Wesley, 2000, ISBN 0-201-30977-7.

    Olaf Zwintzscher: Komponentenbasierte & generative Software-Entwicklung. W3L, 2003, ISBN 3-937137-50-5.

    Peter Rechenberg, Hanspeter Mössenböck: Ein Compiler-Generator für Mikrocomputer. Grundlagen, Anwendung, Programmierung in Modula-2. Hanser, 1988, ISBN 3-446-15350-0.

    Michael Klar: Einfach generieren. Generative Programmierung verständlich und praxisnah. Hanser, 2006, ISBN 3-446-40448-1.

    Claude Gomez, Tony Scott: Maple programs for generating efficient FORTRAN code for serial and vectorised machines. In: Computer Physics Communications. Band 115, Nummer 2–3, 1998, S. 548–562, doi:10.1016/S0010-4655(98)00114-3.

    T.C Scott, M.B Monagan, I.P Grant, V.R Saunders: Numerical computation of molecular integrals via optimized (vectorized) FORTRAN code. In: Nuclear Instruments and Methods in Physics Research Section A: Accelerators, Spectrometers, Detectors and Associated Equipment. Band 389, Nummer 1–2, 1997, S. 117–120, doi:10.1016/S0168-9002(97)00059-4.

    T.C. Scott, Wenxing Zhang: Efficient hybrid-symbolic methods for quantum mechanical calculations. In: Computer Physics Communications. Band 191, 2015, S. 221–234, doi:10.1016/j.cpc.2015.02.009.


mit einer einfachen "sprache" können die Anforderungen definiert werden
der generator erzeugt daraus code und verifiziert ihn


```graph
Alice -is- object
Alice -has- attribute
Alice -
Product|id:s:1|type:s:|name:s:modul 
TMM contains VV1
TMM contains VV2
VV1|id:1 contains P1|id:2 
VV1|id:1 contains-3 P2
VV1|id:1 contains P3|id:2

```
## Design einer "Sprache"


## Design eines "Sprach-Interpreters"

```js
ZEILE = "VV1|id:1 contains P1|id:2" 
ZEILE.split(' ') // Array(3) [ "VV1|id:1", "contains", "P1|id:2" ]

ZEILE2="type:object|name:VV1|id:1 contains type:object|name:P1|id:2"
ZEILE2.split(' ')[0].split('|') // Array(3) [ "type:object", "name:VV1", "id:1" ]

ZEILE3="type:object|name:VV1|id:1 contains-3 type:object|name:P1|id:2"
ZEILE3.split(' ')[1].split('-') // Array [ "contains", "3" ]

ZEILE4="VV2 is object|type:s:Valve1"
ZEILE4.split(' ')[2].split('|')[1].split(':') //Array(3) [ "type", "s", "Valve1" ]

```
## Generator

```js
'use strict'
import readline  from 'node:readline';
import fs        from 'node:fs';
const t=[]
,processLine = async ()=> { const FS = fs.createReadStream(process.argv[2]||'lines.txt')
                          ,       RL = readline.createInterface({input:FS,crlfDelay:Infinity})
                          ;
                          RL.on('line', (input) => { t.push(input.length);   
                                                     console.log(`Received: ${input.length}`);  
                                                   }); 
                          RL.on('close' , ()    => { console.log('finished',t.length);
                                                     console.timeEnd('Process...');
                                                   });
                          }
;
console.time('Process...');
processLine();
```