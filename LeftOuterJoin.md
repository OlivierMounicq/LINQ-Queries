## Left Outer Join

We use the DefaultIfEmpty

```cs
var banks = new List<string>{"BNP", "NATIXIS", "BPCE", "CACIB", "SGCIB"};

var accountBanks = new List<AccountQuantityBank>
{
    new Stat{Bank = "BNP", Quantity = 2},
    new Stat{Bank = "CACIB", Quantity = 3},
    new Stat{Bank = "SGCIB", Quantity = 4}
};

var stats = banks.Select(
                        l => accountBanks
                            .Where(r => r.Bank == l)
                            .DefaultIfEmpty(new AccountQuantityBank{Bank = l, Quantity = 0})
                            .ToList()
                        )
                    .SelectMany(t =>t)
                    .ToList();
```

And the class

```cs
public class AccountQuantityBank
{
    public string Bank {get;set;}
    public int Quantity{get;set;}
    
    public override string ToString()
    {
        return $"{Bank} : {Quantity}";
    }
}
```

And the output is:

```cmd

```
