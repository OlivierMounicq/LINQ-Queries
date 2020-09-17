## Distinct by using the GroupBy 

Let a _Person_ class

```cs
public class Person
{
	public string FirstName {get;}
	public string LastName {get;}
	public string Country {get;}
	
	public Person(string firstName, string lastName, string country)
	{
		FirstName = firstName;
		LastName = lastName;
		Country = country;
	}
	
	public override string ToString()
		=> $"{FirstName} {LastName} ({Country})";
}

```

And we populate a person list with some duplicate and the purpose is to remove the duplicate items.

```cs
var personList = new List<Person>();
personList.Add(new Person("Albert","Einstein","USA"));
personList.Add(new Person("Richard","Feynmann","USA"));
personList.Add(new Person("Albert","Einstein","USA"));
personList.Add(new Person("Marie","Curie","France"));
personList.Add(new Person("Paul","Langevin","France"));
personList.Add(new Person("Marie","Curie","France"));
```

Ans there are two ways to remove the duplicate items:


```cs
var distinctPersonList = personList
                        .GroupBy(
                          p => new { p.FirstName, p.LastName, p.Country }, 
                          p => p, 
                          (k,g) => g.First()
                         )
                         .ToList();
```

_or_

```cs
var distinctPersonList = personList
			.GroupBy(p => new { p.FirstName, p.LastName, p.Country })
			.Select(t => t.First())
			.ToList();
```

