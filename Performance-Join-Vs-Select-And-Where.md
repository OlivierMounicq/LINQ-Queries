## Performance Join vs Select with where

We will use the data coming from the file [Liste des communes](https://public.opendatasoft.com/explore/dataset/liste-des-communes-2019/table/?sort=-insee_arr)

Let the City class

```cs
public class City
{
	public string Id {get;}
	public string Name{get;}
	public string DepartmentCode{get;}

	public City(string rawData)
	{
		var arr = rawData.Split(';');
		Id = arr[0].Replace("COMMUNE_", string.Empty);
		Name = arr[1];
		DepartmentCode = arr[8];
	}
}
```

And the Deparment class

```cs
public class Department
{
    public string Code {get;}
    public string Name {get;}

    public Department(string rawData)
    {
        var arr = rawData.Split(';');
        Code = arr[8];
        Name = arr[7];
    }
}
```

Let ```cityList``` a list of city and ```deparmentList``` a department list. And we want to perform a join between the city list and the department list.  
We test two ways to perform the join :

#### Way 1 : the select and where 

```cs
var selectedList = cityList
		.Select(c => new { City = c.Name, Department = departmentList.Where(d => d.Code == c.DepartmentCode).FirstOrDefault()?.Name })
		.ToList();
```          
          
#### Way 2 : we use a join


```cs
var selectedList2 = cityList.Join(
                        departmentList,
                        c => c.DepartmentCode,
                        d => d.Code,
                        (key, g) => new { City = key.Name, Department = g.Name }
                    )
                    .ToList();
```

[Code source to get the statistical data](https://github.com/OlivierMounicq/JoinVersusSelectWhere)
