relational-parsing-with-derivatives
===================================

Relational version of parsing with derivatives code, based on Matt Might's blog posts:

http://matt.might.net/articles/implementation-of-regular-expression-matching-in-scheme-with-derivatives/

and

http://matt.might.net/articles/parsing-with-derivatives/

This repo curently contains five versions of the derivatives code:

1. Matt Might's original Scheme code for regular expression matching, from the first blog post above.  This code is actually for an acceptor, rather than a parser.

2. Will Byrd's Scheme version of Matt's code, using pattern matching rather than 'cond', and with other tweaks to make it easier to convert to miniKanren.

3. Will Byrd's relational version of the regular expression matcher, written in core miniKanren extended with disequality constraints (=/=) and the symbolo type predicate.

4. Nada Amin's Clojure version of the regular expression matcher, written in core.logic.

5. Will Byrd's Super Chobo port of the relational regular expression matcher to Racket.  This version works, but neets some serious Racketization, especially as all the code is shoved into one giant file.  Please halp!

Currently, version 3 should be considered canonical.

The relational matcher can determine whether a "string" (really a sequence of symbols) matches a regular expression.  In addition, the matcher can *generate* strings that match a given regex.  Also, the matcher can generate strings that *don't* match a given regex (which is pretty cool, actually).  Furthermore, the matcher can generate regex that accept a given string, and can generate regex that *don't* match a given string.  Of course, the relation also works when all of its arguments are fresh logic variables.

Obvious TODO's:

1. Fix a problem in the miniKanren code with how terms are destructured.  For example, (rep regex-BLANK) shouldn't be a legal term, since it should be simplified to regex-BLANK.  The current code can generate this term when running backwards. [UPDATE: this has been fixed in the miniKanren version, using helper relations with disequality constraints, but not yet ported to the other implementations.]

2. Try to find a canonical representation for regex; for example, (alt re1 re2) and (alt re2 re1) should be represented by a single canonical term.  Seems like this is only an issue for alt terms.  Might need a term ordering goal (or better yet, constraint): (term<o t1 t2).

3. See if regex-matcho can be simplified. [UPDATE: this has been fixed in the miniKanren version, with deltao now returning #f or #t instead of sometimes returning an alt or seq list. This improvement has not yet ported to the other implementations.]

4. Explore different goal orderings (and possibly add bounds relations to prune the search tree when appropriate) to speed up code when running backwards, and to ensure run*'s terminate when there are finitely many answers.

5. Implement full parsing rather than just matching (right now the title for the repo is misleading!).

6. See if tabling will allow the code to handle CFGs, not just regular languages.  This may require implementing coinductive logic programming.  This may be accomplished more easily in core.logic, since miniKanren's tabling implementation doesn't yet support constraints.

7. Clean up/Racketize the Super Chobo Racket implementation.

8. Implement Matt's derivatives based lexer as well.  Do both lexing and parsing relationally, as a pipeline.  What interesting things can you do when running backwards?

See the comments in the Scheme and miniKanren code for other TODO's and open questions.

All Scheme/miniKanren code was tested with Petite Chez Scheme 8.4.