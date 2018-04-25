# DIVAServices Website
Webpage is [here](https://lunactic.github.io/DIVAServicesweb/).

This Website is built from the DeepDIVA Webpage available [here](https://github.com/DIVA-DIA/DeepDIVAweb/).


# How to Update the Page

## Installing the tools

First install [Node and Node Package Manager (npm)](https://nodejs.org/en/download/) and
then use it to install [wintersmith]([http://wintersmith.io/) as:

```
$ npm install wintersmith -g
```

Navigate to the project directory and enter the `_builder` folder:

```
$ cd .../DIVAServicesweb/_builder
```

Get the third party library:

```
$ npm install
```

## Running a Preview

Navigate to the project directory and enter the `_builder` folder:

```
$ cd .../DIVAServicesweb/_builder
```

Then start the preview server:

```
$ wintersmith preview
```

At this point you are ready to start customizing your site.
Point your browser to http://localhost:8080 and start editing templates and articles.

## Updating the Page  

Navigate to the project directory and enter the `_builder` folder:

```
$ cd .../DIVAServicesweb/_builder
```

Then rebuild the page with:

```
$ wintersmith build
```

This generates your site and places it in the root/ directory - all ready to be
copied to your web server!

Finally, you have to commit and push the modification to make them available online:

```
$ git commit
$ git push
```

**NOTE: Many files auto-generated are not added to git automatically so you will
have to do it manually**

# How to Write the Markdown for a Tutorial

The tutorial files are located in  `_builder/contents/articles` folder.
Each of tutorial consist in a folder with snake-case with a meaningful name for
internal use. The name of the folder will not be displayed anywhere but will be
used for links and such. Inside such folder there are
- `index.md` file with the actual tutorial specifications
- all images used in the `index.md` file

## General comments

- Use single quotes \` for inline code  instead of \`\`\`
- Provide the language of the script in multi-line code snippets (see [here](https://help.github.com/articles/creating-and-highlighting-code-blocks/#syntax-highlighting))
- Use the # coherently for titles (don't use h6 if you are not using h5, h4, h3 and so on)

## Adding an image

The images have to be located in the same folder as the `index.md` file.
Then you can add them with the almost absolute path to them as:

``` markdown
![alt text](/DeepDIVAweb/articles/tutorial-name/file_name.png)
```

## Adding a link

Regular links (to other pages in the world) can be written as [usual](ühttps://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#links) in markdown.
To add a link to another tutorial have to specify the almost absolute path to its
snake-case folder:

``` markdown
![displayed text](/DeepDIVAweb/articles/tutorial-name)
```


## Adding a New Tutorial to the List

In order for a new tutorial to appear its necessary to add its entry in the file
`_builder/templates/tutorials-list.jade`. As you can see from other entires, within
the brackets its necessary to specify the real file name and after them is the official
displayed name on the webpage.
