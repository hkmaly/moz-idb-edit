# mozilla indexeddb reader
Firefox's indexeddb backend is a sqlite file with unique encoding implemented in [`dom/indexeddb/Key.cpp`](https://hg.mozilla.org/mozilla-central/file/895e12563245/dom/indexedDB/Key.cpp). More on
https://stackoverflow.com/questions/54920939/parsing-fb-puritys-firefox-idb-indexed-database-api-object-data-blob-from-lin

## Usage

### Example 1: calibre
Extracting and building book reading position URLs from [calibre](https://calibre-ebook.com/)'s indexeddb entries.
The important notes from this example are the location of the database, here as `$IDB`, and the output formula: either `--json` flag or `jmespath` query.
 

The line noise that makes [`jq`](https://stedolan.github.io/jq/) pipe is useful only in illustrating post-processing -- you can ignore it and still understand `moz-idb-edit`.

#### json
```bash
IDB=$HOME/.mozilla/firefox/leerowtj.dev-edition-default/storage/default/http+++192.168.1.130+8081/idb/2796537424cearlbi.sqlite
./moz-idb-edit --dbpath $IDB --json |
  jq -r '.[]|values|["#book_id=\(.key[1])&bookpos=\(.last_read_position|.[]|values)&fmt=\(.key[2])&library_id=\(.key[0])&mode=read_book"]|@tsv'
```
outputs
> #book_id=294&bookpos=epubcfi(/16/2/4/86/2/1:0)&fmt=EPUB&library_id=calibre&mode=read_book
> #book_id=296&bookpos=epubcfi(/2/2/4/2@0:0)&fmt=EPUB&library_id=calibre&mode=read_book
> #book_id=308&bookpos=epubcfi(/26/2/4/2/2[chapter006]/8/2/1:0)&fmt=EPUB&library_id=calibre&mode=read_book

####  jmespath
 * A [`jmespath`](https://jmespath.org/) query can be used instead of outputting the entire db as json. No arguments default to "everything" `@`, i.e. `./moz-idb-edit --dbpath $IDB '@'`. NB. the python pretty printer used for displaying this is not guarantied to be valid json.

```bash
./moz-idb-edit --dbpath $IDB '*.[key[1], last_read_position.[*][0][0], key[2], key[0]]' # | jq -r '.[]|@tsv'
```

outputs
```
[[294, "epubcfi(/16/2/4/86/2/1:0)", "EPUB", "calibre"],
 [296, "epubcfi(/2/2/4/2@0:0)", "EPUB", "calibre"],
 [308, "epubcfi(/26/2/4/2/2[chapter006]/8/2/1:0)", "EPUB", "calibre"]]
```
