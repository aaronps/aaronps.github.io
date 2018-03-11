# The Book of Notes

## Todo

[ ] gradle.md: consider using Copy task for custom templates

```groovy
copy {
   into 'build/webroot'
   exclude '**/.svn/**'
   from('src/main/webapp') {
      include '**/*.jsp'
      filter(ReplaceTokens, tokens:[copyright:'2009', version:'2.3.1'])
   }
   from('src/main/js') {
      include '**/*.js'
   }
}
```

[ ] gradle.md consider using ~/.gradle/templates as base for downloaded
templates, could use zip and/or full folders (including git).

## Changes

2018-03-11 - cleanup vscode.md. java,spring,grade WIP
2018-03-05 - converted the remaining asciidoc files to markdown


