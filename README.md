# Protohype
A prototyping language useful for rapid styling and mocking layouts. Essentially a set of aliases for commonly used CSS key/value pairs that (with a little extra cognitive overhead) helps speed up development. Intended for use with or without a preprocessor such as LESS, SASS or SCSS.

In general, protohype directives are simple abbreviations of their CSS counterparts. For example:<br>

- `justify-content: space-between` is aliased to `jcsb`
- `align-items: flex-start` is aliased to `aifs`
- `text-align: center` is aliased to `tac`

There are certain cases where simple abbreviation of the key/value is not feasible, these include:<br>

- Properties with unlimited valid numerical values such as `margin` and `padding`.<br><br>
In cases like this only the property is abbreviated and the value is casted to some binary value where `on` is a sensible default such as `1rem` and `off` is `none` or `0`. For example `mt` is the alias for `margin-top: 1rem` and `mtn` is the alias for `margin-top: 0`. A suffix of `n` in these cases, denotes a `null` value. A suffix of `a` denotes an `auto` value. The likes of `mt1` or `mt-1` are dismissed on the grounds of being	inexhaustible (if `mt1` is defined, then why not `mt2`, `mt3`, etc.)

- Properites with multiple values such as `border`<br><br>
In cases like this, again, only the property is abbreviated and a sensible default value is assumed. For example, in the current version of this library `bt` is the alias for `border-top: 1px solid #000`.

- Common pairings such as `min-height: 100vh`<br><br>
In cases like this the aliases get a little less succinct, more opinionated and therefore just have to be learnt or looked up. Such directives should arguably be excluded from the library but are so commonly used that they warrent inclusion. For the given example the alias is `vh`.

- Common compound rules such as `{ display: flex; flex-direction: column; }`<br><br>
In cases like this, where one rule is a prerequsite to another or is often used in conjunction with, then an alias is created for the pair, usually based on the expected behaviour of the combination. For the given example the alias is `col`.


## Usge without a preprocessor
All protohype directives can be applied to elements directly as attribute selectors. The advantages of this is that no preprocessor is required and that the developer can tell at a glance what styles will be applied to an element when it is rendered. The main disadvantage of this approach is more cluttered markup (but arguably not as cluttered as using inline styles).

```html
<nav row aic bb >
  <li p >Home</li>
  <li p >Profile</li>
  <li p mla >Logout</li>
</nav>
```

The above snippet is the equivelant of writing the following CSS

```
nav {
  display: flex;
  flex-direction: row;
  align-items: center;
  border-bottom: 1px solid #000;
}

li {
  padding: 1rem;
}

li:last-child {
  margin-left: auto;
}
```

## Usage with a preprocessor
If you use SASS, LESS or another preprocessor then you can move all the protohype directives out of your markup and into your stylesheets. This makes for much cleaner templates and more concise stylesheets. Using `@extend` funtionality is one of the easiest ways to get started.

```html
<nav>
  <li>Home</li>
  <li>Profile</li>
  <li>Logout</li>
</nav>
```

To style the above snippet in the same way as the previous example, looks as follows

```
nav { 
  @extend [row],[aic],[bb];
}
li {
  @extend [p];
}
li:last-child {
  @extend [mla];
}
```

# Conclusion
There are some pros and cons of using this library in both modes (or not at all). I am not usually a fan of the *just shorten all the variable names* approach to making code cleaner because often there is a tradeoff for readability and maintainability. The thing is I got so tired of typing flex properties (as they are all so damn long and you often require more then one or two).. in the same way as I quickly got tired of writing `git push origin head`. I figured ZSH ships with a set of alias' for git, why not create some shorthand for flex and other CSS properties I end up writing hundered of times a day.<br><br>
So far I have found it useful and not ran into any massive issues. The biggest being when people first look at it and say _wtf is that_ to which I point them at this repo and the definiton of an *abbreviation*. Once you start writing like this it is hard to stop.

# Further Expreiments
I would like to run some tests to see how using `@extend [],[]...` has an effect on compiled stylesheet size. My guess would be that as long as the average selector length is less than the average property/value length then there might actually be gains to be made. For example:<br><br>

```
.a { @extend [jcsb]; }
.b { @extend [jcsb]; }
```

Would get compiled to:

```
.a,
.b {
  justify-content: space-between;
}
```

Which is much smaller than:

```
.a { justify-content: space-between; }
.b { justify-content: space-between; }
```
