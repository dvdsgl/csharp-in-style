# Comments

Comments begin with `// ` (note the single space), and use sentence casing, grammar, and proper spelling.

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

For long, multiline comments use the following style:

```csharp
/*
 * Blah
 * Blah again
 * and another Blah
 */
```
