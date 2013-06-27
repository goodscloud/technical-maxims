Technical Maxims
================

This repository contains the technical & engineering maxims for the programming team at [GoodsCloud](http://goodscloud.net/).

These are not rules, nor are they guidelines. Rather, they are called "maxims", after the maxims followed by people in the fourth and fifth level of [the Dreyfus model of skill acquisition](http://en.wikipedia.org/wiki/Dreyfus_model_of_skill_acquisition).

We have published it so that other organizations can benefit from our experience, and so that people can see what it's like to work here.

Pull requests are welcome.

## Coding

* Naming things is hard:
 * Don't be afraid of long, descriptive variable names. That's why your editor has tab completion. Optimize for comprehension, not keystrokes.
 * The length of a variable name should be inversely proportional to its scope.
 * Avoid acronyms and abbreviations, unless they are unambiguous and widely understood, like `HTTP` or `sync`.
 * Avoid plurals. Better to use `item_list` than `items`, to distinguish it from `item`.
 * Avoid negatives and [double-negatives](http://zvon.org/xxl/XSLTreference/Output/xslt_text_disable-output-escaping.html) in names and default values. Better to use `escape=True` than `disable-output-escaping=False`.
 * Try to make function names unambiguously verb-y and variable names unambiguously noun-y: `delete(result)` not `trash(query)`
 * Don't use language built-ins as identifiers. This includes `id`, `type`, `filter`, and all other [Python built-ins](http://docs.python.org/2/library/functions.html).
 * Strive for consistency in naming, use the same variable names for the same kinds of objects across different functions/modules.
* Comment things that are confusing or weird. Don't comment obvious things.
* Version control is for remembering old or unused code. Comments are *not* for remembering old or unused code. Be ruthless about deleting old or unused code.
* Don't copy and paste code. Ever. No, not even just this one time. Write a little function, use a template, or [fragments](https://github.com/glyphobet/fragments).
* Subclass, inherit, wrap, @decorate, encapsulate. But do not patch the monkey. And do not punch the duck.
* Think of the novice developer who will inherit your code in four years, after you, your documentation, and everyone you worked with are long gone. Code with that poor sucker in mind.
* Separate your ego from your code. Everyone writes bugs, that's why we have tests.
  * Separate other people from their code. Don't blame or point fingers when someone else writes a bug. Help fix it, write a test, and move on.
* Avoid feelings of ownership, control, or dominion over certain parts of the code. Anybody can fix bugs, modify, or rewrite any part of the code, as long as you are making it better. Better is defined by the whole team.
* Avoid ambiguity. For CoffeeScript, always use:
  * parentheses around function calls: `func(arg)` instead of `func arg`.
  * curly brackets around literal objects: `{key: 'val', key: 'val'}` instead of `key: 'val', key: 'val'`.
* Use double quotes `"` for human-readable, natural language text.
* Use single quotes `'` for machine-readable, string keys and unique identifiers.
* Trailing whitespace causes noise in diffs, both when it's added and when it's removed. Make sure your editor strips it out, or install a pre-commit hook to strip it.
* Follow coding style for your language:
  * Python code should follow [PEP 8](http://www.python.org/dev/peps/pep-0008/).
  * JavaScript code should follow [Crockford's conventions](http://javascript.crockford.com/code.html).
  * HTML code should follow [this guide](http://github.com/goodscloud/technical-maxims/wiki/HTML-Styleguide).
  * CoffeeScript code should follow [this guide](https://github.com/polarmobile/coffeescript-style-guide).
  * Exception: for both languages, occasional long lines, up to about 160 characters, are ok. Eighty character TTYs are a thing of the past.

## Database design

* Tables that need to be referred to by foreign keys should have a primary key, named `id`. Foreign keys pointing at composite primary keys are too complex.
* All tables should have a column marked `unique`, or a multi-column `UniqueConstraint` describing the real-world value(s) that make this data unique. Do not succumb to [Primary Keyvil](http://it.toolbox.com/blogs/database-soup/primary-keyvil-part-i-7327).
* Foreign keys end with `_id`.
* `label` columns are machine-readable unique identifiers, quite often (but not always) they are *not* `NULL`-able.
* Avoid overly vague terms in column and table names. They can be redundant and/or confusing. Examples include, but are not limited to: `value`, `length`, `status`, `store`, &c.
* Don't use `table` or `column` in table or column names. Nobody wants to write `select stuff_column from thing_table;`.
* Do not include the table name, or part of the table name, in column identifiers. `select stuff from thing;` is better than `select thing_stuff from thing;`.
* Values that function as unique identifiers when communicating with external systems are named `external_identifier`. Exception: standard industry terms like `sku` ("stock-keeping unit") can be used, if they exist.
* Columns that come from an externally defined code specification are named with `_code`: `language_code`, `country_code`, `region_code`. Exception: the externally defined list of timezones do not have codes, thus they are named `timezone_name`.
* Boolean columns should be prefixed with `is_` if it's not obvious from their name that they are booleans. Example: `enabled` is fine, but `is_warehouse` is better than just `warehouse`.
* Only allow `NULL`s in columns when it makes sense for that column type. For example, you might want to `NULL` an `external_identifier` when there is no corresponding item in any external system. But you shouldn't allow `NULL`s in a `price` column because there's no difference between a price of `0` and a `NULL` price.
* Don't store passwords in cleartext. Use [bcrypt](http://codahale.com/how-to-safely-store-a-password/).

## Dependencies

* Dependencies stay separate. Do not commit them, that might violate their licenses, and encourages monkey- and duck- patching and punching.
* Install dependencies with the package manager for that language:
  * Install them with `pip` and list them in `requirements.txt` (Python).
* Use virtual environments whenever possible. Example: `virtualenv` (Python) and `npm` (JavaScript).
* Use `aptitude` (deployment) or MacPorts/Homebrew (development) to install other dependencies (Apache, PostgreSQL, etc.).
* Thou shalt never `sudo easy_install ...` or `sudo setup.py install`, lest a lightning bolt strike you down as you hack.

## Testing

* Always make sure all the tests pass before committing.
* Always commit and push your code before you leave for the day. If you're not done, push it to a branch.
* If you break the build for other developers, you have to stop everything you're doing and fix it ASAP. Yes, including eating lunch.
  * Alternatively, roll back your changes on master and fix the tests in a branch.
* Mock external resources in the tests. The tests should be able to pass when you are coding at thirty-thousand feet over the Atlantic with no internet connection.

## Committing

* Finished code is code with complete test coverage.
* Commit early, commit often.
* Use [Tower](http://www.git-tower.com/) or `git commit -p` to commit small, self-contained changes separately. [If your commit message has bullet points, your commit is too big.](http://twitter.com/glyphobet/status/290798617663533056)
* Once passwords, [private keys](https://github.com/search?q=-----BEGIN+RSA+PRIVATE+KEY-----&ref=searchresults&type=Code), credit card numbers, authentication tokens, or other things that shouldn't be committed are in history, they are there forever. Don't commit them.

## Debugging

* Broken tests? `git bisect` is your friend. Especially if you run the tests before you commit, every time.
* The Python debugger is your friend. Use it. Install [a pre-commit hook](https://gist.github.com/glyphobet/3128700) to prevent you from committing `pdb.set_trace()`s.

## Planning

* If you are having trouble estimating something, split it up into smaller components until you aren't having trouble estimating anymore.
* If you are not sure whether something is possible, try to implement a proof-of-concept of the most difficult/problematic part.
* Track technical debt with tickets.
