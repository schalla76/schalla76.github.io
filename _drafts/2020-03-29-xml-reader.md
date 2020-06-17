---
layout: post
title: "XMLReader"
date: 2019-03-29
categories: xml
---

[XMLReader Source](http://diranieh.com/NETXML/XmlReader)

APIs

| API                                  | Description                                                        |
| ------------------------------------ | ------------------------------------------------------------------ |
| XmlReaderSettings                    | Used to set the setting like ignore the spaces, comments etc       |
| XmlReader.Create                     | Creates XML Reader                                                 |
| XmlReader.Read                       | Reads the start node, text, end node                               |
| XmlReader.MoveToContent              | Skips comments, declarations and goes to root element              |
| XmlReader.Depth                      | Gives the depth of the node                                        |
| XmlReader.NodeType                   | Type of the node                                                   |
| XmlReader.ReadStartElement           | Verifies if the node is correct and advances to next node          |
| XmlReader.Value                      | Gets the value of text                                             |
| XmlReader.ReadEndElement             | Verifies if is end node and advances to next node                  |
| XmlReader.ReadElementContentAsString | Reader node, text and end node                                     |
| XmlReader.AttributeCount             | Gives the attribute count and reader should be at start element    |
| XmlReader.HasAttributes              | Returns true if there are attributes                               |
| XmlReader.MoveToAttribute            | Moves the reader to a particular attribute                         |
| XmlReader.MoveToFirstAttribute       | Moves the reader to first attribute                                |
| XmlReader.Name                       | Gives the name of the attributes                                   |
| XmlReader.Value                      | Gives the value of the attributes                                  |
| XmlReader.MoveToNextAttribute        | Moves the reader to Next attribute                                 |
| XmlReader.MoveToElement              | Moves the reader to Element at owns the previously read attribute  |
| XmlReader.ReadInnerXml               | Returns all the elements inside node excluding the current element |
| XmlReader.ReadOuterXml               | Returns all the elements inside node including the current element |
| XmlReader.ReadOuterXml               | Returns all the elements inside node including the current element |

## XmlReader.MoveToContent

Skips all the non content nodes like XmlDeclaration, ProcessingInstruction, DocumentType, Comment, Attribute, Whitespace, SignificanWhitespace and moves to the content nodes like Text, CDATA, Element, EndElement, EntityReference, EndEntity.
