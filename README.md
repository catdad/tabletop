# **Tabletop.js** (gives spreadsheets legs)

**This is a modified version of [Tabletop](https://github.com/jsoma/tabletop) by [jsoma](https://github.com/jsoma). This file will only document the changed I have made. Please refer to the [original](https://github.com/jsoma/tabletop) for more instructions on how to use it.**

**Tabletop.js** takes a Google Spreadsheet and makes it easily accessible through JavaScript. With zero dependencies!

## Callback change

This is the original way of setting up Tabletop in your file:

    function init() {
      Tabletop.init( { key: '0AmYzu_s7QHsmdDNZUzRlYldnWTZCLXdrMXlYQzVxSFE', //your key goes here
                       callback: function(data, tabletop) { console.log(data) },
                       simpleSheet: true } )
    }

However, if no data is fetched, the sheet you request is not available, or anything else goes wrong, the `callback` is never called, and your application is not aware that nothing executed. This was creating problems for me, so I converted the callback to the Node continuation model. Here is the new format:

    function init() {
      Tabletop.init( { key: '0AmYzu_s7QHsmdDNZUzRlYldnWTZCLXdrMXlYQzVxSFE', //your key goes here
                       callback: function(error, data, tabletop) { console.log(data) },
                       simpleSheet: true } )
    }

The first parameter of the `callback` will be an error. If everything works, this object will be `null`. If not, this object will contain a string that describes what went wrong. In the callback, add the following code:

    ...
    callback: function(error, data, tabletop){
        if (error && !data) console.log(error);
        else{
            //valid data is available here
            console.log(data);
        }
    }
    ...

## parseNumbers change

If `parseNumbers` is set to `true`, aside from numbers, it will also recognize the string values 'null', 'Null', and 'NULL' as `null` rather than a string value (they will now test as `false`).

## Credits (original from [jsoma](https://github.com/jsoma))

[Jonathan Soma](http://twitter.com/dangerscarf), who would rather be cooking than coding. Inspired by the relentless demands of [John Keefe(https://twitter.com/jkeefe) of WNYC.

Thanks to [Scott Seaward](https://github.com/plainview) for implementing multi-instance Tabletop.
