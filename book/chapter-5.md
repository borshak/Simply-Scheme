# Глава 5
# Слова и предложения

Мы начали в первой части с примеров об акронимах и так далее, но с тех пор мы работали только с привычными числами. Это потому, что разговоры о процессе вычисления и об определении процедуры были достаточно сложными, чтобы вносить в них еще и дополнительные идеи. Но теперь мы готовы вернуться к символьному программированию.

We started out, in Part I, with examples about acronyms and so on, but since then we've been working with numbery old numbers. That's because the discussions about evaluation and procedure definition were complicated enough without introducing extra ideas at the same time. But now we're ready to get back to symbolic programming.

Как мы уже упоминали в главе 3, все, что вы вводите в Scheme, вычисляется, и результирующее значение распечатывается. Предположим, вы хотите использовать "**square**" в качестве слова в своей программе. Например, вы хотите, чтобы ваша программа решила проблему: «Дайте мне прилагательное, которое описывает Барри Манилоу». Если вы просто наберете "square" в Scheme, вы обнаружите, что квадрат - это процедура:

As we mentioned in Chapter 3, everything that you type into Scheme is evaluated and the resulting value is printed out. Let's say you want to use "square" as a word in your program. For example, you want your program to solve the problem, "Give me an adjective that describes Barry Manilow." If you just type square into Scheme, you will find out that square is a procedure:

```Scheme
> square
#<PROCEDURE>
(Различные версии Scheme могут по разному выводить информацию о внутреннем содержимом процедуры.)
(Different versions of Scheme will have different ways of printing out procedures.)
```
То, что вам нужно, - это способ указать, что вы хотите использовать слово «square», а не значение этого слова в качестве выражения. Способ сделать это - использовать ```quote```:

What you need is a way to say that you want to use the word "square" itself, rather than the value of that word, as an expression. The way to do this is to use quote:

```Scheme
> (quote square)
SQUARE

> (quote (tomorrow never knows))
(TOMORROW NEVER KNOWS)

> (quote (things we said today))
(THINGS WE SAID TODAY)
```

```quote``` представляет собой специальную форму, поскольку ее аргумент не вычисляется. Вместо этого она просто возвращает аргумент как есть.

Quote is a special form, since its argument isn't evaluated. Instead, it just returns the argument as is.

Программисты Scheme часто используют ```quote```, поэтому для нее есть сокращение:

Scheme programmers use quote a lot, so there is an abbreviation for it:

```Scheme
> 'square
SQUARE

> '(old brown shoe)
(old brown shoe)
```

(Поскольку Scheme использует апостроф в качестве сокращения для ```quote```, вы не можете использовать его в качестве обычного знака препинания в предложении. Именно поэтому мы избегали таких строк, как ```(can't buy me love)```. Для Scheme это будет означать ```(can (quote t) buy me love)```!) [1]

(Since Scheme uses the apostrophe as an abbreviation for quote, you can't use one as an ordinary punctuation mark in a sentence. That's why we've been avoiding titles like (can't buy me love). To Scheme this would mean (can (quote t) buy me love)!)[1]

Идея цитирования, хотя и может показаться немного оторванной от реальности в контексте компьютерного программирования, на самом деле хорошо знакома из обычного английского. Что такое книга? Это куча бумажек с напечатанными буквами, скрепленные между собой. Что такое «книга»? Это фраза, из существительного. Понимаете? Точно так же, что такое 2 + 3? Это 5. Что такое «2 + 3»? Это арифметическая формула. Когда вы видите слова в кавычках, вы понимаете, что вы должны думать о самих словах; вы не пыпаетесь оценить (вычислить) их значение. Scheme поступает так же.

This idea of quoting, although it may seem arbitrary in the context of computer programming, is actually quite familiar from ordinary English. What is a book? It's a bunch of pieces of paper, with printing on them, bound together. What is "a book"? It's a noun phrase, made up of an article and a noun. See? Similarly, what's 2+3? It's five. What's "2+3"? It's an arithmetic formula. When you see words inside quotation marks, you understand that you're supposed to think about the words themselves; you don't evaluate what they mean. Scheme is the same way.

(Не случайно дети, которые шутят, как

(It's no accident that kids who make jokes like

```
Мэтт: «Скажи свое имя.»
Matt: "Say your name."

Брайан: Свое имя.
Brian: "Your name."
```
вырастают программистами. Разница между вещью и ее названием является одной из важных идей, которые программисты должны понимать.)

grow up to be computer programmers. The difference between a thing and its name is one of the important ideas that programmers need to understand.)

## Селекторы

Пока все, что мы делали со словами и предложениями - цитировали их. Чтобы сделать что-то поинтереснее, нам нужны инструменты для двух видов операций: мы должны уметь разделять [слова и предложения], и мы должны уметь их комбинировать.[2] Начнем с инструментов для разделения; техническим термином для такой операции - селекторы.

So far all we've done with words and sentences is quote them. To do more interesting work, we need tools for two kinds of operations: We have to be able to take them apart, and we have to be able to put them together.[2] We'll start with the take-apart tools; the technical term for them is selectors.

```Scheme
> (first 'something)
S

> (first '(eight days a week))
EIGHT

> (first 910)
9

> (last 'something)
G

> (last '(eight days a week))
WEEK

> (last 910)
0

> (butfirst 'something)
OMETHING

> (butfirst '(eight days a week))
(DAYS A WEEK)

> (butfirst 910)
10

> (butlast 'something)
SOMETHIN

> (butlast '(eight days a week))
(EIGHT DAYS A)

> (butlast 910)
91
```

Обратите внимание, что первый элемент в предложении - слово, а первый элемент в слове - буква. (Но нет отдельного типа данных, называемого «letter», а буква равнозначна односимвольному слову.) ```butfirst``` в предложении - предложение, а ```butfirst``` в слове - слово. Соответствующие правила сохраняются для last и butlast.

Notice that the first of a sentence is a word, while the first of a word is a letter. (But there's no separate data type called "letter"; a letter is the same as a one-letter word.) The butfirst of a sentence is a sentence, and the butfirst of a word is a word. The corresponding rules hold for last and butlast.

Имена butfirst и butlast не предназначены для описания способов скольжения; єто сокращения «все, кроме первого» (all but the first) и «все, кроме последнего» (all but the last).

The names butfirst and butlast aren't meant to describe ways to sled; they abbreviate "all but the first" and "all but the last."

Вам может быть интересно, почему нам даются способы найти первый и последний элементы, но не 42-й элемент. Оказывается, имеющихся у нас достаточно, так как мы можем использовать эти примитивные селекторы для определения других:

You may be wondering why we're given ways to find the first and last elements but not the 42nd element. It turns out that the ones we have are enough, since we can use these primitive selectors to define others:

```Scheme
(define (second thing)
  (first (butfirst thing)))

> (second '(like dreamers do))
DREAMERS

> (second 'michelle)
I
```
Однако есть примитивный элемент селектора, который принимает два аргумента, число n и слово или предложение, и возвращает n-й элемент второго аргумента.

There is, however, a primitive selector item that takes two arguments, a number n and a word or sentence, and returns the nth element of the second argument.

```Scheme
> (item 4 '(being for the benefit of mister kite!))
BENEFIT

> (item 4 'benefit)
E
```

Не забывайте, что предложение, содержащее ровно одно слово, отличается от самого слова, а селекторы действуют по-разному:

Don't forget that a sentence containing exactly one word is different from the word itself, and selectors operate on the two differently:

```Scheme
> (first 'because)
B

> (first '(because))
BECAUSE

> (butfirst 'because)
ECAUSE

> (butfirst '(because))
()
```

Значение этого последнего выражения - пустое предложение. Вы можете сказать, что это предложение из-за круглых скобок, и вы можете сказать, что оно пустое, потому что между ними нет ничего.

The value of that last expression is the empty sentence. You can tell it's a sentence because of the parentheses, and you can tell it's empty because there's nothing between them.

```Scheme
> (butfirst 'a)
""

> (butfirst 1024)
"024"
```
Как показано в этих примерах, иногда butfirst возвращает слово, которое должно содержать метки двойных кавычек. Первый пример показывает пустое слово, в то время как второе показывает число, которое не находится в его обычной форме. (Его числовое значение 24, но вы обычно не видите ноль впереди.)

As these examples show, sometimes butfirst returns a word that has to have double-quote marks around it. The first example shows the empty word, while the second shows a number that's not in its ordinary form. (Its numeric value is 24, but you don't usually see a zero in front.)

```Scheme
> 024
24

> "024"
"024"
```

Мы постараемся избежать печатания этих забавных слов. Но не удивляйтесь, если увидите один из них как возвращаемое значение от одного из селекторов для слов. (Обратите внимание, что перед двойными кавычками не нужно ставить одиночную кавычку. Строки самооценки, как и числа.)

We're going to try to avoid printing these funny words. But don't be surprised if you see one as the return value from one of the selectors for words. (Notice that you don't have to put a single quote in front of the double quotes. Strings are self-evaluating, just as numbers are.)

Так как butfirst и butlast так трудно печатать, есть аббревиатуры bf и bl. Вы можете понять, что есть.

Since butfirst and butlast are so hard to type, there are abbreviations bf and bl. You can figure out which is which.

## Конструкторы

Функции для объединения вещей называются **конструкторами**. На данный момент у нас есть только два из них: ```word``` и ```sentence```. ```word``` принимает любое количество слов в качестве аргументов и объединяет их все вместе в одно громоздкое слово:

Functions for putting things together are called constructors. For now, we just have two of them: word and sentence. Word takes any number of words as arguments and joins them all together into one humongous word:

```Scheme
> (word 'ses 'qui 'pe 'da 'lian 'ism)
SESQUIPEDALIANISM

> (word 'now 'here)
NOWHERE

> (word 35 893)
35893
```

```sentence``` похоже, но немного отличается, так как оно может принимать как слова, так и предложения в качестве аргументов:

Sentence is similar, but slightly different, since it can take both words and sentences as arguments:

```Scheme
>  (sentence 'carry 'that 'weight)
(CARRY THAT WEIGHT)

> (sentence '(john paul) '(george ringo))
(JOHN PAUL GEORGE RINGO)
```

```sentence``` также слишком сложно набирать, поэтому есть аббревиатура se.

Sentence is also too hard to type, so there's the abbreviation se.

```Scheme
> (se '(one plus one) 'makes 2)
(ONE PLUS ONE MAKES 2)
```

Кстати, почему мы должны создавали цитаты в последнем примере, но не применяли цитирование к 2? Это потому, что числа самовычисляемы (вычисляются в самих себя), как мы говорили в главе 3. Мы должны цитировать ```make```, потому что иначе Scheme будет искать что-то с именем *make* вместо использования самого слова. Но числа не могут быть именами чего-либо; они представляют сами себя. (На самом деле, вы могли бы процитировать 2, и это не имело бы никакого значения - вы понимаете, почему?)

By the way, why did we have to quote makes in the last example, but not 2? It's because numbers are self-evaluating, as we said in Chapter 3. We have to quote makes because otherwise Scheme would look for something named makes instead of using the word itself. But numbers can't be the names of things; they represent themselves. (In fact, you could quote the 2 and it wouldn't make any difference—do you see why?)

### Слова и Предложения как объекты первого рода

Если Scheme не является вашим первым языком программирования, вы, вероятно, привыкли иметь дело с английским текстом на компьютере совершенно по-другому. Многие другие языки рассматривают предложение, например, как просто набор («строку») символов, таких как буквы, пробелы и знаки препинания. Эти языки не помогают вам поддерживать двухуровневый характер английского текста, в котором предложение составлено из слов, а слово состоит из букв.

If Scheme isn't your first programming language, you're probably accustomed to dealing with English text on a computer quite differently. Many other languages treat a sentence, for example, as simply a collection (a "string") of characters such as letters, spaces, and punctuation. Those languages don't help you maintain the two-level nature of English text, in which a sentence is composed of words, and a word is composed of letters.

Исторически, компьютеры просто занимались цифрами. Можно добавить два числа, переместить число из одного места в памяти компьютера в другое место и так далее. Поскольку каждая инструкция на родном машинном языке компьютера не могла обрабатывать ничего большего, чем число, программисты развивали отношение, что один номер является «реальной вещью», в то время как все более сложное должно рассматриваться как совокупность вещей, а не как Одна вещь сама по себе.

Historically, computers just dealt with numbers. You could add two numbers, move a number from one place in the computer's memory to another place, and so on. Since each instruction in the computer's native machine language couldn't process anything larger than a number, programmers developed the attitude that a single number is a "real thing" while anything more complicated has to be considered as a collection of things, rather than as a single thing in itself.

Компьютер представляет собой текстовый символ в виде одного числа. Поэтому во многих языках программирования символ является «реальной вещью», но слово или предложение понимается только как совокупность этих номеров символьных кодов.

The computer represents a text character as a single number. In many programming languages, therefore, a character is a "real thing," but a word or sentence is understood only as a collection of these character-code numbers.

Но это не тот способ, которым люди обычно думают о своем собственном языке. Для вас слово не является в первую очередь строкой символов (хотя, если вы соревнуетесь в орфографической пчеле, это слово может временно выглядеть как одно). Это больше похоже на единицу значения. Аналогично, предложение - это лингвистическая структура, части которой являются словами, а не буквами и пробелами.

But this isn't the way in which human beings normally think about their own language. To you, a word isn't primarily a string of characters (although it may temporarily seem like one if you're competing in a spelling bee). It's more like a single unit of meaning. Similarly, a sentence is a linguistic structure whose parts are words, not letters and spaces.

Язык программирования должен позволять вам выражать свои идеи в терминах, которые соответствуют вашему образу мышления, а не способу компьютера. Технически мы говорим, что слова и предложения должны быть первоклассными данными на нашем языке. Это означает, что предложение, например, может быть аргументом процедуры; Это может быть значение, возвращаемое процедурой; Мы можем дать ему имя; И мы можем строить агрегаты, элементами которых являются предложения. До сих пор мы видели, как сделать первые два из них. Мы закончим работу в главе 7 (о переменных) и главе 17 (в списках).

A programming language should let you express your ideas in terms that match your way of thinking, not the computer's way. Technically, we say that words and sentences should be first-class data in our language. This means that a sentence, for example, can be an argument to a procedure; it can be the value returned by a procedure; we can give it a name; and we can build aggregates whose elements are sentences. So far we've seen how to do the first two of these. We'll finish the job in Chapter 7 (on variables) and Chapter 17 (on lists).

## Ловушки

Мы избегали апострофов в наших словах и предложениях, потому что они являются сокращениями специальной формы цитаты. Вы также должны избегать точек, запятых, точек с запятой, кавычек, вертикальных полос и, конечно, скобок, поскольку все они имеют специальные значения в схеме. Однако вы можете использовать вопросительные знаки и восклицательные знаки.

We've been avoiding apostrophes in our words and sentences because they're abbreviations for the quote special form. You must also avoid periods, commas, semicolons, quotation marks, vertical bars, and, of course, parentheses, since all of these have special meanings in Scheme. You may, however, use question marks and exclamation points.

Хотя мы уже упоминали необходимость избегать имен примитивов при выборе формальных параметров, мы хотим напомнить вам конкретно о названии слова и предложения. Часто это очень заманчивые формальные параметры, потому что многие процедуры имеют слова или предложения в качестве своих доменов. К сожалению, если вы выберете эти имена для параметров, вы не сможете использовать соответствующие процедуры в своем определении.

Although we've already mentioned the need to avoid names of primitives when choosing formal parameters, we want to remind you specifically about the names word and sentence. These are often very tempting formal parameters, because many procedures have words or sentences as their domains. Unfortunately, if you choose these names for parameters, you won't be able to use the corresponding procedures within your definition.

```Scheme
(define (plural word)                        ;; wrong!
  (word word 's))

> (plural 'george)
ERROR: GEORGE isn't a procedure
```

Результат замещения не был, как вы могли подумать,

The result of substitution was not, as you might think,

```Scheme
(word 'george 's)

but rather

('george 'george 's)
```

Мы использовали wd и отправляли формальные параметры вместо слова и предложения, и мы рекомендуем эту практику.

We've been using wd and sent as formal parameters instead of word and sentence, and we recommend that practice.

Есть разница между словом и предложением из одного слова. Например, люди часто попадают в ловушку, думая, что первое из двухсловных предложений, таких как (секси-сади), является вторым словом, но это не так. Это однословное предложение. Например, его счет один, а не пять. [3]

There's a difference between a word and a single-word sentence. For example, people often fall into the trap of thinking that the butfirst of a two-word sentence such as (sexy sadie) is the second word, but it's not. It's a one-word-long sentence. For example, its count is one, not five.[3]

```Scheme
> (bf '(sexy sadie))
(SADIE)

> (first (bf '(sexy sadie)))
SADIE
```

Мы упоминали ранее, что иногда Scheme приходится помещать двойные кавычки вокруг слов. Просто игнорируйте их; Не расстраивайтесь, если ваша процедура возвращает «6-сердец» вместо 6-сердец.

We mentioned earlier that sometimes Scheme has to put double-quote marks around words. Just ignore them; don't get upset if your procedure returns "6-of-hearts" instead of just 6-of-hearts.

Цитата не означает «печать». Некоторые люди смотрят на такие взаимодействия:

Quote doesn't mean "print." Some people look at interactions like this:

```Scheme
> '(good night)
(GOOD NIGHT)
```

И думаю, что кавычка была инструкцией, сообщающей схеме о том, что следует за ней. На самом деле, Scheme всегда печатает значение каждого выражения, которое вы вводите, как часть цикла read-eval-print. В этом случае значением всего выражения является подвыражение, которое цитируется, а именно предложение (спокойной ночи). Это значение не будет напечатано, если бы цитата была частью некоторого более крупного выражения:

and think that the quotation mark was an instruction telling Scheme to print what comes after it. Actually, Scheme always prints the value of each expression you type, as part of the read-eval-print loop. In this case, the value of the entire expression is the subexpression that's being quoted, namely, the sentence (good night). That value wouldn't be printed if the quotation were part of some larger expression:

```Scheme
> (bf '(good night))
(NIGHT)
```

Если вы видите сообщение об ошибке, подобное

If you see an error message like

```Scheme
> (+ 3 (bf 1075))
ERROR: INVALID ARGUMENT TO +: "075"
```

попробуйте ввести выражение

try entering the expression

```Scheme
> (strings-are-numbers #t)
OKAY
```

и попробуй еще раз. (Расширение Схемы, позволяющее арифметическим операциям работать с нестандартными числами типа «075», делает обычную арифметику медленнее, чем обычно.Так мы предусмотрели способ включения и выключения расширения. Вызов строк - это числа с аргументом # F отключает расширение.) [4]

and try again. (The extension to Scheme that allows arithmetic operations to work on nonstandard numbers like "075" makes ordinary arithmetic slower than usual. So we've provided a way to turn the extension on and off. Invoking strings-are-numbers with the argument #f turns off the extension.)[4]

## Скучные упражнения

5.1 Какие значения печатаются при вводе этих выражений в Scheme? (Запомните это перед тем, как попробовать на компьютере.)

5.1  What values are printed when you type these expressions to Scheme? (Figure it out in your head before you try it on the computer.)

```Scheme
(sentence 'I '(me mine))

(sentence '() '(is empty))

(word '23 '45)

(se '23 '45)

(bf 'a)

(bf '(aye))

(count (first '(maggie mae)))

(se "" '() "" '())

(count (se "" '() "" '()))
```

5.2. Для каждого из следующих примеров напишите процедуру из двух аргументов, которая при применении к образцам аргументов возвращает результат выборки. Ваши процедуры могут не включать данные, указанные в кавычках.

5.2  For each of the following examples, write a procedure of two arguments that, when applied to the sample arguments, returns the sample result. Your procedures may not include any quoted data.

```Scheme
> (f1 '(a b c) '(d e f))
(B C D E)

> (f2 '(a b c) '(d e f))
(B C D E AF)

> (f3 '(a b c) '(d e f))
(A B C A B C)

> (f4 '(a b c) '(d e f))
BE
```
5.3 Объясните разницу в значении между (первый «мезонин» и «первый» (мезонин)).

5.3  Explain the difference in meaning between (first 'mezzanine) and (first '(mezzanine)).

5.4 Объясните разницу между двумя выражениями (первая (квадрат 7)) и (первая '(квадрат 7)).

5.4  Explain the difference between the two expressions (first (square 7)) and (first '(square 7)).

5.5 Объясните разницу между (слово 'a' b 'c) и (se' a 'b' c).

5.5  Explain the difference between (word 'a 'b 'c) and (se 'a 'b 'c).

5.6 Объясните разницу между (bf 'забадак) и (но сначала «забабак»).

5.6  Explain the difference between (bf 'zabadak) and (butfirst 'zabadak).

5.7 Объясните разницу между (bf 'x) и (butfirst' (x)).

5.7  Explain the difference between (bf 'x) and (butfirst '(x)).

5.8 Какие из следующих предложений являются законными?

5.8  Which of the following are legal Scheme sentences?

```Scheme
(here, there and everywhere)
(help!)
(all i've got to do)
(you know my name (look up the number))
```

5.9. Выясните, какие значения каждое из следующих будет возвращено до того, как вы попробуете их на компьютере:

5.9  Figure out what values each of the following will return before you try them on the computer:

```Scheme
(se (word (bl (bl (first '(make a))))
          (bf (bf (last '(baseball mitt)))))
    (word (first 'with) (bl (bl (bl (bl 'rigidly))))
          (first 'held) (first (bf 'stitches))))

(se (word (bl (bl 'bring)) 'a (last 'clean))
    (word (bl (last '(baseball hat))) (last 'for) (bl (bl 'very))
	  (last (first '(sunny days)))))
```

5.10. Какие аргументы вы можете дать прежде всего, чтобы он возвращал слово? Предложение?

5.10  What kinds of argument can you give butfirst so that it returns a word? A sentence?

5.11. Какие аргументы вы можете дать последнему, чтобы он возвращал слово? Предложение?

5.11  What kinds of argument can you give last so that it returns a word? A sentence?

5.12 Какие из функций first, last, butfirst и butlast могут возвращать пустое слово? За какие аргументы? Как насчет возвращения пустого предложения?

5.12  Which of the functions first, last, butfirst, and butlast can return an empty word? For what arguments? What about returning an empty sentence?

## Реальные упражнения

5.13 Что означает «банан»?

5.13  What does ' 'banana stand for?

Что такое (первый «банан») и почему?

What is (first ' 'banana) and why?

5.14. Напишите третью процедуру, которая выбирает третью букву слова (или третье слово предложения).

5.14  Write a procedure third that selects the third letter of a word (or the third word of a sentence).

5.15. Напишите процедуру сначала два, которая принимает слово в качестве аргумента, возвращая двухбуквенное слово, содержащее первые две буквы аргумента.

5.15   Write a procedure first-two that takes a word as its argument, returning a two-letter word containing the first two letters of the argument.

```Scheme
> (first-two 'ambulatory)
AM
```

5.16. Напишите процедуру два первых, которая принимает два слова в качестве аргументов, возвращая двухбуквенное слово, содержащее первые буквы двух аргументов.

5.16  Write a procedure two-first that takes two words as arguments, returning a two-letter word containing the first letters of the two arguments.

```Scheme
> (two-first 'brian 'epstein)
BE
```

Теперь напишите процедуру two-first-sent, которая принимает предложение из двух слов в качестве аргумента, возвращая двухбуквенное слово, содержащее первые буквы этих двух слов.

Now write a procedure two-first-sent that takes a two-word sentence as argument, returning a two-letter word containing the first letters of the two words.

```Scheme
> (two-first-sent '(brian epstein))
BE
```

5.17. Напишите процедуру рыцаря, который берет имя человека в качестве аргумента и возвращает имя с "Sir" перед ним.

5.17  Write a procedure knight that takes a person's name as its argument and returns the name with "Sir" in front of it.

```Scheme
> (knight '(david wessel))
(SIR DAVID WESSEL)
```

5.18. Попробуйте следующее и объясните результат:

5.18  Try the following and explain the result:

```Scheme
(define (ends word)
  (word (first word) (last word)))

> (ends 'john)
```

5.19 Напишите вставку процедуры - и она принимает предложение из элементов и возвращает новое предложение с «и» в нужном месте:

5.19  Write a procedure insert-and that takes a sentence of items and returns a new sentence with an "and" in the right place:

```Scheme
> (insert-and '(john bill wayne fred joey))
(JOHN BILL WAYNE FRED AND JOEY)
```

5.20. Определите процедуру поиска чьего-либо имени:

5.20  Define a procedure to find somebody's middle names:

```Scheme
> (middle-names '(james paul mccartney))
(PAUL)

> (middle-names '(john ronald raoul tolkien))
(RONALD RAOUL)

> (middle-names '(bugs bunny))
()

> (middle-names '(peter blair denis bernard noone))
(BLAIR DENIS BERNARD)
```

5.21. Напишите запрос процедуры, который превращает утверждение в вопрос, поменяв первые два слова и добавив вопросительный знак к последнему слову:

5.21  Write a procedure query that turns a statement into a question by swapping the first two words and adding a question mark to the last word:

```Scheme
> (query '(you are experienced))
(ARE YOU EXPERIENCED?)

> (query '(i should have known better))
(SHOULD I HAVE KNOWN BETTER?)
```

[1] Собственно говоря, можно поставить знаки препинания внутри слов, пока все слово заключено в двойные кавычки, например:

[1] Actually, it is possible to put punctuation inside words as long as the entire word is enclosed in double-quote marks, like this:

```Scheme
> '("can't" buy me love)
("can't" BUY ME LOVE)
```

Такие слова называются струнами. Мы не собираемся использовать их в каких-либо примерах, пока не закончится книга. Держитесь подальше от знаков препинания, и у вас не будет неприятностей. Однако, вопросительные знаки и восклицательные знаки в порядке. (Обычные слова, те, которые не являются ни строками, ни номерами, официально называются символами.)

Words like that are called strings. We're not going to use them in any examples until almost the end of the book. Stay away from punctuation and you won't get in trouble. However, question marks and exclamation points are okay. (Ordinary words, the ones that are neither strings nor numbers, are officially called symbols.)

[2] Процедуры, которые мы собираемся показать вам, не являются частью стандартной официальной Схемы. Схема действительно предоставляет способы сделать это, но обычные способы несколько более сложны и подвержены ошибкам для новичков. Мы предоставили более простой способ сделать символические вычисления, используя идеи, разработанные как часть языка программирования Logo.

[2] The procedures we're about to show you are not part of standard, official Scheme. Scheme does provide ways to do these things, but the regular ways are somewhat more complicated and error-prone for beginners. We've provided a simpler way to do symbolic computing, using ideas developed as part of the Logo programming language.

[3] Вы встретили ```count``` в главе 2. Он принимает слово или предложение в качестве аргумента, возвращая либо количество букв в слове, либо количество слов в предложении.

[3] You met count in Chapter 2. It takes a word or sentence as its argument, returning either the number of letters in the word or the number of words in the sentence.

[4] Более полное объяснение см. В приложении А.

[4] See Appendix A for a fuller explanation.
