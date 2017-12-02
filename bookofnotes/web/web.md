# Web notes

* howlerjs for audio playing

`<html lang="zh-Hans">` is the new zh-CN

* To clear iframe: best way set src to an html file empty.
This way avoids problems of browsers not really clearing the iframe.
* Maybe it could be possible to just remove the iframe element from the document.
* transparency on **iframe** (this may be old):

```
    <iframe src="file.html" ... allowTransparency="true"></iframe>
    
    file.html: body { background-color: transparent }
```

* Avoid white background on dark pages using `iframe` or similar to load content: `visibily: hidden` when added, change to `visible` on `load` event of the iframe.

* Microsoft strict mode: https://docs.microsoft.com/en-us/scripting/javascript/advanced/strict-mode-javascript

* no selection

```css
.no-select
{
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
```

### embed flash object

* Both object.data and param.movie are needed because different browsers may take the values from different locations, on my tests, firefox needs the `param`, ie and chrome can work with `data` attribute only.

```html
    <object type="application/x-shockwave-flash" width="100%" height="100%" data="flash-file-url.swf">
        <param name="movie" value="flash-file-url.swf">
    </object>
``` 

## Notes

* Do NOT use `canvas.createRadialGradient` on IE, its buggy.

* Need to study: seems **Chrome** delays flash activation somewhow, cannot call its methods until a little later. Note that flash is on an `iframe` which is loaded with `visibility: hidden` which may be making the problem appear
