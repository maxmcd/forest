Every program is a tree of logic. 

Think of a python module. Top level methods, all with an input and an output. 

You jump from different logical viewpoints. If you start with a script that is supposed to scrape a website. You define a few globals, maybe import a few methods. As you're writing the imports to the file you are in the scope of the "file". You define a function you are promted to define the inputs and outputs and provide examples. If the function is:

```python
def reverse(string):
    return string[::-1] 
```

You would be promted to define an example for the input "string" and possibly an example return. 

The idea is that you don't view code as a file or modules. You view it as an atomic unit. Once this view is forced, we can overload the programmer with context. I'm editing this function, I get to see example values (think Bret Victor), the example values I give are automatically made into tests. Maybe testing is built into this IDE. I jump around my code, I don't jump from line to line I jump from function to function. 

You're climing up and down a stack trace, debugging with values at every moment.

Let's think of a more concrete example. 

This function takes a request and grabs a cookie out of it with a certain name. 

```javascript
let getNic = (req) => {
    var parts =  ("; " + req.headers.cookie).split("; session=");
    if (parts.length == 2) {
        let value = parts.pop()
        value = value.split(";")
        value = value.shift()
        value = value.trim()
        return sessions[value]
    };
}
```

The program would ask me for an example request value. Maybe the program had run before and it had a suggested value for me. I could pass: `{headers: {cookie: "session=foobar"}}` seems like that should work.

Then we pour on all that Bret Victor stuff:

```javascript
// req = {headers: {cookie: "session=foobar "}}
let getNic = (req) => {
    var parts =  ("; " + req.headers.cookie).split("; session="); 
    //=> ['', 'foobar ']
    if (parts.length == 2) {
        let value = parts.pop() // => 'foobar '
        value = value.split(";") // => ['foobar ']
        value = value.shift() // => 'foobar '
        value = value.trim() // => 'foobar'
        return sessions[value]
    };
}
```

`sessions` is a global here, so maybe we were prompted to place a value for that before. 

It seems that by viewing code only by its atomic units you can bake in testing, nice visuals that only have a home in playgrounds, and more meaningful help and information. You're also forced to write more modular code as this context loses value with much more complexity and you should be promted to break things down into smaller units. 

There could even be some internal DSL for library writers to define default example values for classes that are inhereted. Or somehow encode example values in their code. This could be particulairly useful for something like a function that processes an inbound http request as example input params might be quite complex. 