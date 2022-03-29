# SPARQL Queries

## Select 

### 1.Querying classes

All classes: 

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>  
PREFIX art: <http://www.art-ontology.com/fmi#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
select ?class where
{
?class rdf:type owl:Class;
}
```

All subclasses and their parent classes: 

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>  
PREFIX art: <http://www.art-ontology.com/fmi#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select ?subclass ?class where
{
?class rdf:type owl:Class.
?subclass rdfs:subClassOf ?class.
}
```

### 2. Querying individuals

All people: 

```sh
prefix onto:<http://www.ontotext.com/> 
prefix onto:<http://www.ontotext.com/> 
select ?s where
{ ?s a <http://www.art-ontology.com/fmi#Person> . }
```
also valid: 

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>  
PREFIX art: <http://www.art-ontology.com/fmi#>
select ?person where
{
?person rdf:type art:Person;
}
```

All painters: 

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>  
PREFIX art: <http://www.art-ontology.com/fmi#>
select ?person  where
{
    ?person art:hasProfession art:Painter . 
}
```

### 3. Querying properties

All properties: 

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>  
PREFIX art: <http://www.art-ontology.com/fmi#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
select ?p where
{
?p rdf:type owl:ObjectProperty.
}
```

## Describe 

All facts about Painters in the KB: 

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>  
PREFIX art: <http://www.art-ontology.com/fmi#>
describe ?person where
{
    ?person art:hasProfession art:Painter . 
}
```


## Set operations

### 1. Union

All paintings by Leonardo Da Vinci and Claude Monet:

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>  
PREFIX art: <http://www.art-ontology.com/fmi#>
select ?person ?artWork where{ 
{
?person art:firstName ?fname;
art:lastName ?sname;
art:creatorOf ?artWork.
filter( str(?fname) = "Leonardo" ).  
filter( str(?sname) = "Da Vinci").
}
    UNION 
{
?person art:firstName ?fname;
art:lastName ?sname;  
art:creatorOf ?artWork.
filter( str(?fname) = "Claude" ).  
filter( str(?sname) = "Monet").
    }} 
```

### 2. Minus

All people, except for the ones born in Italy: 

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX art: <http://www.art-ontology.com/fmi#>
select ?person where{ 
	{
?person rdf:type art:Person;
	}
MINUS
	{
?person rdf:type art:Person;
art:birthPlace ?loc.
?loc art:locationName "Italy".
    } }  
```
	
## String operations and binding variables 


```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX art: <http://www.art-ontology.com/fmi#>
select  ?statement where{
?person art:hasProfession art:Painter;
art:firstName ?fname;   
art:lastName ?lname;   
art:creatorOf ?artWork.
    BIND (concat(str(?fname), " ", str(?lname), "is author of ", strafter(str(?artWork),str(art:)), ".") AS ?statement)
}
```

## Filtering

All painters from the High Renaissance which are born in Italy and have paintings shown in the Louvre: 

```sh
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX art: <http://www.art-ontology.com/fmi#>
select ?person ?artWork where{
?person art:hasProfession art:Painter;
art:style ?style .
filter( str(?style) = "High Renaissance" ).
?person art:birthPlace ?loc.
?loc art:locationName ?locname . 
filter( str(?locname) = "Italy" ).
?person art:creatorOf ?artWork.
?artWork rdf:type art:Painting;
art:locationIn ?museum.
?museum art:museumName ?mname. 
filter( str(?mname) = "Louvre" ).
}
```

## Tasks: 

1. All facts about Adolphe Monticelli.
2. All cities. 
3. All subregions (with and without reasoning). 
4. All works from the Louvre and the Hermitage.
5. All statements "Painter X is born in Y.", where X is a Painter, and Y is a Location.