# Working with `world.db` Models

Use the worlddb library (github: [`worlddb/world.db`](https://github.com/worlddb/world.db)) 
to work with the world.db schema 'n' models.


## Usage Models

`Country` Model - Example:

``` ruby
at = Country.find_by! key: 'at'
at.name
# => 'Austria'
at.pop
# => 8_414_638
at.area
# => 83_871

at.states.count
# => 9
at.states
# => [ 'Wien', 'Niederösterreich', 'Oberösterreich', ... ]

at.cities.by_pop
# => [ 'Wien', 'Graz', 'Linz', 'Salzburg', 'Innsbruck' ... ]
```

`City` Model - Example:

``` ruby
c = City.find_by! key: 'wien'
c.name
# => 'Wien'
c.country.name
# => 'Austria'
c.country.continent.name
# => 'Europe'

la = City.find_by! key: 'losangeles'
la.name
# => 'Los Angeles'
la.state.name
# => 'California'
la.state.key
# => 'ca'
la.country.name
# => 'United States'
la.country.key
# => 'us'
la.country.continent.name
# => 'North America'
```

`Tag` Model - Example:

``` ruby
euro = Tag.find_by! key: 'euro'
euro.countries.count
# => 17
euro.countries
# => ['Austria, 'Belgium', 'Cyprus', ... ]

flanders = Tag.find_by! key: 'flanders'
flanders.states.count
# => 5
flanders.states
# => ['Antwerpen', 'Brabant Wallon', 'Limburg', 'Oost-Vlaanderen', 'West-Vlaanderen']
flanders.states.first.country.name
# => 'Belgium'
```

and so on.
