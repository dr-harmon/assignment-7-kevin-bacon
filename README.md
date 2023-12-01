# Assignment 7: Six Degrees of Kevin Bacon

[Six degrees of separation](https://en.wikipedia.org/wiki/Six_degrees_of_separation) is the idea that all people are six or fewer social connections away from each other. As a result, a chain of "friend of a friend" statements can be made to connect any two people in a maximum of six steps.

In 1994, this idea was popularized as a party game called [Six Degrees of Kevin Bacon](https://en.wikipedia.org/wiki/Six_Degrees_of_Kevin_Bacon). Players challenge each other to arbitrarily choose an actor and then connect them to another actor via a film that both actors have appeared in together, repeating this process to try to find the shortest path that ultimately leads to actor Kevin Bacon.

In this assignment, you are to create a "Bacon finder" based on this game. Given the name of an actor, your program should find the shortest path that leads to Kevin Bacon. For example, Dustin Hoffman and Tom Cruise starred in "Rain Man", and Tom Cruise and Kevin Bacon starred in "A Few Good Men." Therefore, if given the name "Dustin Hoffman", your program should print:

    The Bacon number of Dustin Hoffman is 2:
      Dustin Hoffman -> Rain Man -> Tom Cruise -> A Few Good Men -> Kevin Bacon

## Data Set

A data set of actors and movies is included with the project in the `data` directory. It is filtered to include only actors considered to be one of the movie's "principal" actors, according to [IMDb](https://www.imdb.com). It also includes only movies released in 1973 or later to reduce the data size.

To avoid the duplicate name problem, the data set assigns a unique ID to every movie and actor. For example, Kevin Bacon is `nm0000102` and "Footloose" is `tt0087277`. Use the IDs instead of the names for your computations.

## Breadth First Search

To find the Bacon path, you must use the provided graph ADT and `BFS` subclass. The structure is similar to the `DFS` subclass of Lab 45, but the `traverse` method is unimplemented. Fill in this method and make sure that all unit tests in `bfs_test.cpp` pass.

## Bacon Finder

Once BFS is working, your next task is to implement the "Bacon finder". A `BaconFinder` class that inherits from `BFS` is already provided. It includes a method called `addRelationship` for adding vertices and edges to the actor graph. Note that the vertices may be either the name of an actor _or_ the name of a movie.

The `getBaconPath` method is unimplemented. This method should traverse the actor graph and find a _shortest_ path from the given actor to Kevin Bacon. Fill in this method so that the provided `BaconFinder` unit test passes.

You should also test your code with the `main` target. For example, if you run the `main` target as-is, it should print the Bacon path for Dustin Hoffman as in the example above. Likewise, if you change the `actor` string to `nm1503403` (Shane Carruth), your program should print:

    The Bacon number of Shane Carruth is 4:
      Shane Carruth -> Upstream Color -> Frank Mosley -> Collective: Unconscious -> James Adomian -> Comics Open -> Paul Rodriguez -> Quicksilver -> Kevin Bacon

(Shane Carruth is one of the few actors with a Bacon number greater than 3.)

What is the Bacon number of your favorite actor? You can find actor IDs in the `data/names.tsv` file.

## Hints

* The BFS algorithm described in textbook section 13.3.5 is accurate but lacking certain details. Feel free to search online for additional insight and examples of BFS. In particular, the pseudocode in [Wikipedia's article](https://en.wikipedia.org/wiki/Breadth-first_search) is quite useful, especially its technique for keeping track of vertex parents to reconstruct shortest paths.
* To compare two vertices for equality, you can compare their elements using the overloaded dereference operator. For example: `if ((*v).element == targetActor) { ... }`.
* Remember to use references whenever you need to modify the state of the graph. For example, `Vertex w = edge.opposite(v)` will return a _copy_ of the vertex, and therefore `(*v).element = ...` will modify only that copy and not the vertex data within the graph. Instead, do `Vertex& w = edge.opposite(v)`.
* The C++ compiler can get confused when nested classes have templates. For example, it may think the type is undefined when declaring `vector<G::Vertex>`. To resolve this, simply add `typename` in front of the type: `vector<typename G::Vertex>`.
