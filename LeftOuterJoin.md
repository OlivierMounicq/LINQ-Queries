## Left Outer Join

We use the DefaultIfEmpty

```cs
var banks = new List<string>{"BNP", "NATIXIS", "BPCE", "CACIB", "SGCIB"};

var accountBanks = new List<AccountQuantityBank>
{
    new AccountQuantityBank{Bank = "BNP", Quantity = 2},
    new AccountQuantityBank{Bank = "CACIB", Quantity = 3},
    new AccountQuantityBank{Bank = "SGCIB", Quantity = 4}
};

var stats = banks.Select(
                        left => accountBanks
                            .Where(r => r.Bank == left)
                            .DefaultIfEmpty(new AccountQuantityBank{Bank = left, Quantity = 0})
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
BNP : 2
NATIXIS : 0
BPCE : 0
CACIB : 3
SGCIB : 4
```
