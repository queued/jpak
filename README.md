         _ ____   _    _  __        _   ___  
        | |  _ \ / \  | |/ / __   _/ | / _ \ 
     _  | | |_) / _ \ | ' /  \ \ / / || | | |
    | |_| |  __/ ___ \| . \   \ V /| || |_| |
     \___/|_| /_/   \_\_|\_\   \_/ |_(_)___/ 

Description
========

**JPAK** is a multi-use Javascript Package System, developed for loading much files at once with only one package.


What is done
========
*   Javascript Loader
*   Python Packer
*   JPAK 1.0 Specification file

TODO
========
*   Load files from JPAK only when needed. A.K.A. - Fetch only the necessary parts from jpak file.

How to use it
========

**JPAK** is made to be simple. You can use for any file you want. First, you need to create a JPAK package.

You can use the python script called **packer.py** at `packer` folder. The sintax is simple, just create a folder and thrown what you want on that package inside it.

```shellscript
python packer.py PackageFolder
```

This will generate a file called `PackageFolder.jpak`, with that we can try it at web.

For the webpage, you need to create a JPAK.jpakloader object with a `file` parameter for it, pointing to the package file, and set a onload function.
```javascript
var Package = new JPAK.jpakloader({"file" : "PackageFolder.jpak"});
Package.onload = function()   {
    alert("Package loaded!");
};
```

After that, you need to call the load function, to start loading the file. At the end of processing, it will execute the onload function (if its defined).

```javascript
Package.Load();
```

By that, you are ready to use it to get the files. I will show two examples of how you can manage files. One image, and one text.

You have two functions to get files.

```javascript
jpakloader.GetFile(path, type)
```
Arguments: 
*   `path`      -> That is the relative path to the package. If the file before packaging was at `PackageFolder/test.jpg` here you would put `/test.jpg`
*   `type`      -> The mimetype of the file. Used in construction of the blob. Defaults to `application/octet-stream`
*   `returns`   -> Returns a blob file with the contents of the file

```javascript
jpakloader.GetFileURL(path, type)
```
*   Same as GetFile, but returns a local `URL` to the blob content.

So basicly, for images you can do something like:

```javascript
var imagetest = Package.GetFileURL("/test.jpg", "image/jpeg"); 
$("body").append('<img src="'+imagetest+'">');
```

and you can do for HTML (text/plain) for example:

```javascript
var testdothtml = Package.GetFile("/test.html", "text/html");
var reader = new FileReader();
reader.onloadend = function(evt)  {
    $("body").append(evt.target.result);
};
reader.readAsText(testdothtml);
```


Class
========

The **JPAK** script has a few features. It caches the already loaded blobs, for memory reduce for example.
Here are few functions for now:

```javascript
jpakloader.ls(path)
```

Lists the content of the path

Arguments: 
*   `path`      -> The path you want to list
*   `returns`   -> Returns an object `{ "dirs" : [ arrayofdirs ], "files" : [ arrayoffiles ], "error" : "An error message, if happens" }`


Example
========

You can check the folder `test` for see a working simple example that loads a Image, Javascript and a HTML file to the page.

