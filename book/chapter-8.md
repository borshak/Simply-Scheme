Part III

Functions as Data

By now you're accustomed to the idea of expressing a computational process in terms of the function whose value you want to compute, rather than in terms of a sequence of actions. But you probably think of a function (or the procedure that embodies it) as something very different from the words, sentences, numbers, or other data that serve as arguments to the functions. It's like the distinction between verbs and nouns in English: A verb represents something to do, while a noun represents something that is.

In this part of the book our goal is to overturn that distinction.

Like many big ideas, this one seems simple at first. All we're saying is that a function can have functions as its domain or range. One artificially simple example that you've seen earlier was the number-of-arguments function in Chapter 2. That function takes a function as argument and returns a number. It's not so different from count, which takes a word or sentence as argument and returns a number.

But you'll see that this idea leads to an enormous rise in the length and complexity of the processes you can express in a short procedure, because now a process can give rise to several other processes. A typical example is the acronym procedure that we introduced in Chapter 1 and will examine now in more detail. Instead of applying the first procedure to a single word, we use first as an argument to a procedure, every, that automatically applies it to every word of a sentence. A single every process gives rise to several first processes.

The same idea of function as data allows us to write procedures that create and return new procedures. At the beginning of Part II we showed a Scheme representation of a function that computes the third person singular of a verb. Now, to illustrate the idea of function as data, we'll show how to represent in Scheme a function make-conjugator whose range is the whole family of verb-conjugation functions:

(define (make-conjugator prefix ending)
  (lambda (verb) (sentence prefix (word verb ending))))
Never mind the notation for now; the idea to think about is that we can use make-conjugator to create many functions similar to the third-person example of the Part II introduction:

> (define third-person (make-conjugator 'she 's))

> (third-person 'program)
(SHE PROGRAMS)

> (define third-person-plural-past (make-conjugator 'they 'ed))

> (third-person-plural-past 'play)
(THEY PLAYED)

> (define second-person-future-perfect
    (make-conjugator '(you will have) 'ed))

> (second-person-future-perfect 'laugh)
(YOU WILL HAVE LAUGHED)
We'll explore only a tiny fraction of the area opened up by the idea of allowing a program as data. Further down the same road is the study of compilers and interpreters, the programs that translate your programs into instructions that computers can carry out. A Scheme compiler is essentially a function whose domain is Scheme programs.
