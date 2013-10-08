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

For long, multiline comments, stick to `//`:

```csharp
// Sartorial leggings ennui before they sold out banjo, lo-fi Truffaut
// Shoreditch sustainable Godard skateboard next level iPhone. Locavore tousled
// meh fingerstache DIY church-key keytar, Vice pug quinoa seitan. Blog photo
// booth Pinterest letterpress kogi leggings aesthetic irony.
```

Large comments tend to grow from smaller ones, and it's simpler to always use `//` than to switch to `/* ... */` when a comment becomes "long".
