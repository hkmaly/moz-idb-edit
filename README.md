# moz-idb-edit
Tool for getting data from Firefox IndexedDB tables (including extension storage data) while the browser is not running. Under Mozilla Public License Version 2.0, as specified in LICENCE.md.

Based on https://gitlab.com/ntninja/moz-idb-edit and when I say based I mean I'm doing only few small fixes. Ask someone else for editing, see https://gitlab.com/ntninja/moz-idb-edit/-/issues/2.

Note that personally I'm using it in combination with https://github.com/gyng/save-in (see https://github.com/gyng/save-in/issues/159), the correct command line is

```./moz-idb-edit --extension '{72d92df5-2aa0-4b06-b807-aa21767545cd}' --profile ../profile '"save-in-history"'```
