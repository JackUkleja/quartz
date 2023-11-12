---
title: Code generation with Roslyn
date: 2016-05-29
layout: post
categories:
- Uncategorized
tags: []
comments: true
aliases:
    - ../code-generation-with-roslyn/
---

I recently wrote an answer on StackOverflow.com regarding what I've learned doing code generation with Roslyn.

My conclusion after a few weeks playing around is that using a combination of inline code snippets (parsed using `CSharpSyntaxTree.ParseText`) and manually generated `SyntaxNode`s, works well. But I felt a strong preference for the former. I have also used T4 in the past but am moving away from them due to general lack of integration &amp; capability. I compare the three approaches below and conclude with some tips about doing code gen with Roslyn:

## Advantages/disadvantages of each:

### Roslyn ParseText

* Generates arguably more readable code-generator code.
* Allows 'text templating' approach e.g. using C# 6 string interpolation.
* Less verbose.
* Guarantees valid syntax trees.
* Can be [more performant](http://comealive.io/Syntax-Factory-Vs-Parse-Text/).
* Easier to get started.
* Text can become harder to read than `SyntaxNodes` if majority is procedural.

### Roslyn SyntaxNode building

* Better for transforming existing syntax trees - no need to start from scratch.
    * But existing trivia can make this confusing/complex.
* More verbose. Arguably harder to read and build.
    * Syntax trees are often more complex than you imagine
* `SyntaxFactory` API provides guidance on valid syntax.
* [Roslyn Quoter](http://roslynquoter.azurewebsites.net/) helps you transform textual code to factory code.
* Syntax trees are not necessarily valid.
* Code is perhaps more robust once written.

## T4 templates
	
* Good if majority of code to be generated is boiler plate.
* No proper CI support.
* No syntax highlighting or intellisense without 3rd party extensions.
* One to one mapping between input and output files.
    * Not ideal if you are doing more complex generation e.g. entire class hierarchy based on single input.
* Still probably want to use Roslyn to "reflect" on input types, otherwise you will get into trouble with System.Reflection and file locks etc.
* Less discoverable API. T4 includes, parameters etc. can be confusing to learn.

## Roslyn code-gen tips
	
* If you are only parsing snippets of code e.g. method statements, then you will need to use `CSharpParseOptions.Default.WithKind(SourceCodeKind.Script)` to get the right syntax nodes back.
* If you are parsing a whole block of code for a method body then you will want to parse it as a `GlobalStatementSyntax ` and then access the `Statement` property as a `BlockSyntax`.
* Use a helper method to parse single `SyntaxNode`s:
```csharp
        private static TSyntax ParseText<TSyntax>(string code, bool asScript = false)
        {
            var options = asScript
                ? CSharpParseOptions.Default.WithKind(SourceCodeKind.Script)
                : CSharpParseOptions.Default;
    
            var syntaxNodes =
                CSharpSyntaxTree.ParseText(code, options)
                    .GetRoot()
                    .ChildNodes();
    
            return syntaxNodes.OfType<TSyntax>().First();
        }
```
* When building `SyntaxNode`s by hand you will typically want to make a final call to `SyntaxTree.NormalizeWhitespace(elasticTrivia: true)` to make the code "round-trippable".
* Typically you will want to use `SyntaxNode.ToFullString()` to get the actual code text including trivia.	
* Use `SyntaxTree.WithFilePath()` as a convenient place to store the eventual file name for when you come to write out the code.
* If your goal is to output source files, the end game is to end up with valid `CompilationUnitSyntax`s.
* Don't forget to pretty-print using `Formatter.Format` as one of the final steps.

