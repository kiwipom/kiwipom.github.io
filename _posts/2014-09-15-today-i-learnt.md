---
layout: post
title: Today I learnt:
tags:
- XDocument
- titbits
---

If you have an `XDocument` in memory, and you initialised it using code that looks like:

    private XDocument GetDocument()
    {
        var xDeclaration = new XDeclaration("1.0", Encoding.UTF8.EncodingName, string.Empty);
        var document = new XDocument(xDeclaration, new XElement("Root", GetChildElements());
    }

then you want to stash that document somewhere, let's say you just want to load it into memory:

    var memoryStream = new MemoryStream();
    using (var streamWriter = new StreamWriter(memoryStream)) {
        var document = GetDocument();
        streamWriter.Write(document);
    }

you will get:

    <Root>
        ... child elements
    </Root>


*That's right: You will **lose** the declaration from the xml document*


This is because even though the `XDeclaration` is 'in' the `XDocument`, calling `.ToString` will not render it.

You need to `.Save` the document to the stream instead:


    var memoryStream = new MemoryStream();
    using (var streamWriter = new StreamWriter(memoryStream)) {
        var document = GetDocument();
        document.Save(streamWriter);
    }

you will get:

    <?xml version="1.0" encoding="utf-8"?>
    <Root>
        ... child elements
    </Root>

Filed under "huh"...
