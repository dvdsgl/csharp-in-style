# Comments

Comments begin with `//` followed by a single space, use sentence casing, and exhibit proper spelling and grammar.

```csharp
// Great:
// Verify that the client and server states are consistent.

// Bad - missing space:
//Verify that the client and server states are consistent.

// Bad - not a sentence:
// verify client server states
```

If your comment just paraphrases code, remove it:

```csharp
// Bad
// Makes the window key and orders it front.
window.MakeKeyAndOrderFront ();
```

### Multiline comments

For long comments, stick to `//`:

```csharp
// Sartorial leggings ennui before they sold out banjo, lo-fi Truffaut
// Shoreditch sustainable Godard skateboard next level iPhone. Locavore tousled
// meh fingerstache DIY church-key keytar, Vice pug quinoa seitan. Blog photo
// booth Pinterest letterpress kogi leggings aesthetic irony.
```

Long comments tend to grow from smaller ones, so it's simpler to always use `//` than to switch to `/* ... */` when a comment becomes "long".

The only recommended use of `/* ... */`-style comments is for commenting out code. Please do not comment out multiple lines of code with `//`. You should avoid commenting out code anyway, prefering version control or other methods.
