# Rule 6.1.4

## Summary

This test consists in checking whether the context of each combined link
is enough explicit to understand the purpose and the target

## Business description

### Criterion

[6.1](https://references.modernisation.gouv.fr/referentiel-technique-0#crit-6-1)

### Test

[6.1.4](https://references.modernisation.gouv.fr/referentiel-technique-0#test-6-1-4)

### Description

Chaque <a href="https://references.modernisation.gouv.fr/referentiel-technique-0#lien-composite">lien composite</a> v&eacute;rifie-t-il une de ces conditions (<a href="http://references.modernisation.gouv.fr/referentiel-technique-0#critres-61-et-63" title="Cas particuliers pour le crit&egrave;re 6.1">hors cas particuliers</a>) ? 
 
 * L'<a href="https://references.modernisation.gouv.fr/referentiel-technique-0#intitul-de-lien">intitul&eacute; de lien</a> seul permet d'en comprendre la fonction et la destination 
 * Le <a href="https://references.modernisation.gouv.fr/referentiel-technique-0#contexte-du-lien">contexte du lien</a> permet d'en comprendre la fonction et la destination 


### Level

**A**

## Technical description

### Scope

**page**

### Decision level

**semidecidable**

## Algorithm

### Selection

##### Set1 :

All the `<a>` tags with a "href" attribute, with children (a[href]:has(*) )

##### Set2 :

All the elements of **Set1** with own text or with more than 1 child or with
only one child not of type img or object (where "img ,
object[type^=image], object[data^=data:image], object[data$=png],
object[data$=jpeg], object[data$=jpg],object[data$=bmp],
object[data$=gif]" returns empty)

##### Set3 :

All the elements of **Set2** with a not empty text and without context
(assuming [the definition of a link context in Rgaa3.2](https://references.modernisation.gouv.fr/referentiel-technique-0#contexte-du-lien))

##### Set4 :

All the elements of **Set2** with a not empty text, with a context (assuming
[the definition of a link context in Rgaa3.2](https://references.modernisation.gouv.fr/referentiel-technique-0#contexte-du-lien))

in other words :

size(**Set2**) = size(**Set3**) + size(**Set4**)

### Process

##### Test1

For each element of **Set3**, we check whether the link content doesn't
belong to the text link blacklist.

For each element returning false in **Test1**, raise a MessageA, raise a
MessageB instead

##### Test2

For each element of **Set4**, we check whether the link content doesn't
belong to the text link blacklist.

For each element returning false in **Test2**, raise a MessageC, raise a
MessageD instead

##### Test3

For each element of **Set3**, we check whether the link content doesn't only
contain non alphanumeric characters

For each element returning false in **Test3**, raise a MessageA, raise a
MessageB instead

##### Test4

For each element of **Set4**, we check whether the link content doesn't only
contain non alphanumeric characters

For each element returning false in Test4, raise a MessageC, raise a
MessageD instead

##### MessageA : Unexplicit Link

-   code : UnexplicitLink
-   status: Failed
-   parameter : link text, title attribute, snippet
-   present in source : yes

##### MessageB : Check link without context pertinence

-   code : CheckLinkWithoutContextPertinence
-   status: Need More Info
-   parameter : link text, title attribute, snippet
-   present in source : yes

##### MessageC : Unexplicit Link With context

-   code : UnexplicitLinkWithContext
-   status: Need More Info
-   parameter : link text, title attribute, snippet
-   present in source : yes

##### MessageD : Check link with context pertinence

-   code : CheckLinkWithContextPertinence
-   status: Need More Info
-   parameter : link text, title attribute, snippet
-   present in source : yes

### Analysis

#### Not Applicable

**Set2** is empty

#### Failed

At least one element of the **Set3** has a text content which is blacklisted (**Test1** returns false for at least one element)

#### Pre-qualified

In all other cases

## Notes

All the links that have children different from img or object, are considered as combined links.

examples :

-   `<a href="/target.html">` `<span>` my link`</span>``</a>`
-   `<a href="/target.html">` my `<span>`my link`</span>``</a>`
-   `<a href="/target.html">` `<p>`my link`</p>``</a>`
-   `<a href="/target.html">` my `<p>` link`</p>``</a>`

The properties `"aria-label"` and `"aria-labelledby"` are not taken in account to determine the context.

