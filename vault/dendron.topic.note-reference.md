---
id: f1af56bb-db27-47ae-8406-61a98de6c78c
title: Note Reference
desc: "Dendron allows you to reference content from other notes and **embed** them in your current note"
updated: 1658179297646
created: 1597356582509
stub: false
---

## Summary

{{fm.desc}}.  This is also known as [transclusion](https://en.wikipedia.org/wiki/Transclusion).

## Details

Note references are different from regular [[links|dendron://dendron.dendron-site/dendron.topic.links]] in that they actually include the content of the destination in the current note.

Dendron supports the following reference types:
1. note references
2. header references
3. block references
4. positional references

References have the following syntax:

```
![[note]]
```

You can create a reference with an anchor to reference a specific part of the note.

```
![[note#hello]]
```

> NOTE: When referencing headers with spaces in them, the note ref needs to use `-` instead of spaces within the name
>
> For more information:
>
> - [StackOverflow: How to escape symbols in GitHub-flavored Markdown internal links / heading anchors?](https://stackoverflow.com/a/48760076/5340149)

## Reference Types

- NOTE: the following references will be using the [[Note Reference Sample|dendron.topic.note-reference.sample]] note as the source note

### Full Note Reference

A full note reference includes the entire contents of a note. 

```md
![[dendron.topic.note-reference.sample]]
```

![[dendron.topic.note-reference.sample]]

### Header References

A header reference includes the contents of a note starting from a header. A header reference will transclude everything from the given header to the next header of equal or lesser depth

- NOTE: if you are coming from an pre `0.103.0` version of dendron, you will need to manually enable this by setting [[enableSmartRefs|dendron.topic.note-reference.config.enable-smart-refs]] to `true`

```md
![[dendron.topic.note-reference.sample#header-1]]
```

![[dendron.topic.note-reference.sample#header-1]]

- NOTE: notice that this reference does not include [[Header 2|dendron.topic.note-reference.sample#header-2]] because the header is at an equal depth as [[Header 1|dendron.topic.note-reference.sample#header-1]]

### Block References

A block reference includes to an arbitrary markdown block. Block references append a [[block anchor|dendron.topic.note-reference.concepts.block-anchors]] to the end of some text by using the `^` (caret) character to denote an anchor. 

- Source content 
```md
<!-- NOTE: ^block-ref-example will be grayed out in both the editor and in the preview -->
Header 1.1 Content ^1f1egthix10t
```
- Example of block reference
```md
![[dendron.topic.note-reference.sample#^1f1egthix10t]]
```

![[dendron.topic.note-reference.sample#^1f1egthix10t]]

#### Reserved Anchors

Block references are generated automatically when you use the [[Copy Note Reference|dendron://dendron.dendron-site/dendron.topic.note-reference.commands.copy-note-ref]] command.
You can also manually add human readable block anchors. If so, note that the following anchors are reserved:

- `begin`
- `end`

### Positional Reference

A positional reference references the position of a note. It has a special case of [[#block-references]]. 

#### Begin Positional Reference

This reference will reference the start of a note until it encounters the first header

```md
![[dendron.topic.note-reference.sample#^begin]]
```

![[dendron.topic.note-reference.sample#^begin]]

#### End Positional Reference

This reference will reference the end of a note. 

- NOTE: the `end` positional reference cannot be used in the `start-anchor` position of a [[#range-reference]]

```md
![[dendron.topic.note-reference.sample#header-1:#^end]]
```

![[dendron.topic.note-reference.sample#header-1:#^end]]

## Range Reference

References can also include a range which will cause it to transclude from the `start-anchor` to the `end-anchor`.

```
![[note#{start-anchor}:#{end-anchor}]]
```

- Example from a header to a header
```md
![[dendron.topic.note-reference.sample#header-1:#header-22]]
```
![[dendron.topic.note-reference.sample#header-1:#header-22]]

- Example of a header to a block anchor
```md
![[dendron.topic.note-reference.sample#header-1:#^1f1egthix10t]]
```
![[dendron.topic.note-reference.sample#header-1:#^1f1egthix10t]]

## Commands

You can create a reference either by hand or using the `Copy Note Ref` command.

![[dendron://dendron.dendron-site/dendron.topic.note-reference.commands.copy-note-ref]]

## Advanced Options
### Line Offset

You can use line offsets to skip a number of lines when using a header reference. The syntax is `,{number}`. Below is an example of using a note reference offset to offset an initial heading, skipping the actual header when doing the embedding.

- NOTE: currently, note reference offsets are limited to the first anchor inside a block reference. They must also be a positive value

```
![[demo.embed.block#head1,1]]
```

### Wildcard Headers as a Range Boundary

When you're referencing a header by reference, sometimes you don't care what the next header is, just that the reference covers all text up to the next header. You can specify this using the `*` symbol in a header reference.

For example, the following would reference the content from header1 to the next header.

```
![[demo.embed.block#head1:#*]]
```

### Recursive References

References can refer to notes with references inside. Dendron current supports references two levels deep. This applies to both the **local preview** as well as **publishing**.

### Wildcard Link Reference

References accept the `*` operator at the end which lets you grab all notes of a given level of hierarchy. This also works with typical note reference operation like block selection which means you can use it to grab specific blocks from every note in a level.

```
![[demo.journal.2021.*]]
```

## Publishing

When you [[publish|dendron.topic.publish]] a note with a note reference, Dendron will embed the content of the reference in the page. If the content is part of a published page as specified by the publishing [[configuration|dendron.topic.publish-legacy.configuration]], Dendron will include a link to the page. If not, Dendron will embed the content without a link. If the referenced content is not publishable (eg. `published: false` set on frontmatter), Dendron will generate a custom [[404 link|dendron.topic.publish-legacy.features#selective-publication]].

## Configuration

- note references by default come with an outline. They are called [[pretty refs|dendron.concepts#pretty-ref]].

Toggle the following configuration to turn off this setting.

![[dendron.ref.config#useprettyrefs:#*]]

You can override this configuration for individual notes if you want (or don't want) pretty refs for only some notes.
Just add `config: {global: {enablePrettyRefs: true}}` (or `false`) to the [[frontmatter|dendron.topic.frontmatter]] of that note, like this:

```
...
updated: 1636492098692
config: { global: { enablePrettyRefs: false}}
---
```
