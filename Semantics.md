
### Enumerations

Don't use the [Flags] attribute, instead use a set of the enumeration. This adheres to the general principle that when all else is equal, prefer typed over untyped code.

Bad:
```csharp
[Flags]
enum Inventory
{
	Hat,
	Sword,
	BigSword
}

class Player
{
	Inventory Items
	{
		get;
		set;
	}

	int CalculateDamage()
	{
		int result = 0;
		if (Items.HasFlag(Inventory.Sword))
			result += 1;
		if (Items == Inventory.BigSword) //Was the omission of HasFlag intended?
			result += 2;
		return result;
	}
}
```

Good:
```csharp
enum Item
{
	Hat,
	Sword,
	BigSword
}

class Player
{
	ISet<Item> Items
	{
		get;
		set;
	}

	int CalculateDamage()
	{
		int result = 0;
		if (Items.Contains(Item.Sword))
			result += 1;
		if (Items.SetEquals(new [] { Item.BigSword })) //Intention is clear.
			result += 2;
		return result;
	}
}
```

### #if directive

Don't use the #if directive. Instead, use ConditionalAttribute. See [this blog post](http://blogs.msmvps.com/peterritchie/2011/11/24/if-you-re-using-if-debug-you-re-doing-it-wrong/) for more information.
