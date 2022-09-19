# What is EzDrama

EzDrama format is a Markdown-like (or YAML-like) markup language that serves as intermediary between bare txt and TEI/XML that is ready to put on dracor.org

![ezdrama.png](ezdrama.png)


## Syntax:

### 1. Play text

Lines are tagged by special symbols at the beginning:

`#` means first level div (e.g. Act 1)

`##` means second level div (e.g. Scene 1)

`$` means new stage direction. NB:  brackets like this `()` are converted to stage directions automatically and do not require any special treatment

`@` means the line contains `<speaker>` appearance (possibly with inner stage direction in brackets). This will create an `<sp>` tag with a `<speaker>` inside it and then it will put all the following unmarked lines in the file inside that `<sp>` as character speech

any other line without these special symbols will be treated as direct speech of the last encoded speaker (`@`)

`~` means all the next lines within this speech are poetic text (will be encoded in `<l>`-s instead of a `<p>`)

### 2. Metadata

You can also encode some metadata for the header in the same file:

`author:` means the line contains the author of the play

`title:` means the line contains the main title of the play 

`subtitle` means the line contains the subtitle of the play

## Example

Here is a simple example. Suppose you have a txt like this:

```
Ham, a tragedy
By William S
Dramatis Personae
Ham
Eggs
Vikings
Act 1
Scene 1
Ham. Lovely Spam! 
Eggs. Wonderful Spam!
Scene 2
Enter Vikings
Ham. Egg, Spam, Sausage, and Bacon! 
Vikings (singing). Spam, Spam, Spam, Spam, Spam, Spam, Spam, and Spam`
The end
```

With EzDrama you encode it like this:

```
@title Ham 
@subtitle A tragedy
@author William S
^Dramatis Personae
Ham
Eggs
Vikings
#Act 1
##Scene 1
@Ham. 
Lovely Spam! 
@Eggs. 
Wonderful Spam!
##Scene 2
$Enter Vikings
@Ham. 
Egg, Spam, Sausage, and Bacon! 
@Vikings (singing).
Spam, Spam, Spam, Spam, Spam, Spam, Spam, and Spam`
$The end
```

And then you automatically get a TEI/XML like this:

```<TEI xml:lang="ukr" xmlns="http://www.tei-c.org/ns/1.0">
  <teiHeader>
    <fileDesc>
      <titleStmt>
        <title type="main">Ham</title>
        <title type="sub">A tragedy</title>
        <author>William S</author>
      </titleStmt>
    </fileDesc>
    <profileDesc>
      <particDesc>
        <listPerson>
          <person xml:id="eggs">
            <persName>Eggs</persName>
          </person>
          <person xml:id="vikings">
            <persName>Vikings</persName>
          </person>
          <person xml:id="ham">
            <persName>Ham</persName>
          </person>
        </listPerson>
      </particDesc>
    </profileDesc>
  </teiHeader>
  <text>
    <body>
      <castList>
        <руфв>Dramatis Personae</castItem>
        <castItem>Ham</castItem>
        <castItem>Eggs</castItem>
        <castItem>Vikings</castItem>
      </castList>
      <div type="act">
        <head>Act 1</head>
        <div type="scene">
          <head>Scene 1</head>
          <sp who="#ham">
            <speaker>Ham.</speaker>
            <p>Lovely Spam! </p>
          </sp>
          <sp who="#eggs">
            <speaker>Eggs.</speaker>
            <p>Wonderful Spam!</p>
          </sp>
        </div>
        <div type="scene">
          <head>Scene 2</head>
          <stage>Enter Vikings</stage>
          <sp who="#ham">
            <speaker>Ham.</speaker>
            <p>Egg, Spam, Sausage, and Bacon! </p>
          </sp>
          <sp who="#vikings">
            <speaker>Vikings</speaker>
            <stage>(singing).</stage>
            <p>Spam, Spam, Spam, Spam, Spam, Spam, Spam, and Spam</p>
          </sp>
          <stage>The end</stage>
        </div>
      </div>
    </body>
  </text>
</TEI>
```

Such markup takes some time to produce manually. But with EzDrama you can just produce it semi-automatically with just a handful of @, $ and linebreaks.
